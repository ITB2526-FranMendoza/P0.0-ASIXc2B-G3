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

<img width="239" height="75" alt="image" src="https://github.com/user-attachments/assets/8fdfbae5-4cc8-449d-a032-9a3559a149f8" />

Con sudo sudo adduser bchecker he creado el usuario bchecker que va a ser nuestro usuario del servidor WEB luego de tener el usuario creado lo añadimos al grupo sudo:

<img width="481" height="58" alt="image" src="https://github.com/user-attachments/assets/d8b37f51-e206-4fc6-9492-b972e1d79eac" />

Cambio del hostname:

<img width="260" height="52" alt="image" src="https://github.com/user-attachments/assets/c1498872-9acb-4d26-8c9c-c9f747e3365b" />

Para cambiar el hostname utilizaremos este comando (`sudo hostnamectl set-hostname W-G3`)

Entrar en (sudo nano /etc/hosts) para Asociar el nuevo nombre de la máquina (W-G3) a su IP local para que el sistema y comandos como sudo la reconozcan sin errores.

<img width="616" height="233" alt="image" src="https://github.com/user-attachments/assets/c9d3e042-e6c3-4c5f-8403-d7effdcc8b75" />

Desplegacion del servidor web

Instalar el servidor web Apache:

<img width="633" height="205" alt="image" src="https://github.com/user-attachments/assets/d78f05a0-3e9d-449a-9d1f-59e92e5723ca" />

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
