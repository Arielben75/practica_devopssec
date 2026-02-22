# Documentación DevSecOps CI/CD

Este documento explica las herramientas y fases integradas en el pipeline de CI/CD del proyecto, siguiendo un enfoque de **DevSecOps** explícito.

## Arquitectura del Pipeline

El pipeline está diseñado para garantizar la calidad, seguridad y reproducibilidad del software en cada etapa del ciclo de vida.

| Fase DevSecOps | Herramienta | Acción | Propósito |
| :--- | :--- | :--- | :--- |
| **Code / Commit** | **ESLint** | Análisis de Estilo (Linting) | Detectar errores comunes, malas prácticas y asegurar consistencia en el código. |
| **Build** | **npm ci** | Instalación Reproducible | Garantiza que las dependencias sean idénticas a las del archivo `package-lock.json`. |
| **Test** | **Jest** | Unit & Integration Testing | Verificar que la lógica de negocio funcione correctamente y evitar regresiones. |
| **Security (SAST)** | **SonarQube** | Análisis Estático de Seguridad | Identificar vulnerabilidades, "code smells" y bugs en el código fuente antes del despliegue. |
| **Security (SCA)** | **Trivy (FS)** | Software Composition Analysis | Escanear las librerías de terceros en busca de vulnerabilidades conocidas (CVEs). |
| **Package** | **Docker** | Build & Versioning | Empaquetar los servicios en contenedores inmutables etiquetados con el hash del commit (`GITHUB_SHA`). |
| **Security (Containers)** | **Trivy (Image)** | Container Image Scan | Escanear la imagen final (sistema operativo base y binarios) antes de ser distribuida. |

---

## Detalle de Etapas

### 1. Instalación Reproducible (`npm ci`)
A diferencia de `npm install`, el comando `npm ci` es estricto: requiere un `package-lock.json` y borra la carpeta `node_modules` para asegurar una instalación limpia y reproducible. Si hay inconsistencias entre los archivos, el pipeline fallará.

### 2. Calidad de Código (ESLint)
Se ha configurado una regla base en `backend/.eslintrc.json` compartida por todos los microservicios. Esto asegura que el código siga estándares modernos de JavaScript y previene errores sintácticos antes de las pruebas.

### 3. Seguridad Estática (SAST con SonarQube)
SonarQube analiza el código sin ejecutarlo. Busca patrones peligrosos (como inyecciones SQL o manejo inseguro de secretos) y proporciona un informe detallado sobre la salud técnica del proyecto.

### 4. Seguridad de Contenedores (Trivy)
Trivy se usa en dos momentos:
1. **SCA**: Escanea los archivos del repositorio para detectar librerías vulnerables.
2. **Container Security**: Escanea las imágenes Docker generadas para asegurar que el sistema operativo base (Alpine/Nginx) no tenga vulnerabilidades críticas.

---

## Configuración Requerida
Para que el pipeline funcione al 100%, se deben configurar los siguientes **GitHub Secrets**:
- `SONAR_TOKEN`: Token generado desde SonarQube.
- `SONAR_HOST_URL`: URL de tu instancia de SonarQube (ej. SonarCloud o instancia propia).
