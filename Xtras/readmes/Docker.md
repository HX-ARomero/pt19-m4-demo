# Docker: Paso a Paso

## 1. Creación de "Dockerfile"

- Creamos ARCHIVO "Dockerfile" en la raíz del proyecto

```dockerfile
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "run", "start"]
```

### 2. Creación de "docker-compose.yaml"

[Ejemplo de Archivo "docker-compose.yaml"](./docker-compose.yaml)

### 3. Declarar variables de entorno

- En ARCHIVO ".development.env":
  - Estas variables se crearán al crearse la base de datos en el contenedor

```.env
POSTGRES_PASSWORD=root
POSTGRES_DB=ecommerce_ft50
```

### 4. Declaración de "host"

- En ARCHIVO "./src/config/typeorm.ts":
  - Declaramos "host" donde se encontrará el Contenedor

```ts
const config = {
	type: 'postgres',
	database: process.env.DB_NAME,
	// host: process.env.DB_HOST,
	host: 'postgresdb',
```

### 5. Inicialización de Docker Daemon

- Inicializamos Docker Desktop
  - Necesario para poder generar imágenes y contenedores

### 6. Ignorar carpeta "node_modules"

- Creamos ARCHIVO ".dockerignore" en la raíz del proyecto:
  - Los "node_modules" se instalarán dentro del contenedor, y copiarlos puede generar conflictos.

```.dockerignore
node_modules
```

### 7. Configuración de "typeorm.ts"

- NECESITAMOS QUE SE CREEN LAS TABLAS !!!!!

```ts
const config = {
  // ----- ----- ----- -----
  synchronize: true,
  dropSchema: true,
};
```

### 8. Corremos "docker compose"

- En la Terminal Integrada ingresamos:

```bash
docker compose up
```
