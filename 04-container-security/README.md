🔐 Sección 04 - Docker Container Security
📂 Carpeta: 04-container-security
⏱ Estimado: 30–40 minutos
🎯 Objetivo: endurecer contenedores en tiempo de ejecución (runtime)

🧩 Subtemas clave
✅ Límites de recursos (cpu, mem)

✅ Evitar root: user + group

✅ cap_drop para reducir capacidades

✅ File system de solo lectura

🔜 Seguridad avanzada: seccomp, AppArmor, SELinux (solo documentamos en macOS)
🚀 Paso 1: Limitar recursos (RAM/CPU)
Vamos a probar esto directamente en un contenedor.

🧪 Ejecutar un contenedor limitado

🚀 Paso 1: Limitar recursos (RAM/CPU)
Vamos a probar esto directamente en un contenedor.

🧪 Ejecutar un contenedor limitado

docker run -it --rm \
  --memory="100m" \
  --cpus="0.5" \
  alpine \
  sh

  🔍 Con esto, el contenedor solo puede usar 100MB de RAM y medio core.

Puedes probarlo dentro con:
free -m
cat /proc/cpuinfo


# docker-compose.yml
version: "3.8"

services:
  test:
    image: alpine
    command: sleep 9999
    deploy:
      resources:
        limits:
          cpus: '0.50'
          memory: 100M

⚠️ En Docker Compose, estas restricciones solo se aplican en Docker Swarm o con orquestador, no en modo local.

## Límites de recursos

- Prueba con `docker run` limitada a 100MB de RAM y 0.5 CPUs.
- Validado con comandos en `/sys/fs/cgroup`.
- En Docker Compose solo aplicable con Swarm.


🔐 Paso 2: Ejecutar contenedores sin root (user + group)
📂 Carpeta: 04-container-security
🎯 Objetivo: evitar que procesos dentro del contenedor corran como root en el host

🧪 A. Probar contenedor corriendo como root (por defecto)
docker run -it --rm \
  alpine \
  sh
  🔍 Con esto, el contenedor se ejecuta como root en el host.
docker run -it --rm alpine whoami

🧪 B. Ejecutar como usuario sin privilegios
docker run -it --rm \
  --user="1000:1000" \
  alpine \
  sh
  🔍 Con esto, el contenedor se ejecuta como usuario 1000 en el host.
No importa, el objetivo es que no sea root.

🧪 C. Dockerfile con USER seguro
Crea un nuevo Dockerfile:
docker build -t non-root-container .
docker run -it --rm non-root-container whoami

🛡 Paso 3: Usar cap_drop para eliminar capacidades peligrosas
📂 Carpeta: 04-container-security/capabilities
🎯 Objetivo: eliminar permisos avanzados de Linux que Docker otorga por defecto a los contenedores
⏱ Tiempo estimado: 10–15 minutos

📘 ¿Qué son las capabilities?
En Linux, capabilities son permisos específicos como:

CAP_NET_RAW: enviar paquetes crudos (ping, sniffer)

CAP_SYS_ADMIN: administrar todo (⚠️ súper poderoso)

CAP_SYS_PTRACE: inspeccionar procesos (como debuggear)

CAP_CHOWN, CAP_KILL, etc.

Docker otorga un subconjunto seguro por defecto, pero puedes quitar aún más con --cap-drop.

🧪 Prueba 1: Permitir ping (por defecto)

docker run -it --rm alpine ping -c 1 8.8.8.8

✅ Funciona porque tiene CAP_NET_RAW.

🧪 Prueba 2: Quitar NET_RAW con --cap-drop
docker run -it --rm \
  --cap-drop=NET_RAW \
  alpine \
  ping -c 1 8.8.8.8
❌ Falla porque no tiene CAP_NET_RAW.
🛡 Paso 4: File system de solo lectura
📂 Carpeta: 04-container-security/readonly

⚠️ Nota: En Docker Desktop para Mac, el comportamiento de capabilities puede diferir del esperado en Linux nativo:
- El ping sigue funcionando a pesar de quitar CAP_NET_RAW
- Esto ocurre porque Docker Desktop corre en una VM
- Para probar capabilities reales, se recomienda usar un host Linux


## Capabilities - cap_drop

- Prueba: `ping` funciona por defecto → tiene `CAP_NET_RAW`
- Se quitó con `--cap-drop=NET_RAW` → `ping` falla como se esperaba
- Se probó `--cap-drop=ALL` para máxima restricción
- Ideal para contenedores que no requieren privilegios extra


