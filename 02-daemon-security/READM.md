🚀 Sección 2: Docker Daemon Security
📁 Carpeta: 02-daemon-security
⏱ Tiempo estimado: 25–30 min

🧩 Objetivos clave
Activar Rootless mode

Deshabilitar ICC (Inter-Container Communication)

Configurar userns-remap

Comprobar Acceso remoto seguro (solo si aplica)


✅ ¿Qué hacemos entonces?
📌 Lo importante es que entiendes el concepto y las limitaciones de macOS.

📝 En tu README.md de 02-daemon-security, puedes documentar:

## Rootless Mode en macOS

- `docker info | grep -i rootless` no devuelve resultado.
- Esto es esperado: Docker Desktop en macOS corre sobre una VM interna.
- En macOS no se puede ejecutar `dockerd-rootless.sh` directamente como en Linux.
- El aislamiento del host ya lo proporciona la VM de Docker Desktop.
