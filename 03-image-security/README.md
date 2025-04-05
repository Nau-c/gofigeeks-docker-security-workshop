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