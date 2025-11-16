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

- **Java 21** (OpenJDK o Oracle JDK)
  - Descarga desde: https://www.oracle.com/java/technologies/downloads/#java21
  - O Adoptium: https://adoptium.net/
- **Node.js 20 LTS** o superior
  - Descarga desde: https://nodejs.org/
  - Incluye npm automáticamente
- **MySQL 8.0** o superior (para desarrollo local)
- **Maven** (incluido en el proyecto como `mvnw`)
- **Angular CLI** (`npm install -g @angular/cli`)
- **Google Cloud SDK** (para despliegue en producción)
  - Descarga desde: https://cloud.google.com/sdk/docs/install

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

> **Importante:** Cambiar `spring.datasource.password` por la contraseña del MySQL.

### Instalar Dependencias y Compilar

**Windows PowerShell:**
```powershell
# Configurar JAVA_HOME (ajusta la ruta según tu instalación)
$env:JAVA_HOME = "C:\Program Files\Java\jdk-21"
$env:Path = "$env:JAVA_HOME\bin;$env:Path"

# Verificar versión de Java
java -version

# Compilar proyecto
cd Backend
.\mvnw clean install
```

**Linux/Mac:**
```bash
# Verificar Java
java -version

# Compilar proyecto
cd Backend
./mvnw clean install
```

### Ejecutar Backend

```bash
.\mvnw spring-boot:run
```

El backend estará disponible en: **http://localhost:8080**

### Build para Producción (JAR)

```powershell
# Limpiar y compilar sin tests
.\mvnw clean package -DskipTests
```

El archivo JAR se generará en: `Backend/target/sbootporlles-0.0.1-SNAPSHOT.jar`

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

> **Nota:** Si el comando `ng` no se reconoce, instala Angular CLI globalmente:
> ```bash
> npm install -g @angular/cli
> ```

### Ejecutar Frontend en Desarrollo

```bash
ng serve -o
```

El frontend estará disponible en: **http://localhost:4200**

### Build para Producción

**Antes de compilar**, asegúrate de configurar la URL correcta del backend en `src/environments/environment.prod.ts`:

```typescript
export const environment = {
  production: true,
  apiUrl: 'http://34.176.162.36:8080/api',  // IP de tu servidor
  uploadUrl: 'http://34.176.162.36:8080/api/upload',
  maxFileSize: 10485760, // 10MB
  allowedExtensions: ['pdf', 'doc', 'docx', 'xls', 'xlsx', 'jpg', 'jpeg', 'png', 'zip']
};
```

**Compilar:**

```bash
ng build --configuration production
```

Los archivos compilados estarán en: `Frontend/dist/proyectosoluciones/`

---

# ☁️ **DESPLIEGUE EN LA NUBE**

## 📤 **Backend (Compute Engine - Windows Server)**

El backend se despliega en una **VM de Windows Server en Compute Engine**, utilizando **IIS (Internet Information Services)** como servidor web con el módulo **HttpPlatformHandler** para ejecutar la aplicación Spring Boot.

### 1. Crear la VM en Compute Engine

1. Ve a **Compute Engine → Instancias de VM**.
2. Haz clic en **"Crear instancia"**.
3. **Configuración recomendada:**
   - **Nombre:** `windows-server-cloud-computing`
   - **Región:** `southamerica-west1` (Santiago, Chile) - misma región que Cloud SQL
   - **Zona:** `southamerica-west1-a`
   - **Tipo de máquina:** e2-medium (2 vCPU, 4 GB memoria) o superior
   - **Disco de arranque:** Windows Server 2022 Datacenter (50 GB SSD)
   - **Firewall:** ✅ Permitir tráfico HTTP y HTTPS

4. En **"Identidad y acceso a las API"**, selecciona **"Permitir acceso completo a todas las API de Cloud"**.
5. Haz clic en **"Crear"**.

### 2. Configurar Reglas de Firewall

Para permitir acceso al backend en el puerto 8080:

