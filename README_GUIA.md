# Guía de Inicio: Backend y Frontend

Este proyecto utiliza una arquitectura de microservicios con un frontend en React. A continuación, se detallan los pasos para levantar el entorno completo.

## Requisitos Previos
- Docker y Docker Compose instalados.
- Node.js (opcional, para ejecución local sin Docker).

## Estructura del Proyecto
- **backend/users-service**: Servicio de autenticación y usuarios (Node.js/Express).
- **backend/academic-service**: Servicio de gestión académica (Node.js/Express).
- **backend/api-gateway**: Punto de entrada único para el backend.
- **frontend**: Aplicación React (SPA) servida por Nginx.

---

## 1. Configuración de Variables de Entorno
Antes de levantar los servicios, asegúrate de crear los archivos `.env` necesarios. Puedes basarte en los archivos `.env.example` en cada directorio:

- `backend/users-service/.env`
- `backend/academic-service/.env`
- `backend/api-gateway/.env`
- `frontend/.env`

**Ejemplo básico para `users-service`:**
```env
PORT=3001
JWT_SECRET=shared-secret-key
```

---

## 2. Levantar el Proyecto con Docker Compose (Recomendado)
El archivo `backend/docker-compose.yml` está configurado para levantar todos los servicios, incluido el frontend.

1. Navega a la carpeta del backend:
   ```bash
   cd backend
   ```
2. Ejecuta el comando para construir y levantar los contenedores:
   ```bash
   docker compose up --build
   ```

### Accesos Rápidos
| Servicio | URL Local | Puerto |
| :--- | :--- | :--- |
| **Frontend** | [http://localhost:5173](http://localhost:5173) | 5173 |
| **API Gateway** | [http://localhost:3000](http://localhost:3000) | 3000 |
| **Users Service** | http://localhost:3001 (Interno) | 3001 |
| **Academic Service** | http://localhost:3002 (Interno) | 3002 |

---

## 3. Ejecución para Desarrollo (Local)
Si prefieres correr los servicios sin Docker para debuguear:

### Backend (Repetir para cada servicio)
1. Entra al directorio (ej. `backend/users-service`).
2. Instala dependencias: `npm install`
3. Inicia en modo desarrollo: `npm run dev`

### Frontend
1. Entra al directorio `frontend/`.
2. Instala dependencias: `npm install`
3. Inicia el servidor de Vite: `npm run dev`
   *El frontend estará disponible en [http://localhost:5173](http://localhost:5173)*.

---

## Notas de DevSecOps
- El pipeline de GitHub Actions (`.github/workflows/devsecops.yml`) realiza escaneos de seguridad automáticos con **Semgrep** (SAST) y **Trivy** (SCA) antes de levantar los contenedores para pruebas de humo.
- Los manifiestos en la carpeta `k8s` permiten el despliegue en un clúster de Kubernetes, gestionando los servicios y el ingreso (Ingress).
