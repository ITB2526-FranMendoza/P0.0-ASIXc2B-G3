# Documentación de servicios web

[jesús [itb] palma palma](mailto:jesus.palma.7e7@itb.cat)

→ [P0.0-ASIXc2B-JFR03](https://docs.google.com/document/d/1G2iMPUoTqZ9xYHPl9700BkoYyDBON7XAZxHKtVs94bY/edit?tab=t.ts0frwp0utc4) ←

→ [Github (Markdown)](https://github.com/ITB2526-FranMendoza/P0.0-ASIXc2B-G3) ←

## Sprint 1

| Tarea | Descripción |
|---|---|
| Servidor web W-JFR | Desplegar el servidor web W-JFR y crear el usuario bchecker |

### Servidor web W-JFR

Creación del usuario:

![whoami en W-G3 como bchecker](images/s1-01-whoami-bchecker.png)

Con sudo sudo adduser bchecker he creado el usuario bchecker que va a ser nuestro usuario del servidor WEB luego de tener el usuario creado lo añadimos al grupo sudo:

![sudo usermod -aG sudo bchecker](images/s1-02-usermod-sudo.png)

Cambio del hostname:

![hostname actual: W-G3](images/s1-03-hostname.png)

Para cambiar el hostname utilizaremos este comando (`sudo hostnamectl set-hostname W-G3`)

Entrar en (sudo nano /etc/hosts) para Asociar el nuevo nombre de la máquina (W-G3) a su IP local para que el sistema y comandos como sudo la reconozcan sin errores.

![Fichero /etc/hosts con la línea 127.0.1.1 W-G3](images/s1-04-etc-hosts.png)

Desplegacion del servidor web

Instalar el servidor web Apache:

![sudo apt install apache2 -y](images/s1-05-apache-install.png)

Verificación de que el servidor está arrancado:

![sudo systemctl status apache2](images/s1-06-apache-status.png)

Creamos una página web de prueba para probar el servicio:

![/var/www/html/index.html (parte 1)](images/s1-07-index-html-parte1.png)

![/var/www/html/index.html (parte 2)](images/s1-08-index-html-parte2.png)

Verificación de la página web desde el navegador:

![Página de prueba en localhost](images/s1-09-navegador-localhost.png)

---

## Sprint 2

### Prueba de conexión web - BBDD

Comprobar conectividad de red y resolución de B-G3

B-G3:

![ping desde B-G3 a 192.168.3.12](images/s2-09-ping-b-g3.png)

W-G3:

![ping desde W-G3 a 192.168.3.11](images/s2-10-ping-w-g3.png)

Probar conexión directa al puerto MySQL de B-G3

Desde la terminal de W-JFR, Nos conectamos de forma remota al servidor MySQL de B-G3 con el usuario bchecker (contraseña: bchecker121)

![mysql -h B-G3 -u bchecker -p desde W-G3](images/s2-11-mysql-remoto.png)

Probar la conexión desde la aplicación web (Apache + PHP →MySQL)

Para demostrar que el servidor web en sí (y no solo la terminal) puede consultar la base de datos de B-G3, creamos este script de prueba en /var/www/html/test_db.php:

```bash
sudo bash -c 'cat << '\''EOF'\'' > /var/www/html/test_db.php
<?php
$host = "B-G3";
$user = "bchecker";
$pass = "bchecker121";
$db = "BDD_G3";

$conn = new mysqli($host, $user, $pass, $db);

if ($conn->connect_error) {
    die("<h1>Error de conexión con B-G3:</h1><p>" . $conn->connect_error . "</p>");
}

$res = $conn->query("SELECT COUNT(*) AS total FROM equipamientos");
$row = $res->fetch_assoc();

echo "<h1>Conexión exitosa: W-JFR -> B-G3</h1>";
echo "<p>Base de datos: <strong>$db</strong></p>";
echo "<p>Total de equipamientos registrados: <strong>" . $row["total"] . "</strong></p>";

$conn->close();
?>
EOF'
```

Verificacion:

![test_db.php en el navegador](images/s2-12-test-db-php.png)