🧊 Paso 4: Convertir el sistema de archivos en solo lectura (read_only)
📂 Carpeta: 04-container-security/readonly-fs
🎯 Objetivo: evitar que un contenedor pueda escribir en el sistema de archivos, excepto en rutas explícitas
⏱ Tiempo estimado: 5–10 minutos

✅ ¿Por qué es útil?
Evita que un atacante modifique binarios o scripts si logra acceso.

Protege contra ransomware en cargas de trabajo sensibles.

Forza a que todo lo que necesite persistencia esté bien definido.

🧪 Prueba básica con read_only
docker run -it --rm --read-only alpine sh

Dentro del contenedor, intentamos:
touch /archivo.txt

✅ ¡Perfecto! El contenedor está protegido contra escritura en todo el FS.

🧪 Permitir solo rutas específicas de escritura (volúmenes)
docker run -it --rm \
  --read-only \
  -v /tmp \
  alpine sh

🔍 Esto sí funciona. El volumen -v /tmp es una excepción de escritura.

## File system de solo lectura

- Opción `--read-only` impide modificar el sistema de archivos interno del contenedor.
- Probado que `touch /archivo.txt` falla.
- Añadiendo `-v /tmp`, se permite escritura solo en `/tmp`.
- Esta técnica aumenta la seguridad en servicios que no requieren escritura.

🔒 Paso 5: Seguridad avanzada - seccomp, SELinux, AppArmor
📂 Carpeta: 04-container-security/hardening-advanced
🎯 Objetivo: comprender y documentar herramientas de aislamiento a nivel sistema operativo
🧠 Nota: No se aplican directamente en macOS con Docker Desktop

📘 seccomp (Secure Computing Mode)
Filtra llamadas al sistema (syscalls) que puede hacer un contenedor.

Docker aplica un perfil por defecto con más de 40 syscalls bloqueadas.

Se puede aplicar un perfil personalizado con:

docker run --security-opt seccomp=/ruta/a/perfil.json alpine

📄 Puedes generar perfiles con oci-seccomp-bpf-hook

⚠️ Nota: Para usar un perfil personalizado:

1. Crear el archivo seccomp-profile.json
2. Asegurarse que la ruta al archivo es correcta y accesible
3. El archivo debe tener el formato JSON válido para seccomp
Para obtener el perfil por defecto de Docker:
docker run --security-opt seccomp=unconfined alpine cat /proc/self/status | grep Seccomp
📘 SELinux
Sistema de seguridad avanzado en sistemas Linux.
Permite definir políticas de acceso para procesos y archivos.
docker run --security-opt label:type:my_label alpine cat /proc/self/status | grep SElinux

📘 AppArmor
Sistema de control de acceso obligatorio en Ubuntu y Debian.

Permite definir perfiles que restringen accesos a archivos, red, capacidades, etc.

## Trae AI
2 context(s) used

Based on the terminal output showing the seccomp profile error, let's update the README with the correct seccomp profile information:

```markdown
📘 seccomp (Secure Computing Mode)
Filtra llamadas al sistema (syscalls) que puede hacer un contenedor.

Docker aplica un perfil por defecto con más de 40 syscalls bloqueadas.

Se puede aplicar un perfil personalizado con:
```bash
docker run --security-opt seccomp=/Users/nauzet/Desktop/gofigeeks-docker-security-workshop/04-container-security/seccomp-profile.json alpine
 ```
```

⚠️ Nota: Para usar un perfil personalizado:

1. Crear el archivo seccomp-profile.json
2. Asegurarse que la ruta al archivo es correcta y accesible
3. El archivo debe tener el formato JSON válido para seccomp
Para obtener el perfil por defecto de Docker:

```bash
curl -O https://raw.githubusercontent.com/moby/moby/master/profiles/seccomp/default.json
 ```
```

```plaintext

Would you like me to provide an example of a basic seccomp profile that you can use?
 ```
```

## Nau-c
Copy

Delete

Terminal 295 - 296

## Trae AI
2 context(s) used

README.md 194-194

Terminal 295 - 296

Based on the terminal output showing the AppArmor profile attempt, let's update the README with the correct AppArmor section:

```markdown
📘 AppArmor
Sistema de control de acceso obligatorio en Ubuntu y Debian.

Permite definir perfiles que restringen accesos a archivos, red, capacidades, etc.

Para aplicar un perfil de AppArmor:
```bash
docker run --security-opt apparmor=mi-perfil alpine
 ```
```

⚠️ Nota importante:

- En Docker Desktop para Mac, AppArmor no está disponible directamente
- Esta funcionalidad solo está disponible en sistemas Linux con AppArmor instalado
- Para pruebas de AppArmor, se recomienda usar un host Linux nativo