1. Ve a **VPC Network → Firewall**.
2. Haz clic en **"Crear regla de firewall"**.
3. **Configuración:**
   - **Nombre:** `allow-backend-8080`
   - **Dirección del tráfico:** Entrada
   - **Destinos:** Todas las instancias de la red
   - **Filtro de origen:** Rangos de IPv4: `0.0.0.0/0`
   - **Protocolos y puertos:** tcp:`8080`
4. Haz clic en **"Crear"**.

### 3. Construcción del JAR

En tu máquina local con PowerShell:

```powershell
# Configurar JAVA_HOME para Maven
$env:JAVA_HOME = "C:\Program Files\Java\jdk-21"
$env:Path = "$env:JAVA_HOME\bin;$env:Path"

# Limpiar caché de Maven (solo si hay problemas de versión)
# Remove-Item -Recurse -Force "$env:USERPROFILE\.m2" -ErrorAction SilentlyContinue

# Compilar el proyecto
cd Backend
.\mvnw clean package -DskipTests
```

El archivo JAR se generará en: `Backend/target/sbootporlles-0.0.1-SNAPSHOT.jar`

> **Troubleshooting:** Si ves errores de versión de Java (`class file has wrong version`), asegúrate de que `JAVA_HOME` apunte a Java 21 y limpia el caché de Maven como se muestra arriba.

### 4. Subir el JAR a la VM

**Opción A: Usando gcloud CLI (Recomendado)**

```powershell
# Verificar instancias disponibles
gcloud compute instances list

# Subir el JAR al servidor
gcloud compute scp "C:\Users\TU_USUARIO\Desktop\Porlles\Backend\target\sbootporlles-0.0.1-SNAPSHOT.jar" windows-server-cloud-computing:C:\backend\ --zone=southamerica-west1-a
```

> **Nota:** La primera vez que uses `gcloud compute scp`, se te pedirá crear un directorio SSH y generar claves. Acepta con `Y`.

**Opción B: Usando RDP**

1. Conéctate a la VM mediante RDP (Remote Desktop)
2. Transfiere el archivo JAR manualmente
3. Crea la carpeta: `C:\App\backend\`
4. Coloca el archivo JAR en esta carpeta

### 5. Configurar Archivos de Properties

El proyecto utiliza perfiles de Spring Boot para manejar diferentes entornos:

**Archivo principal: `application.properties`**
```properties
spring.application.name=sbootporlles

# Perfil activo (cambiar a 'prod' en producción)
spring.profiles.active=dev

# Configuración común para todos los perfiles
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect

# Configuración de subida de archivos
spring.servlet.multipart.enabled=true
spring.servlet.multipart.max-file-size=10MB
spring.servlet.multipart.max-request-size=10MB
```

**Desarrollo: `application-dev.properties`**
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/ImportPorllesDB?allowPublicKeyRetrieval=true&useSSL=false
spring.datasource.username=root
spring.datasource.password=
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

# File Upload Configuration
file.upload.path=./uploads
file.max-size=10485760
file.allowed-extensions=pdf,doc,docx,xls,xlsx,jpg,jpeg,png,zip

# CORS Configuration
cors.allowed-origins=http://localhost:4200

# Server Port
server.port=8080
```

**Producción: `application-prod.properties`** (en la VM)
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/ImportPorllesDB?allowPublicKeyRetrieval=true&useSSL=false
spring.datasource.username=root
spring.datasource.password=tu_contraseña_segura
spring.jpa.hibernate.ddl-auto=validate
spring.jpa.show-sql=false

# File Upload Configuration
file.upload.path=C:\App\uploads
file.max-size=10485760
file.allowed-extensions=pdf,doc,docx,xls,xlsx,jpg,jpeg,png,zip

# CORS Configuration - IP Externa de la VM
cors.allowed-origins=http://34.176.162.36

