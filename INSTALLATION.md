# INSTALACION DE LAMP

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






