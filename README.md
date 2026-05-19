# 🐾 Tienda de Perritos — EP2 DevOps

## Introducción
Aplicación CRUD de productos para una tienda de alimentos para perritos, desplegada con Docker, GitHub Actions y AWS.

## Objetivos
- Demostrar un flujo CI/CD completo desde el entorno local hasta producción en AWS.
- Contenerizar cada servicio con Docker.
- Automatizar el despliegue con GitHub Actions.

## Arquitectura
Usuario → EC2 Frontend (Nginx) → EC2 Backend (Node.js) → EC2 DB (MySQL)

## Tecnologías
- Docker y Docker Compose
- GitHub Actions
- Amazon ECR
- Amazon EC2 (Amazon Linux 2023)
- AWS SSM
- Node.js + Express
- MySQL 8
- Nginx

## Estructura del proyecto
tienda-perritos/
├── .github/workflows/
│   ├── cicd-tienda-frontend.yml
│   ├── cicd-tienda-backend.yml
│   └── cicd-tienda-db.yml
├── frontend/
│   ├── Dockerfile
│   ├── index.html
│   ├── app.js
│   └── default.conf
├── backend/
│   ├── Dockerfile
│   ├── server.js
│   └── package.json
├── db/
│   ├── Dockerfile
│   └── init.sql
├── docker-compose.yml
└── README.md

## Dockerización
Cada servicio tiene su propio Dockerfile:
- **Frontend**: Nginx Alpine sirviendo HTML/JS estático
- **Backend**: Node.js 18 Alpine con Express y mysql2
- **DB**: MySQL 8 con script de inicialización automática

## Docker Compose
Permite levantar los 3 servicios localmente en una red compartida `tienda-net`.

```bash
docker compose up --build -d
```

## Persistencia
La base de datos usa un volumen Docker `db_data` montado en `/var/lib/mysql`, lo que garantiza que los datos persistan aunque el contenedor se detenga o recree.

## Infraestructura AWS
- 3 instancias EC2 t3.micro con Amazon Linux 2023
- Región: us-east-1

## Security Groups
| Servicio | Puerto | Origen |
|---|---|---|
| Frontend | 80 | 0.0.0.0/0 |
| Backend | 3001 | 0.0.0.0/0 |
| DB | 3306 | Solo desde Backend SG |

## ECR
3 repositorios privados en Amazon ECR:
- tienda-frontend
- tienda-backend
- tienda-db

## CI/CD
El pipeline se activa con cada push a la rama `deploy`:
1. Build de imagen Docker
2. Push a Amazon ECR
3. Deploy automático en EC2 via AWS SSM

## GitHub Secrets
Los secretos se gestionan en GitHub → Settings → Secrets and variables → Actions. No se incluyen credenciales en el código.

## Endpoints
| Endpoint | Método | Descripción |
|---|---|---|
| /api/health | GET | Estado del backend |
| /api/productos | GET | Listar productos |
| /api/productos | POST | Crear producto |
| /api/productos/:id | PUT | Actualizar producto |
| /api/productos/:id | DELETE | Eliminar producto |

## Ejecución local
```bash
git clone https://github.com/Lajerarquia/tienda-perritos.git
cd tienda-perritos
docker compose up --build -d
```

Validar:
- Frontend: http://localhost
- Backend: http://localhost:3001/api/health

## Integrantes
- Benjamín

## Conclusión
Se implementó exitosamente un flujo DevOps completo con contenerización, registro de imágenes en ECR y despliegue automático en EC2 mediante GitHub Actions y AWS SSM.