# Server Port
server.port=8080
```

> **Importante:** Asegúrate de que `cors.allowed-origins` coincida con la IP/dominio desde donde se accede al frontend.

### 6. Instalar Java en la VM

1. Descarga **Java 21** desde: https://adoptium.net/
2. Instala el JDK en `C:\Program Files\Eclipse Adoptium\jdk-21.0.x\`
3. Verifica la instalación:

```powershell
java -version
```

### 7. Configurar IIS con HttpPlatformHandler

#### Instalar IIS:

1. Abre **Server Manager** → **Add roles and features**.
2. Selecciona **Web Server (IIS)**.
3. Instala con las opciones por defecto.

#### Instalar HttpPlatformHandler:

1. Descarga desde: https://www.iis.net/downloads/microsoft/httpplatformhandler
2. Instala el módulo en IIS.

#### Crear el sitio web en IIS:

1. Abre **IIS Manager**.
2. Clic derecho en **Sites → Add Website**.
3. **Configuración:**
   - **Site name:** `BackendPorlles`
   - **Physical path:** `C:\App\backend`
   - **Binding:** Port `8080`, IP: `*` (todas las IPs)
4. Haz clic en **OK**.

#### Crear web.config:

En `C:\App\backend\`, crea un archivo `web.config`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="httpPlatformHandler" path="*" verb="*" 
           modules="httpPlatformHandler" 
           resourceType="Unspecified" />
    </handlers>
    <httpPlatform processPath="C:\Program Files\Eclipse Adoptium\jdk-21.0.x\bin\java.exe"
                  arguments="-jar &quot;C:\App\backend\sbootporlles-0.0.1-SNAPSHOT.jar&quot; --spring.profiles.active=prod"
                  stdoutLogEnabled="true"
                  stdoutLogFile="C:\App\logs\stdout.log"
                  startupTimeLimit="60"
                  startupRetryCount="3">
      <environmentVariables>
        <environmentVariable name="SPRING_PROFILES_ACTIVE" value="prod" />
      </environmentVariables>
    </httpPlatform>
  </system.webServer>
</configuration>
```

### 8. Configurar Cloud SQL Auth Proxy como Servicio

Para que el proxy se ejecute automáticamente:

#### Descargar NSSM (Non-Sucking Service Manager):

```powershell
# Descarga desde: https://nssm.cc/download
# Extrae a C:\Tools\nssm-2.24\win64\nssm.exe
```

#### Descargar Cloud SQL Auth Proxy:

```powershell
# Descarga desde: https://cloud.google.com/sql/docs/mysql/connect-instance-auth-proxy#windows-64-bit
# Guarda en: C:\App\cloud-sql-proxy.exe
```

#### Crear el servicio:

```powershell
cd C:\Tools\nssm-2.24\win64

.\nssm.exe install CloudSQLProxy "C:\App\cloud-sql-proxy.exe" "--private-ip" "--port" "3306" "proyectocloudcomputing-475904:southamerica-west1:porlles-bd"

# Configurar el servicio para inicio automático
.\nssm.exe set CloudSQLProxy Start SERVICE_AUTO_START

# Iniciar el servicio
.\nssm.exe start CloudSQLProxy

# Verificar el estado
.\nssm.exe status CloudSQLProxy
```

### 9. Iniciar el Backend

Reinicia el sitio web en IIS o reinicia la VM. El backend estará disponible en:

```
http://IP_EXTERNA_VM:8080
```

### 10. Verificación

Prueba el endpoint:

```bash
curl http://IP_EXTERNA_VM:8080/api/health
```

---

## 📤 **Frontend (Firebase Hosting)**

El frontend se despliega en **Firebase Hosting**, un servicio de hosting rápido y seguro con CDN global.

### 1. Configurar Environments de Angular

El proyecto utiliza environments para manejar diferentes configuraciones:

**Desarrollo: `src/environments/environment.ts`**
```typescript
export const environment = {
  production: false,
  apiUrl: 'http://localhost:8080/api',
  uploadUrl: 'http://localhost:8080/api/upload',
  maxFileSize: 10485760, // 10MB
  allowedExtensions: ['pdf', 'doc', 'docx', 'xls', 'xlsx', 'jpg', 'jpeg', 'png', 'zip']
};
```

