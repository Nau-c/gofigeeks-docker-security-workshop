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


🔐 Habilitar userns-remap en Docker
📂 Carpeta: 02-daemon-security
🎯 Objetivo: hacer que los contenedores no se ejecuten como root directamente sobre el host, sino como un usuario mapeado sin privilegios.
⏱ Tiempo estimado: 10-15 min


✅ Procedimiento en sistemas Linux
Abre o crea el archivo del daemon:

sudo nano /etc/docker/daemon.json

## userns-remap

### Estado

❌ No aplicable directamente en Docker Desktop para macOS.

### ¿Por qué?

Docker Desktop corre sobre una VM, por lo tanto no expone directamente `/etc/docker/daemon.json` del host real. La configuración de `userns-remap` requiere un demonio Docker corriendo directamente sobre Linux.

### Procedimiento en Linux (para práctica):

```json
{
  "userns-remap": "default"
}

Requiere reinicio de daemon y se crea entorno aislado para imágenes. Protege contra contenedores ejecutándose como root en el host.

🔐 Acceso remoto seguro al daemon Docker
📂 Carpeta: 02-daemon-security
🎯 Objetivo: entender y, si se desea, habilitar acceso remoto al Docker daemon mediante SSH o TLS
⏱ Estimado: 15 minutos

## Acceso remoto seguro

En entornos Linux reales se recomienda acceso vía SSH o TLS usando contextos Docker.

### Simulación vía SSH con Docker context:

```bash
docker context create remote-host --docker "host=ssh://usuario@host"
docker context use remote-host
docker ps

Si tienes acceso a un servidor Linux (o WSL con Ubuntu), puedes hacer lo siguiente:

✅ Paso 1: Generar claves SSH
bash
Copiar
Editar
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_docker_remote
✅ Paso 2: Copiar clave pública al host remoto
bash
Copiar
Editar
ssh-copy-id -i ~/.ssh/id_docker_remote.pub usuario@host-remoto
✅ Paso 3: Crear nuevo contexto Docker
bash
Copiar
Editar
docker context create remote-host \
  --docker "host=ssh://usuario@host-remoto"
Verifica que existe:

bash
Copiar
Editar
docker context ls
✅ Paso 4: Usar el nuevo contexto
bash
Copiar
Editar
docker context use remote-host
docker ps
Esto ejecutará docker ps en el host remoto vía SSH. Super útil para administrar múltiples servidores Docker sin exponer puertos.

## Acceso remoto seguro

En entornos Linux reales se recomienda acceso vía SSH o TLS usando contextos Docker.

### Simulación vía SSH con Docker context:

```bash
docker context create remote-host --docker "host=ssh://usuario@host"
docker context use remote-host
docker ps
