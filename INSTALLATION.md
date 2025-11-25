# INSTALACIÓN DE LAMP

### 1. Primero has de actualizar el sistema por si hay posibles actualizaciones las cuales te den problemas a la hora de instalar todo.

```bash
sudo apt update && sudo apt upgrade -y
```

### 2. A continuación instala apache con el siguiente comando:
   
```bash 
sudo apt install apache2 -y
```

> *Activa e inicialo*

```bash
sudo systemctl enable apache2
sudo systemctl start apache2
```

> *verifica el estado*

```bash
sudo systemctl status apache2
```

Comprueba la página por defecto de Apache en: `http://localhost`

### 3.  Lo siguiente será instalar MySQL

```bash
sudo apt install mysql-server mysql-client -y
```

> *Inicia y habilita el servicio*

```bash
sudo systemctl enable mysql
sudo systemctl start mysql
```

## **Configuración MySQL**

> *Accede a su consola:*

```bash
sudo mysql
```

> *Creación base de datos*

```sql
CREATE DATABASE bbdd;
```

> *Creación de usuario local*

```sql
CREATE USER 'usuario'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
GRANT ALL PRIVILEGES ON bbdd.* TO 'usuario'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

**NOTA:** Esto solo funciona para conectarse desde el servidor local (*localhost*)

### 4. Como ultimo paso de Instalación de LAMP, instalaremos PHP y sus extensiones comunes

> *La versión de Ubuntu 24.04 ya incluye PHP 8.3 a sus repositorios estándares

```bash
sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-zip php-json php-cli -y
```

> *Reinicia Apache para cargar PHP*

```bash
sudo systemctl restart apache2
```

> *Verificamos la versión de PHP:*

```bash
php -v
```

> *Por ultimo creamos un fitxero de prueba y lo comprobamos en: `http://localhost/info.php`*

```bash
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```

> **(puedes eliminar este fitxero creado como prueba con:)**

```bash
sudo rm /var/www/html/info.php
```

# Configuración de VirtualHost con Apache2

### 1. Crearemos una estructura de directorios para organizar mejor los sitios web:

> *Directorio para el dominio: `domini.local`*

```bash
sudo mkdir -p /var/www/domini.local
```

### 2. Lo siguiente será definir el VirtualHost

> *Crearemos un fitxero de configuración para nuestro VirtualHost dentro del siguiente directorio: `/etc/apache2/sites-available/`:*

```bash
sudo nano /etc/apache2/sites-available/domini.local.conf
```

> *Antes de añadir la configuración tener en cuenta que hay que substituir `domini.local`por tu nombre de dominio:

```apache
<VirtualHost *:80>
    ServerAdmin admin@domini.local
    ServerName www.domini.local
    ServerAlias domini.local
    DocumentRoot /var/www/domini.local
    ErrorLog ${APACHE_LOG_DIR}/domini.local_error.log
    CustomLog ${APACHE_LOG_DIR}/domini.local_access.log combined
</VirtualHost>
```

### 3. A continuación habilitaremos el VirtualHost

```bash
sudo a2ensite domini.local.conf
```

### 4. Reiniciaremos Apache2

```bash
sudo systemctl restart apache2
```

### 5. Modificaremos `/etc/hosts` para resolver el dominio localmente

```bash
sudo nano /etc/hosts
```

> *Agrega la siguiente linea:*

```
127.0.0.1   www.domini.local domini.local
```

### 6. Comprobamos que funcione en el navegador

```
http://www.domini.local
```

> Si el directorio `/var/www/domini.local` esta vacio, Apache te mostrará el error 404, por tanto para comprobar que funciona crea un fitxero de prueba:

```bash
echo "<h1>Hola, benvingut domini.local</h1>" | sudo tee /var/www/domini.local/index.html
```

### 7. Solución de problemas: Registros de Apache2

#### Registro de errores (contiene mensajes sobre los errores de la configuración):

```bash
sudo tail -f /var/log/apache2/domini.local_error.log
```