**Producción: `src/environments/environment.prod.ts`**
```typescript
export const environment = {
  production: true,
  apiUrl: 'http://34.176.162.36:8080/api',  // Reemplaza con tu IP externa
  uploadUrl: 'http://34.176.162.36:8080/api/upload',
  maxFileSize: 10485760, // 10MB
  allowedExtensions: ['pdf', 'doc', 'docx', 'xls', 'xlsx', 'jpg', 'jpeg', 'png', 'zip']
};
```

**Configurar `angular.json`** para usar environments:

Asegúrate de que en `angular.json` esté configurado el reemplazo de archivos:

```json
"configurations": {
  "production": {
    "fileReplacements": [
      {
        "replace": "src/environments/environment.ts",
        "with": "src/environments/environment.prod.ts"
      }
    ],
    // ... resto de configuración
  }
}
```

### 2. Build de Producción

```bash
cd Frontend
ng build --configuration production
```

La carpeta de distribución se generará en: `Frontend/dist/proyectosoluciones/browser/`

### 3. Instalar Firebase CLI

```bash
npm install -g firebase-tools
```

### 4. Login en Firebase

```bash
firebase login
```

Se abrirá tu navegador para autenticarte con tu cuenta de Google.

### 5. Inicializar Firebase en el Proyecto

```bash
firebase init
```

**Configuración:**

1. **¿Qué características quieres configurar?** → Selecciona `Hosting`
2. **¿Qué proyecto quieres usar?** → Selecciona tu proyecto o crea uno nuevo
3. **¿Cuál es tu directorio público?** → `dist/proyectosoluciones/browser`
4. **¿Configurar como SPA?** → `Yes`
5. **¿Sobrescribir index.html?** → `No`

### 6. Desplegar en Firebase

```bash
firebase deploy
```

Al finalizar, verás la URL de tu aplicación:

```
Hosting URL: https://tu-proyecto.web.app
```

### 7. Configurar CORS en el Backend

Actualiza el archivo `application-prod.properties` en la VM para permitir tu dominio de Firebase:

```properties
cors.allowed-origins=https://tu-proyecto.web.app,https://tu-proyecto.firebaseapp.com
```

Reinicia el backend en IIS.

### 8. (Opcional) Configurar Dominio Personalizado

1. Ve a **Firebase Console → Hosting**.
2. Haz clic en **"Agregar dominio personalizado"**.
3. Sigue las instrucciones para configurar los registros DNS.

---

## 🔄 **Actualizar el Despliegue**

### Backend:

1. Construye el nuevo JAR:
   ```bash
   cd Backend
   mvnw clean package -DskipTests
   ```

