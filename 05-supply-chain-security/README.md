🔐 Sección 05 - Supply Chain Security
📂 Carpeta: 05-supply-chain-security
⏱ Tiempo estimado: 25–30 minutos
🎯 Objetivo: Proteger el pipeline desde la imagen hasta el despliegue, usando firmas, análisis y políticas



## Firmado de imágenes con Cosign

- Imagen firmada: `tuusuario/insecure-image:v1`
- Claves: `cosign.key`, `cosign.pub`
- Firma exitosa con `cosign sign`
- Verificación OK con `cosign verify`

🧩 Subtemas clave del taller
✅ Firmado de imágenes con Cosign

✅ Generación de un Software Bill of Materials (SBOM)

✅ Escaneo centralizado de vulnerabilidades

✅ Buenas prácticas de cadena de suministro


Documentar en windows la instalacion de cosign
# 05 - Supply Chain Security

## Firma de Imágenes con Cosign (teoría y pasos en Windows)

Cosign permite firmar y verificar imágenes Docker como parte del refuerzo de seguridad en la cadena de suministro.

### ¿Por qué usar Cosign?
- Verifica la autenticidad y origen de las imágenes.
- Garantiza integridad: la imagen no ha sido modificada.
- Es parte de la suite Sigstore, recomendada por CNCF.

---

### Pasos para firmar una imagen en Windows

1. **Instalar Cosign con Scoop** (recomendado):

```powershell

scoop install cosign 

2. Manualmente:

Descargar desde: https://github.com/sigstore/cosign/releases/latest

Archivo: cosign-windows-amd64.exe

Renombrar a cosign.exe y agregar al PATH.


3. Generar claves:

Generar claves:

cosign generate-key-pair

Esto crea:

cosign.key (clave privada)

cosign.pub (clave pública)

4. Subir imagen a Docker Hub:

Subir imagen a Docker Hub:
docker tag insecure-image tuusuario/insecure-image:v1
docker push tuusuario/insecure-image:v1

5. Firmar la imagen:

cosign sign --key cosign.key tuusuario/insecure-image:v1


6. Verificar la firma:

cosign verify --key cosign.pub tuusuario/insecure-image:v1
