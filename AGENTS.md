# Contexto para Agentes — vscode-workspace-config

Repositorio de configuraciones compartidas y reutilizables de Visual Studio Code para estandarizar entornos de desarrollo multi-lenguaje.

## Propósito del proyecto
Centralizar y distribuir plantillas de configuración para VS Code (`settings.json`, `extensions.json`, `launch.json`, `tasks.json`) para proyectos en Rust, Python, JavaScript/TypeScript/React, Java/Spring Boot, SQL y Docker.

## Stack
- **Formato:** JSON con comentarios (JSONC).
- **Herramientas cubiertas:** VS Code, Rust Analyzer / Clippy, Python (Ruff, Pylance, debugpy), Prettier / ESLint, Red Hat Java / Spring Boot, SQLTools, Docker.
- **Entorno de verificación:** Node.js (v20+) o Python 3 para pruebas y validación sintáctica.

## Estructura de carpetas
```
vscode-workspace-config/
├── .gitignore             # Exclusiones de Git (SO, temporales, cachés, .env)
├── AGENTS.md              # Guía de contexto y reglas para agentes
├── README.md              # Documentación para desarrolladores y modo de uso
└── vscode/                # Plantillas modulares de configuración
    ├── extensions.json    # Extensiones recomendadas agrupadas por stack
    ├── launch.json        # Configuraciones de depuración (Rust, Python, Node, Chrome, Java)
    ├── settings.json      # Ajustes globales y específicos por lenguaje del editor
    └── tasks.json         # Tareas de compilación, ejecución y test (cargo, npm, pytest)
```

## Instalación, uso y verificación

### Aplicación en un proyecto destino
Copiar el directorio `vscode/` como `.vscode/` en la raíz del proyecto destino:
```bash
cp -r vscode/ /ruta/a/proyecto/.vscode
```

### Verificación y linting de configuración (JSONC)
Comprobar que todos los archivos JSONC en `vscode/` mantengan sintaxis válida:
```bash
node -e "
const fs = require('fs');
const files = ['vscode/extensions.json', 'vscode/launch.json', 'vscode/settings.json', 'vscode/tasks.json'];
function stripJsonComments(str) {
  let inStr = false, quote = '', esc = false, out = '';
  for (let i = 0; i < str.length; i++) {
    const c = str[i];
    if (inStr) {
      out += c;
      if (esc) esc = false;
      else if (c === '\\\\') esc = true;
      else if (c === quote) inStr = false;
    } else {
      if (c === '\"' || c === '\'') { inStr = true; quote = c; out += c; }
      else if (c === '/' && str[i+1] === '/') { while (i < str.length && str[i] !== '\n') i++; out += '\n'; }
      else if (c === '/' && str[i+1] === '*') { i += 2; while (i < str.length && !(str[i] === '*' && str[i+1] === '/')) i++; i++; }
      else out += c;
    }
  }
  return out;
}
files.forEach(f => {
  try {
    JSON.parse(stripJsonComments(fs.readFileSync(f, 'utf8')).replace(/,(\s*[\]}])/g, '$1'));
    console.log(f + ': OK');
  } catch(e) {
    console.error(f + ': ERROR - ' + e.message);
    process.exit(1);
  }
});
"
```

Verificar formato y espacios en blanco:
```bash
python3 -c "
import glob, sys
for p in glob.glob('vscode/*.json') + ['README.md', 'AGENTS.md']:
    with open(p, encoding='utf-8') as f:
        lines = f.readlines()
    if any(l.rstrip('\r\n') != l.rstrip() for l in lines):
        print(f'Error: espacios al final en {p}')
        sys.exit(1)
    if lines and not lines[-1].endswith('\n'):
        print(f'Error: falta salto de línea final en {p}')
        sys.exit(1)
print('Verificación de formato: OK')
"
```

## Convenciones de código
- **Sintaxis:** JSONC estándar compatible con VS Code.
- **Indentación:** 2 espacios, sin tabulaciones.
- **Comentarios:** Usar `// --- SECCIÓN ---` para delimitar tecnologías o lenguajes.
- **Final de archivo:** Siempre incluir salto de línea final (`insertFinalNewline: true`).
- **Espacios:** Sin espacios en blanco al final de línea (`trimTrailingWhitespace: true`).

## Reglas de seguridad
- **Secretos:** Prohibido escribir credenciales, API keys, contraseñas o tokens en `launch.json` o `tasks.json`.
- **Variables de entorno:** Usar variables de entorno (`${env:NOMBRE_VAR}`) o archivos `.env` locales.
- **Archivos sensibles:** `.env`, `.env.*`, `*.pem`, `*.key` y credenciales nunca se versionan en Git.

## Flujo Git
- **Ramas:** Ramas de trabajo nombradas `dev/*` orientadas hacia `develop-AGENTS`.
- **Restricción:** Prohibido merge o commit directo a `main` o `develop`.
- **Mensajes de commit:** Conventional Commits en inglés (`chore:`, `fix:`, `docs:`, `refactor:`).
- **Prohibido:** `git push`, tocar remotos, rebase de historia pública o `commit --amend` de commits previos.