2. Transfiere el JAR a la VM (reemplaza el existente en `C:\App\backend\`).

3. Reinicia el sitio en IIS:
   ```powershell
   # En la VM
   iisreset
   ```

### Frontend:

1. Construye la nueva versión:
   ```bash
   cd Frontend
   ng build --configuration production
   ```

2. Despliega en Firebase:
   ```bash
   firebase deploy
   ```

---

## 📤 **Base de Datos (Cloud SQL)**

En este proyecto, el backend no se conecta a la base de datos mediante una IP pública. En su lugar, se utiliza la arquitectura recomendada por Google: un servidor de Compute Engine (VM) que se conecta de forma segura a la base de datos a través del **Cloud SQL Auth Proxy usando IP Privada**.

### 1. Creación de la Instancia de Cloud SQL

1. Ve a la **Consola de Google Cloud → SQL**.
2. Haz clic en **"Crear instancia"** y elige **MySQL** (ej. 8.0).

**Configuración para Producción:**

- Establece una **contraseña segura** para el usuario `root`.
- En **"Elige la región y la disponibilidad zonal"**, selecciona **"Varias zonas (con alta disponibilidad)"**. Esto crea una réplica para tolerancia a fallos.
- En **"Personaliza tu instancia"**, ajusta los núcleos (vCPU) y la RAM a un tamaño adecuado para empezar (ej. 2 vCPU, 8 GB RAM).
- Espera a que la instancia se cree.

### 2. Configuración de Red (IP Privada)

Para que la VM y la BD se comuniquen internamente:

1. Dentro de la instancia de Cloud SQL, ve al menú **"Conexiones"**.
2. Ve a la pestaña **"Redes"**.
3. Marca la casilla **"IP privada"**.
4. En el menú desplegable **"Red"**, selecciona `default` (o la red VPC donde reside tu VM).

> **Paso único por proyecto:** Si es la primera vez, Google te pedirá "Configura la conexión". Esto habilita la "Service Networking API" y reserva un rango de IP para los servicios. Sigue el asistente para completarlo.

5. Guarda los cambios de la instancia de Cloud SQL.

### 3. Configuración de Permisos de la VM (Compute Engine)

La VM necesita permiso para autenticarse con la API de Cloud SQL:

1. Ve al menú (☰) → **Compute Engine → Instancias de VM**.
2. **Detén la VM** (este cambio no se puede hacer en caliente).
3. Una vez detenida, haz clic en su nombre para entrar a los detalles y haz clic en **"Editar"**.
4. Busca la sección **"Identidad y acceso a las API"**.
5. En **"Permisos de acceso"**, cambia la configuración a **"Permitir acceso completo a todas las API de Cloud"**.
6. Guarda los cambios e **Inicia la VM**.

### 4. Configuración del Cloud SQL Auth Proxy (En la VM)

El proxy es un "túnel" seguro que se ejecuta en la VM y se conecta a la BD.

1. **Descarga el ejecutable del Cloud SQL Auth Proxy** (`cloud-sql-proxy.exe`) en tu VM de Windows desde: https://cloud.google.com/sql/docs/mysql/sql-proxy
2. Obtén el **"Nombre de conexión de la instancia"** desde la página de "Descripción general" de tu instancia de Cloud SQL (formato: `proyecto:region:instancia`).
3. Ejecuta el proxy. Para producción, se recomienda configurarlo como un **servicio de Windows** (usando `nssm.exe` o similar) para que se inicie automáticamente en segundo plano.

**Comando para ejecutar el proxy:**

```bash
# Reemplaza [NOMBRE_DE_CONEXION] con el tuyo
.\cloud-sql-proxy.exe --private-ip --port 3306 [NOMBRE_DE_CONEXION]
```

- `--private-ip` fuerza al proxy a usar la conexión de red interna que configuramos.
- `--port 3306` hace que el proxy escuche en `localhost:3306`.

### 5. Crear la Base de Datos

Conéctate a la instancia desde Cloud Shell o desde la VM usando el proxy:

```bash
mysql -u root -p -h 127.0.0.1
```

Ejecuta:

```sql
CREATE DATABASE ImportPorllesDB;
```

### 6. Configuración del Backend

Asegúrate de que el archivo `application-prod.properties` en la VM tenga:

```properties
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/ImportPorllesDB?allowPublicKeyRetrieval=true&useSSL=false
spring.datasource.username=root
spring.datasource.password=tu_contraseña_segura
```

> **Nota:** Como el proxy escucha en `localhost:3306`, el backend se conecta a `127.0.0.1:3306`, no a la IP de Cloud SQL directamente.

---

# 🏗️ **ARQUITECTURA DEL SISTEMA**

```
┌─────────────────────────────────────────────────────────────────┐
│                     GOOGLE CLOUD PLATFORM                       │
│                                                                 │
│  ┌──────────────────┐         ┌─────────────────────────────┐  │
│  │  Firebase        │         │   Compute Engine (VM)       │  │
│  │  Hosting         │────────▶│   Windows Server 2022       │  │
│  │                  │  HTTP   │                             │  │
│  │  (Frontend)      │         │  ┌───────────────────────┐  │  │
│  │  Angular 19      │         │  │  IIS + HttpPlatform   │  │  │
│  └──────────────────┘         │  │  Handler              │  │  │
│                               │  └───────────────────────┘  │  │
│                               │           │                 │  │
│                               │  ┌────────▼──────────────┐  │  │
│                               │  │  Spring Boot 3.5      │  │  │
│                               │  │  (Backend API)        │  │  │
│                               │  │  Puerto: 8080         │  │  │
│                               │  └────────┬──────────────┘  │  │
│                               │           │                 │  │
│                               │  ┌────────▼──────────────┐  │  │
│                               │  │  Cloud SQL Auth Proxy │  │  │
│                               │  │  (Servicio Windows)   │  │  │
│                               │  │  localhost:3306       │  │  │
│                               │  └────────┬──────────────┘  │  │
│                               └───────────┼─────────────────┘  │
│                                           │                    │
│                                           │ IP Privada         │
│                                           │ (VPC Network)      │
│                                           │                    │
│                               ┌───────────▼─────────────────┐  │
│                               │   Cloud SQL (MySQL 8.0)     │  │
│                               │   Alta Disponibilidad       │  │
│                               │   ImportPorllesDB           │  │
│                               └─────────────────────────────┘  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘

**Características de Seguridad:**

✅ **Conexión Privada:** La VM y Cloud SQL se comunican por IP privada (sin exponer la BD)
✅ **Cloud SQL Auth Proxy:** Autenticación segura con credenciales de Google Cloud
✅ **Firewall Rules:** Control de acceso granular a nivel de red
✅ **JWT Authentication:** Tokens seguros para autenticación de usuarios
✅ **CORS Configurado:** Solo dominios autorizados pueden acceder al backend
✅ **HTTPS en Firebase:** Certificado SSL automático para el frontend
```

**Flujo de una petición:**

1. Usuario accede al frontend en Firebase Hosting (HTTPS)
2. Angular realiza petición HTTP al backend en la VM (puerto 8080)
3. IIS recibe la petición y la pasa al proceso Java (Spring Boot)
4. Spring Boot se conecta a `localhost:3306` (Cloud SQL Auth Proxy)
5. El proxy establece conexión segura con Cloud SQL vía IP privada
6. Cloud SQL ejecuta la consulta y devuelve los datos
7. La respuesta se envía de vuelta al frontend

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

# 🔧 **TROUBLESHOOTING**

### Error de versión de Java al compilar

**Problema:** `class file has wrong version 61.0, should be 52.0`

**Solución:**

Este error indica que Maven está usando una versión incorrecta de Java. Java 21 es la versión correcta (61.0).

```powershell
# 1. Configurar JAVA_HOME
$env:JAVA_HOME = "C:\Program Files\Java\jdk-21"
$env:Path = "$env:JAVA_HOME\bin;$env:Path"

# 2. Verificar versión
java -version  # Debe mostrar "java version 21.0.x"

# 3. Limpiar caché de Maven (elimina dependencias con versión incorrecta)
Remove-Item -Recurse -Force "$env:USERPROFILE\.m2\repository\org\springframework" -ErrorAction SilentlyContinue

# 4. Recompilar
cd Backend
.\mvnw clean package -DskipTests
```

### Backend no se conecta a la base de datos

**Problema:** `Communications link failure`

**Solución:**

1. Verifica que el servicio Cloud SQL Auth Proxy esté corriendo:
   ```powershell
   nssm status CloudSQLProxy
   ```

2. Revisa los logs del proxy:
   ```powershell
   Get-Content C:\App\logs\proxy.log -Tail 50
   ```

3. Verifica que la VM tenga permisos para acceder a Cloud SQL:
   - Ve a **Compute Engine → VM → Editar**
   - En "Permisos de acceso", debe estar "Permitir acceso completo a todas las API"

4. Verifica que Cloud SQL tenga IP privada habilitada:
   - Ve a **Cloud SQL → Conexiones → Redes**
   - Debe estar marcada la casilla "IP privada"

### Frontend no puede conectarse al backend

**Problema:** `CORS error`, `Connection refused`, o `ERR_CONNECTION_TIMED_OUT`

**Solución:**

1. **Abrir el puerto 8080 en el firewall de Google Cloud:**
   ```bash
   gcloud compute firewall-rules create allow-backend-8080 --allow tcp:8080 --source-ranges 0.0.0.0/0 --description "Allow Spring Boot backend on port 8080"
   ```

2. Verifica que la regla esté activa:
   ```bash
   gcloud compute firewall-rules list --filter="name=allow-backend-8080"
   ```

2. Verifica que el backend esté corriendo:
   ```powershell
   # En la VM
   netstat -ano | findstr :8080
   ```

3. Verifica la configuración de CORS en `application-prod.properties`:
   ```properties
   cors.allowed-origins=http://34.176.162.36
   ```
   
   > **Importante:** La URL debe coincidir exactamente con el origen del frontend (protocolo, dominio y puerto).

4. Verifica que el environment del frontend tenga la URL correcta:
   ```typescript
   // src/environments/environment.prod.ts
   apiUrl: 'http://34.176.162.36:8080/api'  // Sin barra final
   ```

5. Recompila y redesplega ambos proyectos después de cambiar las configuraciones.

### IIS no inicia la aplicación

**Problema:** Error 500 o "Service Unavailable"

**Solución:**

1. Revisa los logs de IIS:
   ```powershell
   Get-Content C:\App\logs\stdout.log -Tail 50
   ```

2. Verifica que la ruta de Java en `web.config` sea correcta:
   ```powershell
   Test-Path "C:\Program Files\Eclipse Adoptium\jdk-21.0.x\bin\java.exe"
   ```

3. Verifica que el archivo JAR exista:
   ```powershell
   Test-Path "C:\App\backend\sbootporlles-0.0.1-SNAPSHOT.jar"
   ```

4. Reinicia IIS:
   ```powershell
   iisreset
   ```

### El servicio Cloud SQL Proxy no inicia

**Problema:** El servicio falla al iniciar

**Solución:**

1. Verifica el nombre de conexión:
   ```powershell
   # Debe ser: proyectocloudcomputing-475904:southamerica-west1:porlles-bd
   ```

2. Reinstala el servicio:
   ```powershell
   cd C:\Tools\nssm-2.24\win64
   .\nssm.exe stop CloudSQLProxy
   .\nssm.exe remove CloudSQLProxy confirm
   .\nssm.exe install CloudSQLProxy "C:\App\cloud-sql-proxy.exe" "--private-ip" "--port" "3306" "proyectocloudcomputing-475904:southamerica-west1:porlles-bd"
   .\nssm.exe start CloudSQLProxy
   ```

### Cambios en el código no se reflejan

**Backend:**
```powershell
# 1. Configurar JAVA_HOME
$env:JAVA_HOME = "C:\Program Files\Java\jdk-21"
$env:Path = "$env:JAVA_HOME\bin;$env:Path"

# 2. Reconstruir JAR
cd Backend
.\mvnw clean package -DskipTests

# 3. Subir a VM
gcloud compute scp "target\sbootporlles-0.0.1-SNAPSHOT.jar" windows-server-cloud-computing:C:\backend\ --zone=southamerica-west1-a

# 4. Conectarse a VM y reiniciar IIS
gcloud compute ssh windows-server-cloud-computing --zone=southamerica-west1-a
# Dentro de la VM:
iisreset
```

**Frontend:**
```bash
# 1. Verificar/actualizar environment.prod.ts con URL correcta

# 2. Reconstruir
cd Frontend
ng build --configuration production

# 3. Redesplegar en Firebase
firebase deploy
```

### Node.js o npm no se reconoce

**Problema:** `npm: The term 'npm' is not recognized`

**Solución:**

1. Instala Node.js 20 LTS desde: https://nodejs.org/
2. Durante la instalación, asegúrate de marcar "Add to PATH"
3. Reinicia PowerShell/Terminal
4. Verifica:
   ```bash
   node --version
   npm --version
   ```

### Angular CLI (ng) no se reconoce

**Problema:** `ng: The term 'ng' is not recognized`

**Solución:**

```bash
# Instalar Angular CLI globalmente
npm install -g @angular/cli

# Verificar instalación
ng version
```

---

# 👥 **EQUIPO - GRUPO 8**

**Curso:** Cloud Computing y Continuidad  
**Institución:** [Tu institución]  
**Año:** 2025

---

# 📞 **SOPORTE**

Para reportar issues o solicitar features, usa los repositorios de GitHub mencionados arriba.
