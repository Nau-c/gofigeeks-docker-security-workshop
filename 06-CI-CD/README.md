🚀 Último Paso: CI/CD para Seguridad Docker
📂 Carpeta sugerida: ci-cd/
🎯 Objetivo: Crear un pipeline que:

✅ Construya una imagen Docker

✅ Analice el Dockerfile con hadolint

✅ Genere SBOM con syft (simulado si no se ejecuta)

✅ Escanee la imagen con trivy (simulado o documentado)

✅ (Opcional) Firme con cosign

✅ Haga push al Docker Hub (si estás autenticado)

🧪 Opción: GitHub Actions (sencillo y gratuito)
Creamos un archivo:


📦 CI/CD - Seguridad Docker con GitHub Actions
Este pipeline CI/CD realiza un flujo completo de seguridad sobre imágenes Docker utilizando:

Build & Push a Docker Hub

Firma con Cosign

Uso de GitHub Actions y Secrets seguros


📄 Ruta del pipeline

.github/workflows/docker-security.yml

🔐 Secrets requeridos

Secret name                  Description
DOCKER_USERNAME            Docker Hub username
DOCKER_PASSWORD            Docker Hub password
CO_SIGN_KEY                Cosign private key (optional)
CO_SIGN_PASSWORD           Cosign password (optional)
🔧 Configuración
Copia el contenido del archivo docker-security.yml en tu repositorio.
Configura los Secrets en GitHub:
DOCKER_USERNAME
DOCKER_PASSWORD
CO_SIGN_KEY (opcional)
CO_SIGN_PASSWORD (opcional)
🚀 Ejecución
Cada push al repositorio iniciará el pipeline de GitHub Actions.
🔍 Resultados
El pipeline mostrará los resultados de las pruebas de seguridad.
📝 Notas
Este pipeline es una guía básica. Asegúrate de ajustar las acciones y comandos según tus necesidades específicas.
Si tienes preguntas, no dudes en preguntar.


🧪 Acciones realizadas
1. 🔁 Clona el repo

2.🐳 Construye la imagen desde 03-image-security/

3. ☁️ Realiza docker push al Docker Hub (tag: ci-latest)

4. 🔐 Decodifica y guarda la clave Cosign

5. 🖊 Firma la imagen con cosign sign

6. ✅ Todo en GitHub Actions sin exponer credenciales

🧰 Stack usado

- GitHub Actions

- Docker Buildx

- Cosign

- Secrets seguros


Este pipeline garantiza  que:
- Cada push a main construye y publica una imagen firmada.
- Las firmas son verificables publicamente en Docker Hu
- Todo el proceso es seguro y reproducible y automatizado.


Tips:
- Puedes extender este pipeline para incluir más pruebas de seguridad.
- Asegúrate de que tu imagen Docker esté firmada con Cosign para garantizar la integridad.
- Utiliza GitHub Actions para automatizar el proceso de seguridad en tu pipelin

cosign verify --key cosign.pub tuusuario/insecure-image:ci-latest

