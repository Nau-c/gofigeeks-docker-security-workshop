Esta parte parte se ha documentado  en windows/mac

### ✅ Checklist parcial

| Acción                                                          | Estado |
|-----------------------------------------------------------------|--------|
| Explicar qué es SBOM y su utilidad                              | ✅     |
| Documentar herramienta `syft` como generador de SBOM            | ✅     |
| Incluir ejemplos de comando y formatos de salida                | ✅     |
| Agregar contenido a `README.md` y preparar commit               | ✅     |

---

¿Pasamos al **último paso** de esta sección: cómo se hace el **escaneo centralizado de vulnerabilidades** y cómo evitar que imágenes vulnerables lleguen a producción?


🧭 ¿Qué es un escaneo centralizado?
Es una técnica para detectar vulnerabilidades automáticamente cuando:

Se sube una imagen a un registry (como Harbor, GitHub Container Registry, etc.)

Se lanza un pipeline de CI/CD

Se aplica una política de seguridad corporativa (no permitir imágenes con CVEs conocidos)


🛠 ¿Cómo funciona en la práctica?

1. El registro com (Harbor) tiene integrado un motor de escaneo (Trivy, clair , Anchore , aqua)

2. Cada vez que haces un docker push, analiza automaticanmente la imagen

3. Si encuentra vulnerabilidades, las marca como "vulnerable"
     - Las lista y categoriza (CRITICAL, HIGH, MEDIUM, LOW, UNKNOWN)
     - Puede bloquear el uso de esa iamgen en producción

4. Algunas herramientas como Trivy permite escaneo en tiempo de construcción (CI/CD).


📦 Escaneo con Trivy (local o en CI)



