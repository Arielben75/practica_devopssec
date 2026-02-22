# Guía: Configuración de Self-Hosted Runners

Un **Self-Hosted Runner** es un sistema que tú mismo gestionas para ejecutar tus jobs de GitHub Actions. Esto es útil para ahorrar costos o acceder a recursos locales (como bases de datos o hardware específico).

## 1. Requisitos en la Máquina Destino (Runner)
Para que este proyecto funcione en tu propio servidor/PC, debe tener instalado:
- **Docker**: Necesario para el build de imágenes y escaneos de Trivy.
- **Node.js**: Necesario para las etapas de linting y testing (aunque se puede correr vía Docker).
- **Git**: Para clonar el repositorio.

---

## 2. Pasos para Configurar el Runner en GitHub

1.  **Entra a tu Repositorio en GitHub.**
2.  Ve a **Settings** > **Actions** > **Runners**.
3.  Haz clic en el botón **"New self-hosted runner"**.
4.  Selecciona el sistema operativo de tu máquina (ej. Windows, Linux o macOS).
5.  Sigue los comandos que te proporciona GitHub en la sección **"Download"** y **"Configure"**.
    - *Tip*: Durante la configuración (`./config.sh` o `./config.cmd`), te preguntará por etiquetas (labels). Asegúrate de incluir la etiqueta `self-hosted`.

---

## 3. Preparar el Proyecto para Self-Hosted

He actualizado el archivo `.github/workflows/devsecops.yml` con un comentario en la línea `runs-on`. Para usar tu propio runner:

1.  Abre `.github/workflows/devsecops.yml`.
2.  Busca la línea `runs-on: ubuntu-latest`.
3.  Cámbiala por: `runs-on: self-hosted`.

---

## 4. Seguridad de los Runners
> [!WARNING]
> Ten cuidado al usar self-hosted runners en repositorios públicos, ya que si alguien envía un Pull Request con código malicioso, podría ejecutarse en tu máquina local.

Para este proyecto de maestría, si lo corres en tu propia PC, asegúrate de que el runner tenga permisos para ejecutar comandos de Docker sin sudo (en Linux) o con permisos de administrador (en Windows).
