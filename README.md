# vscode-workspace-config

Configuración compartida y modular de Visual Studio Code para estandarizar entornos de desarrollo en múltiples lenguajes y herramientas.

> **Nota importante:** El *dev container* y el script de instalación local mencionados anteriormente no se encuentran disponibles en este repositorio por el momento. El repositorio contiene únicamente los archivos de configuración dentro de la carpeta `vscode/`.

---

## Contenido de la configuración (`vscode/`)

Los archivos dentro del directorio `vscode/` definen ajustes de editor, extensiones recomendadas, tareas automatizadas y perfiles de depuración.

### 1. `settings.json` (Ajustes del editor y herramientas)

Define el comportamiento general del editor y ajustes específicos para los lenguajes cubiertos:

* **Configuración general del editor:**
  * Formateo automático al guardar (`editor.formatOnSave: true`).
  * Organización de importaciones y correcciones automáticas al guardar (`editor.codeActionsOnSave` con `source.fixAll` y `source.organizeImports`).
  * Coloreado de pares de corchetes y guías activas (`editor.bracketPairColorization.enabled: true`, `editor.guides.bracketPairs: "active"`).
  * Autoguardado al perder el foco (`files.autoSave: "onFocusChange"`).
  * Eliminación de espacios en blanco al final de línea (`files.trimTrailingWhitespace: true`).
  * Inserción automática de salto de línea al final del archivo (`files.insertFinalNewline: true`).

* **Lenguajes cubiertos y ajustes específicos:**
  * **Rust**: Formateador predeterminado `rust-lang.rust-analyzer`, resaltado semántico activado, comando de comprobación configurado a `clippy` (`rust-analyzer.check.command: "clippy"`), y habilitación de scripts de compilación (`cargo.buildScripts.enable`) y macros procedurales (`procMacro.enable`).
  * **Python**: Formateador predeterminado `charliermarsh.ruff` con corrección y ordenamiento de imports al guardar, servidor de lenguaje `Pylance` y modo de análisis de tipos `basic`.
  * **JavaScript / TypeScript / React** (`[javascript]`, `[typescript]`, `[typescriptreact]`): Formateador predeterminado `esbenp.prettier-vscode` y actualización automática de rutas de importación al mover archivos (`updateImportsOnFileMove.enabled: "always"`).
  * **Java**: Formateador predeterminado `redhat.java`.
  * **SQL**: Formateo mediante SQLTools (`sqltools.format`) con sangría de 2 espacios, 2 líneas entre consultas y palabras reservadas en mayúsculas (`reservedWordCase: "upper"`).

---

### 2. `extensions.json` (Extensiones recomendadas)

Agrupa las extensiones recomendadas según su categoría:

* **General:**
  * `editorconfig.editorconfig`: Soporte para consistencia de estilo con EditorConfig.
  * `christian-kohler.path-intellisense`: Autocompletado inteligente de rutas de archivos.
  * `shardulm94.trailing-spaces`: Detección y resaltado de espacios en blanco sobrantes.
  * `oderwat.indent-rainbow`: Coloreado visual de niveles de indentación.
* **Rust:**
  * `rust-lang.rust-analyzer`: Servidor de lenguaje y soporte para Rust.
  * `vadimcn.vscode-lldb`: Depurador nativo basado en LLDB (CodeLLDB).
  * `tamasfe.even-better-toml`: Soporte mejorado para archivos TOML.
* **Python:**
  * `ms-python.python`: Soporte principal para Python.
  * `ms-python.vscode-pylance`: Servidor de lenguaje y análisis estático con Pylance.
  * `ms-python.debugpy`: Soporte de depuración para Python.
  * `charliermarsh.ruff`: Integración con el linter y formateador Ruff.
* **Node / JS / TS / React:**
  * `dbaeumer.vscode-eslint`: Integración con el linter ESLint.
  * `esbenp.prettier-vscode`: Formateador de código con Prettier.
  * `ms-vscode.vscode-typescript-next`: Soporte para versiones recientes del compilador de TypeScript.
  * `dsznajder.es7-react-js-snippets`: Snippets de código para React, Redux y JavaScript.
