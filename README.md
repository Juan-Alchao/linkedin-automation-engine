# 🚀 Guía de Instalación - Backend

## Paso 1: Instalar PostgreSQL

### Windows:
1. Descarga PostgreSQL desde: https://www.postgresql.org/download/windows/
2. Ejecuta el instalador
3. Durante la instalación, anota la contraseña que defines para el usuario `postgres`
4. Verifica la instalación: abre CMD y ejecuta `psql --version`

### Mac:
```bash
brew install postgresql@15
brew services start postgresql@15
```

### Linux (Ubuntu/Debian):
```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
sudo systemctl start postgresql
```

---

## Paso 2: Crear la Base de Datos

1. Abre la terminal o CMD
2. Conéctate a PostgreSQL:
```bash
psql -U postgres
```

3. Ejecuta estos comandos dentro de `psql`:
```sql
CREATE DATABASE linkedin_automation;
\q
```

4. Ejecuta el script de schema:
```bash
psql -U postgres -d linkedin_automation -f schema.sql
```

O copia y pega el contenido del archivo `schema.sql` directamente en psql.

---

## Paso 3: Configurar el Proyecto Backend

1. **Crea una carpeta para el proyecto:**
```bash
mkdir linkedin-automation-backend
cd linkedin-automation-backend
```

2. **Crea la estructura de archivos:**
```
linkedin-automation-backend/
├── server.js
├── package.json
├── .env
└── database/
    └── schema.sql
```

3. **Copia los archivos:**
   - Copia el contenido de `server.js`
   - Copia el contenido de `package.json`
   - Copia el contenido de `schema.sql` en la carpeta `database/`

4. **Crea el archivo `.env`:**
```bash
# Copia .env.example a .env
cp .env.example .env
```

5. **Edita `.env` con tus credenciales:**
```
PORT=3000
DB_HOST=localhost
DB_PORT=5432
DB_NAME=linkedin_automation
DB_USER=postgres
DB_PASSWORD=TU_PASSWORD_DE_POSTGRES
JWT_SECRET=mi_super_secreto_2024_xyz
```

---

## Paso 4: Instalar Dependencias

```bash
npm install
```

Esto instalará:
- express (servidor web)
- cors (permitir peticiones desde el frontend)
- pg (conexión a PostgreSQL)
- bcrypt (encriptar contraseñas)
- jsonwebtoken (autenticación)
- dotenv (variables de entorno)
- node-cron (tareas programadas)

---

## Paso 5: Ejecutar el Servidor

### Modo desarrollo (con reinicio automático):
```bash
npm run dev
```

### Modo producción:
```bash
npm start
```

Si todo está correcto, verás:
```
✅ Servidor corriendo en http://localhost:3000
```

---

## Paso 6: Probar la API

### Usando curl (desde la terminal):

**1. Registrar un usuario:**
```bash
curl -X POST http://localhost:3000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "password123",
    "name": "Usuario Test",
    "team_name": "Mi Equipo"
  }'
```

**2. Login:**
```bash
curl -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "password123"
  }'
```

**3. Crear campaña (requiere token):**
```bash
curl -X POST http://localhost:3000/api/campaigns \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer TU_TOKEN_AQUI" \
  -d '{
    "name": "Mi Primera Campaña",
    "description": "Campaña de prueba",
    "sequence": [
      {"type": "visit", "delay": 0, "name": "Visitar perfil"},
      {"type": "connect", "delay": 2, "name": "Conectar", "message": "Hola!"}
    ],
    "daily_limit": 50
  }'
```

---

## Endpoints Disponibles

### Autenticación:
- `POST /api/auth/register` - Registrar usuario
- `POST /api/auth/login` - Iniciar sesión

### Campañas:
- `GET /api/campaigns` - Listar campañas
- `POST /api/campaigns` - Crear campaña
- `PUT /api/campaigns/:id` - Actualizar campaña
- `DELETE /api/campaigns/:id` - Eliminar campaña

### Prospectos:
- `GET /api/campaigns/:id/prospects` - Listar prospectos de una campaña
- `POST /api/campaigns/:id/prospects` - Agregar prospecto
- `PUT /api/prospects/:id` - Actualizar prospecto

### Estadísticas:
- `GET /api/stats` - Obtener estadísticas generales

### Para la Extensión:
- `GET /api/extension/active-campaigns` - Campañas activas
- `GET /api/extension/pending-prospects/:campaignId` - Prospectos pendientes
- `POST /api/extension/report-action` - Reportar acción ejecutada

---

## Solución de Problemas

### Error: "connect ECONNREFUSED"
- PostgreSQL no está corriendo. Inicia el servicio:
  - Windows: Busca "Services" → PostgreSQL → Start
  - Mac: `brew services start postgresql`
  - Linux: `sudo systemctl start postgresql`

### Error: "password authentication failed"
- Verifica que la contraseña en `.env` sea correcta
- Verifica que el usuario sea `postgres`

### Error: "database does not exist"
- Ejecuta: `psql -U postgres -c "CREATE DATABASE linkedin_automation;"`

### Puerto 3000 ocupado:
- Cambia el puerto en `.env`: `PORT=3001`

---

## ✅ Siguiente Paso

Una vez que el backend esté funcionando correctamente:
1. Verifica que puedas hacer login y crear campañas
2. Procederemos a crear la **Extensión de Chrome**

¿El backend está corriendo correctamente?
