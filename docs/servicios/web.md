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

<img width="631" height="237" alt="image" src="https://github.com/user-attachments/assets/8daefa6b-3c12-4ac9-a733-04bfa14a9f78" />

Creamos una página web de prueba para probar el servicio:

<img width="636" height="603" alt="image" src="https://github.com/user-attachments/assets/8ddbcd24-4041-426a-a6c3-4b33bd29a85d" />

<img width="630" height="410" alt="image" src="https://github.com/user-attachments/assets/e081c468-da4a-44d4-8fcb-084836ccb0b9" />

Verificación de la página web desde el navegador:

<img width="420" height="438" alt="image" src="https://github.com/user-attachments/assets/4a96f62d-2f29-4948-85fa-d014c2c2eaf7" />

---

## Sprint 2

### Prueba de conexión web - BBDD

Comprobar conectividad de red y resolución de B-G3

B-G3:

<img width="631" height="203" alt="image" src="https://github.com/user-attachments/assets/e1a32d89-40e0-4159-8ef8-37d534929120" />

W-G3:

<img width="628" height="201" alt="image" src="https://github.com/user-attachments/assets/bff98abe-7912-4663-b1c2-b412fec75961" />

Probar conexión directa al puerto MySQL de B-G3

Desde la terminal de W-JFR, Nos conectamos de forma remota al servidor MySQL de B-G3 con el usuario bchecker (contraseña: bchecker121)

<img width="634" height="137" alt="image" src="https://github.com/user-attachments/assets/ed2d3ff6-4dd0-43f8-ad38-15c3cca5a259" />

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
