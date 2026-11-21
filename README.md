# vscode-workspace-config

Configuración compartida y modular de Visual Studio Code para estandarizar entornos de desarrollo en múltiples lenguajes y herramientas.

> **Nota:** El repositorio contiene actualmente las plantillas de configuración dentro del directorio `vscode/`. La implementación de *dev containers* y scripts de aprovisionamiento local están proyectados para versiones futuras.

---

## Contenido de la configuración (`vscode/`)

Los archivos dentro del directorio `vscode/` definen ajustes de editor, extensiones recomendadas, perfiles de depuración y tareas automatizadas:

### 1. `settings.json` (Ajustes del editor y herramientas)
Configuración optimizada para consistencia de código y herramientas por stack:
- **General:** Formateo automático al guardar (`editor.formatOnSave`), organización y corrección de imports al guardar (`source.organizeImports`, `source.fixAll`), coloreado de pares de corchetes, autoguardado al cambiar foco, recorte de espacios al final de línea e inserción de salto de línea final.
- **Rust:** Formateador `rust-lang.rust-analyzer`, verificación en segundo plano con `clippy`, resaltado semántico y habilitación de macros procedurales y scripts de compilación de Cargo.
- **Python:** Formateador `charliermarsh.ruff` con corrección y organización de imports, servidor de lenguaje `Pylance` y modo de análisis de tipos `basic`.
- **JavaScript / TypeScript / React:** Formateador `esbenp.prettier-vscode` y actualización automática de rutas de importación al mover archivos (`updateImportsOnFileMove: "always"`).
- **Java:** Formateador predeterminado `redhat.java`.
- **SQL:** Formateo mediante `SQLTools` con sangría de 2 espacios, 2 líneas de separación entre consultas y palabras reservadas en mayúsculas (`reservedWordCase: "upper"`).

### 2. `extensions.json` (Extensiones recomendadas)
Extensiones esenciales organizadas por categoría:
- **General:** `editorconfig.editorconfig`, `christian-kohler.path-intellisense`, `shardulm94.trailing-spaces`, `oderwat.indent-rainbow`.
- **Rust:** `rust-lang.rust-analyzer`, `vadimcn.vscode-lldb` (CodeLLDB), `tamasfe.even-better-toml`.
- **Python:** `ms-python.python`, `ms-python.vscode-pylance`, `ms-python.debugpy`, `charliermarsh.ruff`.
- **Node / JS / TS / React:** `dbaeumer.vscode-eslint`, `esbenp.prettier-vscode`, `ms-vscode.vscode-typescript-next`, `dsznajder.es7-react-js-snippets`.
- **Java / Spring Boot:** `vscjava.vscode-java-pack`, `vscjava.vscode-spring-boot-dashboard`, `vmware.vscode-spring-boot`.
- **SQL:** `mtxr.sqltools`, `mtxr.sqltools-driver-pg`.
- **Docker:** `ms-azuretools.vscode-docker`.

### 3. `launch.json` (Perfiles de depuración)
Perfiles de ejecución y depuración preconfigurados:
- **Debug Rust** (`lldb`): Compila el proyecto con `cargo build` e inicia el depurador CodeLLDB.
- **Python: FastAPI** (`debugpy`): Inicia servidores FastAPI con `uvicorn main:app --reload` y soporte Jinja.
- **Python: Flask** (`debugpy`): Inicia servidores Flask (`flask run --no-debugger --no-reload`) con `FLASK_APP=app.py` y `FLASK_DEBUG=1`.
- **Node: Run Index** (`node`): Ejecuta `${workspaceFolder}/index.js` en la terminal integrada con reinicio automático.
- **Launch Chrome against localhost** (`chrome`): Conecta la depuración de Chrome contra `http://localhost:5173` (puerto por defecto de Vite).
- **Spring Boot Launch** (`java`): Perfil de inicio para aplicaciones Spring Boot.

### 4. `tasks.json` (Tareas automatizadas)
Tareas integradas ejecutables desde la terminal de VS Code:
- **Rust:** `cargo build` (build), `cargo run` (build), `cargo test` (test) con problemMatcher `$rustc`.
- **Node.js / JS / TS:** `npm install`, `npm run dev` (ejecución en fondo con matcher `$tsc-watch`), `npm test` (test).
- **Python:** `pytest` (test), `pip install requirements` (`pip install -r requirements.txt`).

---

## Cómo usar esta configuración

### Opción 1: Copiar a un proyecto específico
Copia el directorio `vscode/` renombrándolo a `.vscode/` en la raíz de tu proyecto:

```bash
cp -r vscode/ /ruta/a/tu/proyecto/.vscode
```

O copia individualmente solo los archivos que necesites para tu stack (por ejemplo, `settings.json` y `extensions.json`).

### Opción 2: Integrar en la configuración de usuario de VS Code
Para aplicar los ajustes de forma global a todos tus espacios de trabajo:
1. Abre la paleta de comandos (`Ctrl+Shift+P` en Windows/Linux o `Cmd+Shift+P` en macOS).
2. Selecciona **Preferences: Open User Settings (JSON)**.
3. Copia las secciones deseadas de `vscode/settings.json` a tu archivo de configuración de usuario.
