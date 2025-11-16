# 🖥️ **T3 CLOUD COMPUTING Y CONTINUIDAD GRUPO 8**

![Logo del Proyecto](https://media.discordapp.net/attachments/1111808588231479369/1439259737244831744/image.png?ex=6919de95&is=69188d15&hm=b1dbc59f78f1ba6ed026304c4cf4d075b4433f5aa93e892a1db41071b9f020ac&=&format=webp&quality=lossless&width=717&height=659)

Sistema de gestión de importaciones y ventas desarrollado con tecnologías cloud-native para el curso de Cloud Computing y Continuidad.

---

# 🛠️ **TECNOLOGÍAS UTILIZADAS**

- **Google Cloud Platform** - Infraestructura en la nube
- **Angular 19** - Framework frontend
- **Spring Boot 3.5** - Framework backend  
- **MySQL 8.0** - Base de datos relacional
- **Java 21** - Lenguaje backend
- **JWT** - Autenticación y seguridad

![Logo del Proyecto](https://media.discordapp.net/attachments/1111808588231479369/1439295337390018640/78ca285e-7cf5-4c1c-88ed-92361a0f3fdb.png?ex=6919ffbd&is=6918ae3d&hm=a52213284c7a3a50398e541788e82978b367d2fd120783f470a52303c6302b6e&=&format=webp&quality=lossless&width=820&height=547)

---

# 🖼️ **Repositorios**

## 🖥️ **Frontend**
Link: del repositorio frontend https://github.com/SoreIllidan/Porlles_Frontend/

## ⚙️ **Backend**
Link: del repositorio backend https://github.com/SoreIllidan/Porlles_Frontend/

---


# 🚀 **EJECUCIÓN LOCAL**

## 📋 **Requisitos Previos**

- **Java 21** o superior
- **Node.js 18** o superior
- **MySQL 8.0** o superior
- **Maven 3.6** o superior (incluido en el proyecto como `mvnw`)
- **Angular CLI** (`npm install -g @angular/cli`)

---

## 🔧 **1. Base de Datos (MySQL)**

### Pasos:

1. **Instala MySQL Workbench** o MySQL Server.
2. **Conéctate a tu instancia local de MySQL.**
3. **Ejecuta el siguiente comando:**

```sql
CREATE DATABASE ImportPorllesDB;
```

> **Nota:** La base de datos debe llamarse exactamente `ImportPorllesDB` para que el backend funcione correctamente. Spring Boot creará automáticamente las tablas necesarias al iniciar.

---

## ⚙️ **2. Backend (Spring Boot)**

### Configurar Base de Datos

Edita el archivo `Backend/src/main/resources/application-dev.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/ImportPorllesDB?allowPublicKeyRetrieval=true&useSSL=false
spring.datasource.username=root
spring.datasource.password=tu_contraseña_mysql
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
server.port=8080
```

> **Importante:** Cambia `spring.datasource.password` por tu contraseña de MySQL.

### Instalar Dependencias

```bash
cd Backend
mvnw clean install
```

O en Linux/Mac:
```bash
./mvnw clean install
```

### Ejecutar Backend

```bash
mvnw spring-boot:run
```

O en Linux/Mac:
```bash
./mvnw spring-boot:run
```

El backend estará disponible en: **http://localhost:8080**

---

## 🌐 **3. Frontend (Angular)**

### Configurar Entorno

Verifica que el archivo `Frontend/src/environments/environment.ts` tenga la URL correcta del backend:

```typescript
export const environment = {
  production: false,
  apiUrl: 'http://localhost:8080/api',
  uploadUrl: 'http://localhost:8080/api/upload'
};
```

### Instalar Dependencias

```bash
cd Frontend
npm install
```

### Ejecutar Frontend

```bash
ng serve -o
```

O:
```bash
npm start
```

El frontend estará disponible en: **http://localhost:4200**

> **Nota:** El navegador se abrirá automáticamente con la opción `-o`.

---

## ✅ **Verificación**

Una vez iniciados ambos servicios:

1. **Backend:** http://localhost:8080
2. **Frontend:** http://localhost:4200
3. **Base de datos:** Debe tener las tablas creadas automáticamente en `ImportPorllesDB`

---

# ☁️ **DESPLIEGUE EN LA NUBE**

## 📤 **Backend (Google Cloud)**

### Construcción del JAR

```bash
cd Backend
mvnw clean package -DskipTests
```

El archivo JAR se generará en: `Backend/target/sbootporlles-0.0.1-SNAPSHOT.jar`

### Variables de entorno necesarias (Google Cloud Run/Compute Engine)

```bash
DB_HOST=tu_ip_cloudsql
DB_PORT=3306
DB_NAME=ImportPorllesDB
DB_USER=root
DB_PASSWORD=tu_contraseña
UPLOAD_PATH=/var/uploads/porlles
PORT=8080
```

### Desplegar con Docker (Opcional)

```bash
docker build -t porlles-backend .
docker run -p 8080:8080 \
  -e DB_HOST=tu_ip_cloudsql \
  -e DB_USER=root \
  -e DB_PASSWORD=tu_contraseña \
  porlles-backend
```

---

## 📤 **Frontend (Vercel / Firebase Hosting)**

### Build de producción

```bash
cd Frontend
ng build --configuration production
```

La carpeta de distribución se generará en: `Frontend/dist/proyectosoluciones/`

### Configurar URL de producción

Edita `Frontend/src/environments/environment.prod.ts`:

```typescript
export const environment = {
  production: true,
  apiUrl: 'http://tu-ip-backend:8080/api',
  uploadUrl: 'http://tu-ip-backend:8080/api/upload'
};
```

### Desplegar en Firebase

```bash
npm install -g firebase-tools
firebase login
firebase init
firebase deploy
```

### Desplegar en Vercel

```bash
npm install -g vercel
vercel
```

---

## 📤 **Base de Datos (Cloud SQL)**

### Crear instancia Cloud SQL (MySQL)

1. Ve a Google Cloud Console → SQL
2. Crea una instancia MySQL 8.0
3. Configura usuario y contraseña
4. Crea la base de datos `ImportPorllesDB`

```sql
CREATE DATABASE ImportPorllesDB;
```

### Conectar desde backend

Usa las variables de entorno mencionadas anteriormente o configura Cloud SQL Proxy.

---

# 📘 **COMANDOS FRECUENTES**

### Angular
```bash
ng serve                          # Iniciar servidor de desarrollo
ng build                          # Construir proyecto
ng build --configuration production  # Build de producción
ng generate component nombre      # Crear nuevo componente
ng test                           # Ejecutar tests
```

### Spring Boot
```bash
mvnw clean install                # Compilar e instalar dependencias
mvnw spring-boot:run              # Ejecutar aplicación
mvnw test                         # Ejecutar tests
mvnw clean package                # Generar JAR
```

### MySQL
```sql
SHOW DATABASES;                   # Listar bases de datos
USE ImportPorllesDB;              # Seleccionar base de datos
SHOW TABLES;                      # Listar tablas
SELECT * FROM tabla;              # Ver datos de tabla
```

---

# 🧪 **TESTS**

### Angular
```bash
cd Frontend
ng test
```

### Spring Boot
```bash
cd Backend
mvnw test
```

---

# 📁 **ESTRUCTURA DEL PROYECTO**

```
Porlles/
├── Backend/                 # API REST con Spring Boot
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/
│   │   │   └── resources/
│   │   └── test/
│   └── pom.xml
│
└── Frontend/                # Aplicación Angular
    ├── src/
    │   ├── app/
    │   │   ├── admin/      # Módulo de administración
    │   │   ├── auth/       # Autenticación
    │   │   ├── pages/      # Páginas públicas
    │   │   └── shared/     # Servicios y modelos
    │   └── environments/
    └── package.json
```

---

# 🔐 **SEGURIDAD**

- **Autenticación:** JWT (JSON Web Tokens)
- **CORS:** Configurado para desarrollo y producción
- **Upload de archivos:** Máximo 10MB
- **Extensiones permitidas:** PDF, DOC, DOCX, XLS, XLSX, JPG, JPEG, PNG, ZIP

---

# 👥 **EQUIPO - GRUPO 8**

**Curso:** Cloud Computing y Continuidad  
**Institución:** [Tu institución]  
**Año:** 2025

---

# 📞 **SOPORTE**

Para reportar issues o solicitar features, usa los repositorios de GitHub mencionados arriba.
