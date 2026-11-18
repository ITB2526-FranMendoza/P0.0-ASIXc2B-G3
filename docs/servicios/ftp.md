# Documentación de Configuración: Servidor FTP y Cliente Gráfico

En esta sección se detalla el despliegue completo del servidor FTP en nuestra infraestructura utilizando `vsftpd`, su configuración para permitir transferencias locales seguras, y el desarrollo de un cliente con interfaz gráfica para facilitar su uso.

## 1. Instalación de Servicios FTP

Comenzamos actualizando los repositorios del sistema e instalando el paquete `vsftpd`, que es el demonio encargado de gestionar las conexiones FTP en el servidor.

* Ejecutamos el siguiente comando para la instalación:

  ```bash
  sudo apt update && sudo apt install -y vsftpd
  ```

## 2. Configuración de Permisos en vsftpd

Para que el servidor FTP funcione según los requisitos de nuestra práctica (permitiendo la conexión del usuario local `bchecker` y la subida de archivos), debemos editar su archivo de configuración.

* Visualizamos y editamos el archivo de configuración principal:

  ```bash
  sudo cat /etc/vsftpd.conf
  ```

* Nos aseguramos de que el archivo contenga los siguientes parámetros descomentados o añadidos (configuración aplicada para la máquina `F-G3`):

  ```ini
  listen=NO
  listen_ipv6=YES
  anonymous_enable=NO
  local_enable=YES
  write_enable=YES
  local_umask=022
  dirmessage_enable=YES
  use_localtime=YES
  xferlog_enable=YES
  connect_from_port_20=YES
  chroot_local_user=YES
  allow_writeable_chroot=YES
  secure_chroot_dir=/var/run/vsftpd/empty
  pam_service_name=vsftpd
  rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
  rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
  ssl_enable=NO
  ```

## 3. Reinicio y Comprobación del Servicio

Tras aplicar los cambios en el archivo de configuración, es necesario reiniciar el servicio para que las nuevas reglas surtan efecto.

* Reiniciamos `vsftpd` y comprobamos que está corriendo sin errores:

  ```bash
  sudo systemctl restart vsftpd
  sudo systemctl status vsftpd
  ```

  *(La salida del sistema debe mostrar que el servicio está `active (running)`).*

## 4. Comprobación de Conectividad FTP

Realizamos una prueba de conexión rápida desde la terminal para verificar que el servidor FTP responde correctamente a las peticiones.

* Nos conectamos mediante la IP del servidor e introducimos las credenciales del usuario `bchecker`:

  ```bash
  ftp 192.168.3.40
  ```

  *(Al introducir la contraseña, el servidor nos debe devolver el código `230 Login successful` y confirmarnos que estamos usando el modo binario para transferir archivos).*

## 5. Cliente FTP con Interfaz Gráfica (Desarrollo Propio)

Para mejorar la experiencia de usuario a la hora de subir y descargar archivos, se creó un cliente FTP personalizado con interfaz gráfica.

1. **Script en Python:** Mediante el uso de inteligencia artificial, se desarrolló un script en Python (`mi_ftp.py`). Esta herramienta cuenta con una ventana donde se puede introducir la IP del servidor FTP (por ejemplo, `192.168.3.41`), el usuario (`bchecker`) y la contraseña. Además, incluye un botón para abrir el explorador de archivos y una barra de progreso.
   
2. **Ejecución desde Terminal:** Para arrancar el cliente gráfico desde la terminal de comandos, utilizamos:

   ```bash
   python3 mi_ftp.py
   ```

3. **Creación del Ejecutable Desktop:** Para evitar tener que abrir la terminal cada vez, se solicitó a la IA convertir este script en un archivo ejecutable. Esto generó un acceso directo llamado **"Cliente FTP de Joel"**, permitiendo abrir la interfaz gráfica haciendo doble clic directamente desde el escritorio.
