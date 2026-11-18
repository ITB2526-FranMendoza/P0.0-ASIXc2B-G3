# Documentación de Configuración: SSH y Capa de Seguridad

En esta sección se detalla el proceso íntegro de instalación y configuración del servidor SSH, la creación y gestión del usuario administrador, y la implementación de las medidas de seguridad utilizando UFW, Fail2Ban y Knockd.

## 1. Instalación y Activación del Servidor SSH

El primer paso para habilitar las conexiones remotas es instalar el servidor OpenSSH en nuestra máquina.

*   Actualizamos los repositorios del sistema e instalamos el paquete `openssh-server`:
    ```bash
    sudo apt update && sudo apt install -y openssh-server
    ```

*   Comprobamos que el servicio está activo y lo habilitamos para que arranque automáticamente junto con el sistema operativo:
    ```bash
    sudo systemctl enable --now ssh
    sudo systemctl status ssh
    ```
    *(La salida del comando `status` confirmará que el servicio está en estado `active (running)`).*

## 2. Creación del Usuario Administrador y Permisos

Para gestionar el sistema de forma segura, creamos un usuario específico en lugar de utilizar las cuentas por defecto.

*   Creamos el usuario `bchecker`. Durante la ejecución de este comando, el sistema solicitará asignar una contraseña (se estableció `bchecker121`):
    ```bash
    sudo adduser bchecker
    ```

*   Añadimos el nuevo usuario al grupo `sudo` para otorgarle permisos de administrador, permitiéndole ejecutar comandos con privilegios elevados:
    ```bash
    sudo usermod -aG sudo bchecker
    ```

*   Verificamos el acceso remoto mediante SSH hacia el servidor utilizando la IP asignada a la máquina (`192.168.3.40`) y las credenciales recién creadas:
    ```bash
    ssh bchecker@192.168.3.40
    ```

## 3. Instalación de Herramientas de Seguridad

Para proteger el servidor frente a accesos no autorizados y ataques automatizados, desplegamos una capa de seguridad compuesta por tres herramientas fundamentales: **Fail2Ban** (bloqueo por fuerza bruta), **Knockd** (ocultación de puertos) y **UFW** (cortafuegos).

*   Instalamos los paquetes necesarios:
    ```bash
    sudo apt update && sudo apt install -y fail2ban knockd ufw
    ```

*   Verificamos que las herramientas se han instalado correctamente comprobando sus respectivas versiones y el estado inicial del cortafuegos:
    ```bash
    fail2ban-client --version && knockd --version && sudo ufw status
    ```

## 4. Configuración Base del Cortafuegos (UFW)

Para garantizar que el puerto 22/TCP (SSH) permanezca oculto y cerrado por defecto frente a escaneos de red, establecemos una política restrictiva inicial.

*   Denegamos por defecto todo el tráfico entrante (evitando conexiones externas no deseadas) y permitimos el tráfico saliente (para que el servidor pueda comunicarse con el exterior):
    ```bash
    sudo ufw default deny incoming
    sudo ufw default allow outgoing
    ```
    *(La consola mostrará mensajes confirmando que las políticas por omisión han cambiado a "deny" y "allow").*

## 5. Prueba de Funcionamiento y Bloqueo con Fail2Ban

Para asegurar que Fail2Ban está monitorizando el servicio SSH y bloqueando ataques reales, realizamos una prueba forzando intentos fallidos de inicio de sesión.


*   Tras los intentos fallidos, comprobamos el estado de la jaula `sshd` en Fail2Ban para verificar si ha detectado la intrusión:
    ```bash
    sudo fail2ban-client status sshd
    ```
    
    **Resultado esperado:**
    En la salida de este comando, bajo la sección de `Actions`, se debe confirmar que hay al menos 1 IP baneada actualmente y, en la lista `Banned IP list`, debe aparecer la IP desde la que realizamos el ataque simulado (`127.0.0.1`).
