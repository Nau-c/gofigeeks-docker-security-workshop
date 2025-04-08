# gofigeeks-docker-security-workshop

🛠️ 🔐 ACCESO Y PREPARACIÓN

🔑 Conectar vía SSH

ssh -i /ruta/a/tu.pem ubuntu@IP_PUBLICA_EC2

🔹 Usa tu clave .pem (asegúrate de que tenga permisos 400)


🐳 DOCKER - Comandos esenciales
🐋 Verificar que Docker está instalado y corriendo

docker --version
docker info

sudo systemctl status docker

📦  Construir una imagen desde un Dockerfile

 docker build -t nombre_de_la_imagen .
 usando dockerfile en el mismo directorio

📦  Listar imágenes

docker images

📦  Ejecutar un contenedor a partir de una imagen

docker run -d -p 80:80 nombre_de_la_imagen

otra opción es:

docker run -it --rm nombre-imagen

🚀 Ejecutar un contenedor
docker run -it --rm nombre-imagen

🔹 -it: interactivo
🔹 --rm: se borra al cerrar

🔍 Ver contenedores
docker ps # solo los corriendo
docker ps -a # todos incluso los detenidos

🧼 Eliminar contenedores e imágenes

docker rm id_contenedor
docker rmi nombre_imagen

🧱 Redes
docker network ls
docker inspect nombre_o_id

🔐 SEGURIDAD - Contenedores
✅ Ejecutar sin root
docker run --user 1000:1000 alpine whoami

🚫 Quitar capacidades
docker run --cap-drop=ALL alpine sh

🧊 Sistema de archivos solo lectura
docker run --read-only alpine sh

💾 Limitar memoria y CPU
docker run --memory="100m" --cpus="0.5" alpine

🖊️ COSIGN - Firma de imágenes
Generar claves (si aplica)
cosign generate-key-pair

Firmar imagen
cosign sign --key cosign.key tuusuario/imagen:tag

Verificar firma
cosign verify --key cosign.pub tuusuario/imagen:tag

🧾 SBOM y Escaneo (Teóricos o si están instalados)
SBOM con Syft
syft imagen:tag -o table

Escaneo con Trivy
trivy image imagen:tag

🐳 Comandos extra útiles
docker inspect nombre        # Ver detalles de imagen o contenedor
docker exec -it nombre sh    # Entrar en un contenedor en ejecución
docker logs nombre           # Ver logs de un contenedor

🔁 Git Básico en AWS
git clone https://github.com/Nau-c/tu-repo.git
cd tu-repo

git pull
git add .
git commit -m "cambio"
git push


🧾 ¿Qué es un archivo .pem?
Un archivo .pem (Privacy Enhanced Mail) es una clave privada en formato plano, usada para autenticarse vía SSH con servidores remotos, como una máquina EC2 de AWS.

🔐 ¿Para qué sirve?
Permite acceso seguro y sin contraseña a una instancia EC2

Funciona como tu "llave SSH" privada

Es generada cuando creas una instancia EC2 (o la puedes descargar desde IAM si usas acceso por usuario)

🔑 ¿Cómo usar? Darle permisos
chmod 400 taller.pem


Si estas en windows usando WSL podremos darle permisos con el comando:
chmod ~/.ssh/taller.pem 400

conectar por ssh:
ssh -i ~/.ssh/taller.pem ubuntu@IP_PUBLICA_EC2




