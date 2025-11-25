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

Comprueba la pagina por defecto de Apache en: `http://localhost`

### 3.  
