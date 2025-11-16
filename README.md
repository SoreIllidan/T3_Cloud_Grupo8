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

> **Importante:** Cambiar `spring.datasource.password` por la contraseña del MySQL.

### Instalar Dependencias

```bash
cd Backend
mvnw clean install
```

### Ejecutar Backend

```bash
mvnw spring-boot:run
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

El frontend estará disponible en: **http://localhost:4200**

---

# ☁️ **DESPLIEGUE EN LA NUBE**

## 📤 **Backend (Compute Engine - Windows Server)**

El backend se despliegó en una **VM de Windows Server en Compute Engine**, utilizando **IIS (Internet Information Services)** como servidor web con el módulo **HttpPlatformHandler** para ejecutar la aplicación Spring Boot.

### 1. Crear la VM en Compute Engine

1. Vamos a **Compute Engine → Instancias de VM**.
2. Hacemos clic en **"Crear instancia"**.
3. **Configuración aplicada:**
   - **Nombre:** `windows-server-cloud-computing`
   - **Región:** `southamerica-west1` (Santiago, Chile) - misma región que Cloud SQL
   - **Zona:** `southamerica-west1-a`
   - **Tipo de máquina:** e2-medium (2 vCPU, 4 GB memoria)
   - **Disco de arranque:** Windows Server 2022 Datacenter (50 GB SSD)
   - **Firewall:** Permitir tráfico HTTP y HTTPS

4. En **"Identidad y acceso a las API"**, seleccionamos **"Permitir acceso completo a todas las API de Cloud"**.
5. Hacemos clic en **"Crear"**.

### 2. Configurar Reglas de Firewall

Para permitir acceso al backend en el puerto 8080:

1. Vamos a **VPC Network → Firewall**.
2. Hacemos clic en **"Crear regla de firewall"**.
3. **Configuración:**
   - **Nombre:** `allow-backend-8080`
   - **Dirección del tráfico:** Entrada
   - **Destinos:** Todas las instancias de la red
   - **Filtro de origen:** Rangos de IPv4: `0.0.0.0/0`
   - **Protocolos y puertos:** tcp:`8080`
4. Haz clic en **"Crear"**.

### 3. Construcción del JAR

En tu máquina local:

```bash
cd Backend
mvnw clean package -DskipTests
```

El archivo JAR se generará en: `Backend/target/sbootporlles-0.0.1-SNAPSHOT.jar`

### 4. Subir el JAR 

Transferimos el archivo JAR a la siguiente ruta:

```powershell
C:\App\backend\
```

Colocamos el archivo JAR en esta carpeta.

### 5. Configurar application-prod.properties

editamos el archivo `application-prod.properties`:

```properties
spring.application.name=sbootporlles
spring.datasource.url=jdbc:mysql://${DB_HOST:localhost}:${DB_PORT:3306}/${DB_NAME:ImportPorllesDB}?allowPublicKeyRetrieval=true&useSSL=true&serverTimezone=UTC
spring.datasource.username=${DB_USER:root}
spring.datasource.password=${DB_PASSWORD:}
spring.jpa.hibernate.ddl-auto=validate
spring.jpa.show-sql=false

file.upload.path=${UPLOAD_PATH:/var/uploads/porlles}
file.max-size=10485760
file.allowed-extensions=pdf,doc,docx,xls,xlsx,jpg,jpeg,png,zip

cors.allowed-origins=http://34.176.162.36

server.port=${PORT:8080}
```

### 6. Instalar Java en la VM

1. Descargamos **Java 21** desde: https://adoptium.net/
2. Instalamos el JDK en `C:\Program Files\Eclipse Adoptium\jdk-21.0.x\`

### 7. Configurar IIS con HttpPlatformHandler

#### Instalar IIS:

1. Abrimos **Server Manager** → **Add roles and features**.
2. Selecciona **Web Server (IIS)**.
3. Instalamos con las opciones por defecto.

#### Instalar HttpPlatformHandler:

1. Descargamos desde: https://www.iis.net/downloads/microsoft/httpplatformhandler
2. Instalamos el módulo en IIS.

#### Crear el sitio web en IIS:

1. Abrimos **IIS Manager**.
2. Clic derecho en **Sites → Add Website**.
3. **Configuración:**
   - **Site name:** `BackendPorlles`
   - **Physical path:** `C:\App\backend`
   - **Binding:** Port `8080`, IP: `*` (todas las IPs)
4. Hacemos clic en **OK**.

#### Crear web.config:

En `C:\App\backend\`, creamos un archivo `web.config`:

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
# Descargamos desde: https://nssm.cc/download
# Extraemos a C:\Tools\nssm-2.24\win64\nssm.exe
```

