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

## 📤 **Frontend (IIS - Puerto 80)**

El frontend se desplegó en **IIS (Internet Information Services)** en la misma VM donde está el backend, utilizando el puerto 80. Esto permite que el frontend y backend se comuniquen mediante rutas relativas aprovechando la configuración de proxy inverso.

### 1. Configurar URL de Producción

Edita `Frontend/src/environments/environment.prod.ts` para usar rutas relativas:

```typescript
export const environment = {
  production: true,
  apiUrl: '/api', 
  uploadUrl: '/api/upload'
};
```

### 2. Build de Producción

```bash
cd Frontend
ng build --configuration production
```

La carpeta de distribución se generará en: `Frontend/dist/proyectosoluciones/browser/`

### 3. Subir archivos a la VM

Transferimos todo el contenido de la carpeta `dist/proyectosoluciones/browser/` a la VM en la ruta:

```
C:\inetpub\wwwroot\
```

### 4. Configurar el Sitio Web en IIS

1. Abrimos **IIS Manager** en la VM.
2. En el panel izquierdo, expandemos **Sites**.
3. Hacemos clic derecho en **Default Web Site** → **Edit Bindings**.
5. Verificamos que esté configurado en **Puerto 80** para HTTP.

### 5. Configurar web.config para SPA

En `C:\inetpub\wwwroot\`, creamos o editamos el archivo `web.config` para habilitar el enrutamiento de Angular y el proxy inverso:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <!-- Redirección de rutas /api al backend en puerto 8080 -->
    <rewrite>
      <rules>
        <rule name="API Proxy" stopProcessing="true">
          <match url="^api/(.*)" />
          <action type="Rewrite" url="http://localhost:8080/api/{R:1}" />
        </rule>
        <!-- SPA - Redirigir todas las rutas a index.html -->
        <rule name="Angular Routes" stopProcessing="true">
          <match url=".*" />
          <conditions logicalGrouping="MatchAll">
            <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
            <add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" />
            <add input="{REQUEST_URI}" pattern="^/api" negate="true" />
          </conditions>
          <action type="Rewrite" url="/index.html" />
        </rule>
      </rules>
    </rewrite>
    
    <!-- Tipos MIME para Angular -->
    <staticContent>
      <mimeMap fileExtension=".json" mimeType="application/json" />
      <mimeMap fileExtension=".woff" mimeType="application/font-woff" />
      <mimeMap fileExtension=".woff2" mimeType="application/font-woff2" />
    </staticContent>
  </system.webServer>
</configuration>
```

### 6. Instalar URL Rewrite Module (si no está instalado)

El módulo URL Rewrite es necesario para que funcione el proxy inverso:

1. Descargamos desde: https://www.iis.net/downloads/microsoft/url-rewrite
2. Instalamos el módulo en IIS.
3. Reiniciamos IIS:
   ```powershell
   iisreset
   ```

### 7. Verificación

Accedemos al frontend desde tu navegador:

```
http://34.176.162.36
```

## 📤 **Base de Datos (Cloud SQL)**

En este proyecto, el backend no se conecta a la base de datos mediante una IP pública. En su lugar, se utiliza la arquitectura recomendada por Google: un servidor de Compute Engine (VM) que se conecta de forma segura a la base de datos a través del **Cloud SQL Auth Proxy usando IP Privada**.

### 1. Creación de la Instancia de Cloud SQL

1. Vamos a la **Consola de Google Cloud → SQL**.
2. Hacemos clic en **"Crear instancia"** y elige **MySQL**.

**Configuración para Producción:**

- Establecemos una **contraseña** para el usuario `root`.
- En **"Elige la región y la disponibilidad zonal"**, seleccionamos **"Varias zonas (con alta disponibilidad)"**. Esto crea una réplica para tolerancia a fallos.
- En **"Personaliza tu instancia"**, ajustamnos los núcleos (vCPU) y la RAM a un tamaño adecuado para empezar (ej. 2 vCPU, 8 GB RAM).

### 2. Configuración de Red (IP Privada)

Para que la VM y la BD se comuniquen internamente:

1. Dentro de la instancia de Cloud SQL, vamos al menú **"Conexiones"**.
2. Vamos a la pestaña **"Redes"**.
3. Marcamos la casilla **"IP privada"**.
4. En el menú desplegable **"Red"**, seleccionamos `default` (o la red VPC donde reside tu VM).
5. Guardamos los cambios de la instancia de Cloud SQL.

### 3. Configuración de Permisos de la VM (Compute Engine)

La VM necesita permiso para autenticarse con la API de Cloud SQL:

1. Vamos al menú  **Compute Engine → Instancias de VM**.
2. **Detenemos la VM**.
3. Una vez detenida, haz clic en su nombre para entrar a los detalles y haz clic en **"Editar"**.
4. Busca la sección **"Identidad y acceso a las API"**.
5. En **"Permisos de acceso"**, cambia la configuración a **"Permitir acceso completo a todas las API de Cloud"**.
6. Guarda los cambios e **Inicia la VM**.

### 4. Configuración del Cloud SQL Auth Proxy (En la VM)

El proxy es un "túnel" seguro que se ejecuta en la VM y se conecta a la BD.

1. **Descargamos el ejecutable del Cloud SQL Auth Proxy** (`cloud-sql-proxy.exe`) en la VM de Windows desde: https://cloud.google.com/sql/docs/mysql/sql-proxy
2. Obtenemos el **"Nombre de conexión de la instancia"** desde la página de "Descripción general" de la instancia de Cloud SQL.
3. Ejecutamos el proxy. Para producción, se recomienda configurarlo como un **servicio de Windows**

**Comando para ejecutar el proxy:**

```bash
.\cloud-sql-proxy.exe --private-ip --port 3306 proyectocloudcomputing-475904:southamerica-west1:porlles-bd
```

- `--private-ip` fuerza al proxy a usar la conexión de red interna que configuramos.
- `--port 3306` hace que el proxy escuche en `localhost:3306`.

---

# 👥 **EQUIPO - GRUPO 8**

**Curso:** Cloud Computing y Continuidad  
**Institución:** Universidad Privada del Norte  
**Participantes:** Sebastian Dongo Quezada, Karen Rozas Valera, Alejandro Palma Tafur, Fabrizio Reyna Arce, Omar Palomino Galvez
**Año:** 2025


