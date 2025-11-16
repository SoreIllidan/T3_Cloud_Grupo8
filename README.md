# 🖥️ **T3 CLOUD COMPUTING Y CONTINUIDAD GRUPO 8**

![Logo del Proyecto](https://media.discordapp.net/attachments/1111808588231479369/1439259737244831744/image.png?ex=6919de95&is=69188d15&hm=b1dbc59f78f1ba6ed026304c4cf4d075b4433f5aa93e892a1db41071b9f020ac&=&format=webp&quality=lossless&width=717&height=659)

Descripción breve del proyecto.

---

# 🛠️ **TECNOLOGÍAS UTILIZADAS**

- Google cloud
- Angular  
- Spring Boot  
- MySQL

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
- **Maven 3.6** o superior
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
---

## ⚙️ **2. Backend (Spring Boot)**

### Archivo `application.properties`

spring.datasource.url=jdbc:mysql://localhost:3306/nombre_bd
spring.datasource.username=usuario
spring.datasource.password=contraseña
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
server.port=8080
```

### Instalar dependencias
```bash
cd backend
mvn clean install
```

### Ejecutar backend
```bash
mvn spring-boot:run
```

---

## 🌐 **3. Frontend (Angular)**

### Instalar dependencias
```bash
cd frontend
npm install
```

### Ejecutar Angular
```bash
ng serve -o
```

---

# ☁️ **DESPLIEGUE EN LA NUBE**

## 📤 **Backend**

### Docker
```bash
docker build -t backend-app .
docker run -p 8080:8080 backend-app
```

### Variables de entorno necesarias
```
SPRING_DATASOURCE_URL=
SPRING_DATASOURCE_USERNAME=
SPRING_DATASOURCE_PASSWORD=
```

---

## 📤 **Frontend**

### Build de producción
```bash
ng build --configuration production
```

Subir la carpeta:

```
frontend/dist/
```

A Vercel / Netlify / servidor propio.

---

## 📤 **Base de Datos**

### Importar en servidor remoto
```bash
mysql -h host -u usuario -p nombre_bd < database/schema.sql
```

---

# 📘 **COMANDOS FRECUENTES**

### Angular
```bash
ng serve
ng build
ng generate component nombre
```

### Spring Boot
```bash
mvn clean install
mvn spring-boot:run
```

### MySQL
```sql
SHOW TABLES;
SELECT * FROM tabla;
```

---

# 🧪 **TESTS**

### Angular
```bash
ng test
```

### Spring Boot
```bash
mvn test
```

---

# 👤 **AUTOR**

Nombre del autor  
Correo / Redes (opcional)