#### Descargar Cloud SQL Auth Proxy:

```powershell
# Descargamos desde: https://cloud.google.com/sql/docs/mysql/connect-instance-auth-proxy#windows-64-bit
# Guardamos en: C:\App\cloud-sql-proxy.exe
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

Reiniciamos el sitio web en IIS. El backend estará disponible.

## 📤 **Frontend (Firebase Hosting)**

El frontend se despliega en **Firebase Hosting**, un servicio de hosting rápido y seguro con CDN global.

### 1. Configurar URL de Producción

Edita `Frontend/src/environments/environment.prod.ts` con la IP externa de tu VM:

```typescript
export const environment = {
  production: true,
  apiUrl: 'http://34.176.162.36:8080/api',  // Reemplaza con tu IP externa
  uploadUrl: 'http://34.176.162.36:8080/api/upload'
};
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

**Problema:** `CORS error` o `Connection refused`

**Solución:**

1. Verifica que la regla de firewall para el puerto 8080 esté activa:
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
   cors.allowed-origins=https://tu-proyecto.web.app
   ```

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
```bash
# Reconstruir JAR
cd Backend
mvnw clean package -DskipTests

# Transferir a VM y reiniciar IIS
iisreset
```

**Frontend:**
```bash
# Reconstruir y redesplegar
cd Frontend
ng build --configuration production
firebase deploy
```

### Configuración de Rutas del Frontend con IIS como Proxy Inverso

Cuando se implementa IIS como proxy inverso, el frontend debe usar rutas relativas en lugar de llamar directamente a la IP y puerto del backend. Esto permite que IIS maneje internamente las peticiones y las redirija al backend.

#### **Configuración del Frontend para usar Rutas Relativas**

**Antes (Configuración directa):**
```typescript
// Configuración directa al puerto del backend
apiUrl: 'http://34.176.162.36:8080/api/auth/registrar'
```

**Después (Configuración con proxy inverso):**
```typescript
// Ruta relativa - IIS redirige internamente
apiUrl: '/api/auth/registrar'
```

**Cómo funciona:**

Cuando usas una **ruta relativa**, IIS (puerto 80) recibe la llamada, identifica que empieza con `/api/` y, según el `web.config` configurado, la redirige internamente a `http://localhost:8080/api/auth/registrar`.

**Implementación:**

1. Configura el archivo de entorno (`environment.ts` o `environment.prod.ts`):
   ```typescript
   export const environment = {
     production: true,
     apiUrl: '/api',  // Ruta relativa, sin IP ni puerto
     uploadUrl: '/api/upload'
   };
   ```

2. Actualiza los servicios Angular para usar la configuración:
   ```typescript
   // En auth.service.ts, producto.services.ts, etc.
   private apiUrl = environment.apiUrl;
   ```

3. Reconstruye el frontend:
   ```bash
   cd Frontend
   ng build --configuration production
   ```

4. Despliega los archivos actualizados a IIS en la VM.

#### **Verificación del Backend**

Para confirmar que el backend está funcionando correctamente:

1. Conéctate a la VM por **Escritorio Remoto (RDP)**.

2. Abre un navegador dentro de la VM (Chrome, Edge).

3. Visita: `http://localhost:8080/productos/GetAll` (o cualquier endpoint válido).

4. Verifica el estado del servicio:
   ```powershell
   # Si usas NSSM
   nssm status MiServicioSpring
   
   # Verifica el proceso Java
   Get-Process java
   
   # Verifica que el puerto esté escuchando
   netstat -ano | findstr :8080
   ```

**Si necesitas iniciar el backend:**
```powershell
# Con NSSM
nssm start MiServicioSpring

# O manualmente
java -jar C:\App\backend\sbootporlles-0.0.1-SNAPSHOT.jar --spring.profiles.active=prod
```

---

# 👥 **EQUIPO - GRUPO 8**

**Curso:** Cloud Computing y Continuidad  
**Institución:** [Tu institución]  
**Año:** 2025

---

# 📞 **SOPORTE**

Para reportar issues o solicitar features, usa los repositorios de GitHub mencionados arriba.
