✅ [x]Paso 1: Instalar Docker Desktop para Mac
Ve a: https://www.docker.com/products/docker-desktop/

Descarga la versión para macOS (Apple Chip o Intel , windows, linux).

Instala el .dmg normalmente arrastrando a Aplicaciones.

Ábrelo y permite los permisos que te pida (requiere admin).

✅ [x]Paso 2: Verificar instalación
Corre los siguientes comandos:

```bash
docker --version
docker compose version
```
cd ~/.docker
cat config.

✅ Paso 3: Autenticación y credenciales
Abre Docker Desktop

Autentícate con tu cuenta de Docker Hub

Verifica con:
```bash
docker system info | grep Username
```

## Estado
✅ Docker Desktop instalado
✅ Autenticado con Docker Hub
⚠️ Warning por seccomp documentado (ver sección 04)
