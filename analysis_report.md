# Root Cause Analyse: MCPHub Path-Problem bei Service-Start

## 🜄 Ziel 🜄
Detaillierte Analyse des Issues, dass MCPHub beim Start als Service keine Paths erkennt, wodurch MCP Server nicht starten können.

## 🜄 Kontext 🜄
MCPHub läuft normal als Entwicklungsprozess, aber als Docker-Service (Container) können MCP Server nicht initialisiert werden. Das Problem betrifft Pfad-Auflösung für MCP Server Konfigurationen.

## 🜄 Verantwortung 🜄
Analyse durchgeführt von System-Architekt. Root Cause in mcpService.ts identifiziert.

## 🜄 Root Cause Analyse 🜄

### Problem-Beschreibung
- **Symptom**: MCP Server starten nicht beim Service-Start
- **Umgebung**: Docker Container (Service-Modus)
- **Konfiguration**: mcp_settings.json mit absoluten Host-Pfaden

### Code-Analyse

#### 1. Konfigurations-Laden (`src/config/index.ts`)
```typescript
export const getConfigFilePath = (filename: string, description = 'Configuration'): string => {
  const envPath = process.env.MCPHUB_SETTING_PATH;
  const potentialPaths = [
    ...(envPath ? [envPath] : []),
    path.resolve(process.cwd(), filename),  // process.cwd() = /app im Container
    // ...
  ];
  // Returns default path if not found
  return defaultPath;
};
```
- Config wird korrekt von `process.cwd()` (Container: /app) geladen.

#### 2. MCP Server Initialisierung (`src/services/mcpService.ts`)
```typescript
const createTransportFromConfig = (name: string, conf: ServerConfig): any => {
  // ...
  } else if (conf.command && conf.args) {
    // Stdio transport
    const env: Record<string, string> = {
      ...(process.env as Record<string, string>),
      ...replaceEnvVars(conf.env || {}),
    };
    env['PATH'] = expandEnvVars(process.env.PATH as string) || '';

    transport = new StdioClientTransport({
      cwd: os.homedir(),  // PROBLEM: os.homedir() = /root im Container
      command: conf.command,
      args: replaceEnvVars(conf.args) as string[],
      env: env,
      stderr: 'pipe',
    });
  }
  // ...
};
```
- **Root Cause**: `cwd: os.homedir()` setzt Working Directory auf `/root` im Container.
- Aber Config-Pfade sind absolute Host-Pfade wie `/home/auctor/srv/ct-serena/serena`.

#### 3. Beispiel-Konfiguration (`mcp_settings.json`)
```json
{
  "serena": {
    "command": "uv",
    "args": ["run", "--directory", "/home/auctor/srv/ct-serena", "serena", "start-mcp-server"]
  },
  "ct-zen": {
    "command": "python3",
    "args": ["/home/auctor/srv/ct-zen/server.py"]
  }
}
```
- Pfade sind absolute Host-Pfade, nicht verfügbar im Container.

### Runtime-Unterschiede

#### Normaler Start (Entwicklung)
- `process.cwd()` = Projekt-Verzeichnis (z.B. `/home/user/mcphub`)
- `os.homedir()` = User Home (z.B. `/home/user`)
- Config-Pfade relativ zum Projekt oder absolut verfügbar

#### Service-Start (Docker)
- `process.cwd()` = `/app` (Container WORKDIR)
- `os.homedir()` = `/root` (Container Root)
- Config-Pfade sind Host-Pfade, nicht gemountet im Container

### Architektur-Analyse

#### MCP Server Architektur
1. **Konfiguration**: JSON-Datei mit Server-Definitionen
2. **Transport-Erstellung**: Stdio/SSE/HTTP basierend auf Config
3. **Client-Verbindung**: MCP SDK Client verbindet zu Servern
4. **Service-Registrierung**: Server werden in Hub registriert

#### Pfad-Abhängigkeiten
- **Config-Datei**: Relativ zu `process.cwd()` (funktioniert)
- **Server-Commands**: Absolut oder relativ zu `cwd` (problem)
- **Working Directory**: Beeinflusst Pfad-Auflösung für relative Commands

## 🜄 Root Cause Identifikation 🜄

**Primäre Ursache**: Falsche `cwd`-Einstellung in `createTransportFromConfig()`.
- `os.homedir()` = `/root` im Container
- Aber Commands erwarten Pfade relativ zum Config-Verzeichnis oder absoluten Pfaden
- Host-Pfade sind im Container nicht verfügbar

**Sekundäre Ursache**: Konfiguration verwendet absolute Host-Pfade statt container-spezifischer Pfade.

## 🜄 Lösungsansätze 🜄

### Lösung 1: cwd-Korrektur (Empfohlen)
```typescript
// In mcpService.ts, createTransportFromConfig
transport = new StdioClientTransport({
  cwd: process.cwd(),  // Ändern von os.homedir() zu process.cwd()
  // ... rest unchanged
});
```
- **Vorteile**: Minimal invasiv, behebt Kernproblem
- **Kompatibilität**: Funktioniert für beide Modi (Entwicklung/Service)

### Lösung 2: Relative Pfade in Config
- Config-Pfade relativ zum Container machen
- Volumes für externe Pfade mounten
- **Nachteil**: Erfordert Config-Änderungen pro Deployment

### Lösung 3: Environment-spezifische Config
- Verschiedene Configs für Entwicklung vs. Production
- Production verwendet Container-Pfade
- **Nachteil**: Komplexere Deployment-Konfiguration

## 🜄 Risiken 🜄

### Lösung 1 Risiken
- Potenzielle Seiteneffekte bei relativen Pfaden in Entwicklung
- Commands könnten andere Working Directory erwarten

### Allgemeine Risiken
- Pfad-Injection möglich bei unsicherer Config
- Performance-Impact bei falschen Pfaden
- Debugging erschwert bei Container-Isolation

## 🜄 Empfehlung 🜄

**Lösung 1 implementieren**: `cwd: process.cwd()` in `createTransportFromConfig`.
- Behebt das Problem minimal invasiv
- Behält bestehende Architektur bei
- Funktioniert für beide Deployment-Modi

**Zusätzlich**: Dokumentation für Container-Deployment verbessern, Volume-Mounting empfehlen.