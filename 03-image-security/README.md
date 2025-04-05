🐳 Sección 03 - Docker Image Security
📂 Carpeta: 03-image-security
⏱ Tiempo estimado: 30 minutos
🔐 Objetivo: Asegurar imágenes Docker desde la construcción (Dockerfile) hasta su análisis estático

🧩 Subtemas de esta sección
✅ Permisos en imágenes

✅ ENTRYPOINT vs CMD

✅ docker scan para escaneo de vulnerabilidades

✅ Linter: hadolint para Dockerfile

✅ Otras buenas prácticas

🚦 Paso 1: Crear un proyecto de imagen insegura
Vamos a crear una imagen sencilla para experimentar los conceptos.

📄 Crea un archivo Dockerfile dentro de 03-image-security:
# Dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY app.py .

# Este entrypoint será difícil de sobrescribir
ENTRYPOINT ["python3"]
CMD ["app.py"]


✅ 2. Reconstruye la imagen desde cero (sin cache)
docker build --no-cache -t insecure-image .

✅ 3. Ejecuta con modo interactivo para ver si el contenedor arranca correctamente

docker run -it insecure-image

🧪 4. Verifica qué hay dentro del contenedor (debug)
Si aún así no se muestra nada, entra al contenedor manualmente:
docker run -it --entrypoint sh insecure-image

🔍 Paso 3: Escaneo de vulnerabilidades
⚠️ Nota: `docker scan` ha sido descontinuado. Alternativas recomendadas:

1. Snyk Container
```bash
snyk container test insecure-image
```
docker scan usa Snyk por detrás para detectar vulnerabilidades en la imagen.

docker scout quickview insecure-image


docker scan descontinuado

✅ 1. Escanea la imagen creada


- Imagen escaneada: `insecure-image`
- Herramienta: `docker scan` (Snyk)
- Vulnerabilidades detectadas: X (resumen del output)
- Notas: Se recomienda minimizar capas y actualizar `FROM` con imágenes oficiales y actualizadas.

🧼 Paso 4: Analizar Dockerfile con hadolint
📂 Carpeta: 03-image-security/linting
🎯 Objetivo: Validar buenas prácticas en tu Dockerfile y evitar errores comunes de seguridad/sintaxis.

✅ 1. Instalar hadolint (opciones)
Opción A: Usar contenedor hadolint directamente (recomendada en tu caso)
```bash
docker run --rm -i hadolint/hadolint < Dockerfile
```
Opción B: Instalar hadolint en tu sistema local
```bash
# En macOS
brew install hadolint
# En Linux (Debian/Ubuntu)
sudo apt-get install hadolint
# En Linux (CentOS/RHEL)
sudo yum install hadolint
``` 

📄 Análisis línea a línea
dockerfile
Copiar
Editar
FROM python:3.11-slim
✅ Usas una imagen oficial y slim → buena práctica.
🔍 Extra tip: podrías especificar el hash (digest) para mayor seguridad en producción.

dockerfile
Copiar
Editar
WORKDIR /app
✅ Establece el directorio de trabajo dentro del contenedor. hadolint te habría advertido si esto faltaba.

dockerfile
Copiar
Editar
COPY app.py .
✅ Copia el archivo de tu proyecto al contenedor. Si tuvieras otros archivos, considera un .dockerignore para evitar copiar cosas innecesarias (como .git).

dockerfile
Copiar
Editar
ENTRYPOINT ["python3"]
CMD ["app.py"]
✅ Patrón ideal: ENTRYPOINT como el binario fijo y CMD como argumentos.
📌 Esto te da flexibilidad: puedes sobrescribir CMD fácilmente pero no ENTRYPOINT (a menos que lo forces con --entrypoint).

✅ Resultado: ¡Dockerfile 10/10!
No solo hadolint lo aprueba, también está bien pensado desde el punto de vista de seguridad, claridad y portabilidad.