* **Java / Spring Boot:**
  * `vscjava.vscode-java-pack`: Paquete de extensiones esenciales para desarrollo con Java.
  * `vscjava.vscode-spring-boot-dashboard`: Panel interactivo para proyectos Spring Boot.
  * `vmware.vscode-spring-boot`: Herramientas de soporte y navegación para Spring Boot.
* **SQL:**
  * `mtxr.sqltools`: Cliente y gestor de bases de datos para VS Code.
  * `mtxr.sqltools-driver-pg`: Controlador PostgreSQL para SQLTools.
* **Docker:**
  * `ms-azuretools.vscode-docker`: Gestión y soporte de Dockerfiles, contenedores e imágenes.

---

### 3. `launch.json` (Configuraciones de depuración)

Define los perfiles de ejecución y depuración configurados:

* **Debug Rust** (`lldb`): Compila el proyecto con `cargo build` e inicia la depuración mediante CodeLLDB.
* **Python: FastAPI** (`debugpy`): Inicia servidores FastAPI con `uvicorn` (`main:app --reload`) y soporte Jinja activado.
* **Python: Flask** (`debugpy`): Inicia servidores Flask (`flask run --no-debugger --no-reload`) con variables de entorno `FLASK_APP=app.py` y `FLASK_DEBUG=1`.
* **Node: Run Index** (`node`): Ejecuta `${workspaceFolder}/index.js` en la terminal integrada con reinicio automático (`restart: true`).
* **Launch Chrome against localhost** (`chrome`): Conecta la depuración de Chrome contra `http://localhost:5173` (puerto predeterminado de Vite).
* **Spring Boot Launch** (`java`): Perfil de inicio para aplicaciones Spring Boot en Java.

---

### 4. `tasks.json` (Tareas automatizadas)

Define tareas de compilación, ejecución y pruebas ejecutables desde la terminal de VS Code:

* **Rust:**
  * `cargo build`: Compila el proyecto (grupo `build`).
  * `cargo run`: Ejecuta el proyecto (grupo `build`).
  * `cargo test`: Ejecuta las pruebas del proyecto (grupo `test`).
* **Node.js / JS / TS:**
  * `npm install`: Instala las dependencias del proyecto.
  * `npm run dev`: Ejecuta el entorno de desarrollo en segundo plano (`isBackground: true`) con vigilante `$tsc-watch` (grupo `build`).
  * `npm test`: Ejecuta las pruebas mediante npm (grupo `test`).
* **Python:**
  * `pytest`: Ejecuta las pruebas del proyecto mediante pytest (grupo `test`).
  * `pip install requirements`: Instala dependencias desde `requirements.txt`.

---

## Cómo usar esta configuración

Existen dos formas principales de utilizar estas configuraciones:

### 1. Copiar `vscode/` como `.vscode/` en la raíz de un proyecto

Para aplicar esta configuración a un proyecto en específico, copia la carpeta `vscode/` renombrándola como `.vscode/` en la raíz del proyecto objetivo:

```bash
# Ejemplo copiando el directorio completo:
cp -r vscode/ /ruta/a/tu/proyecto/.vscode
```

También puedes copiar únicamente los archivos que requieras para tu proyecto (por ejemplo, `settings.json` y `extensions.json`).

### 2. Copiar configuraciones a los ajustes de usuario de VS Code

Si deseas que las directivas de `settings.json` se apliquen de forma global a todos tus espacios de trabajo:

1. Abre la paleta de comandos en VS Code (`Ctrl+Shift+P` en Linux/Windows o `Cmd+Shift+P` en macOS).
2. Busca y selecciona **Preferences: Open User Settings (JSON)**.
3. Copia y añade las opciones deseadas de `vscode/settings.json` en tu archivo de configuración de usuario.
