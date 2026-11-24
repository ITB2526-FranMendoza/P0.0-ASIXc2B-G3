# Documentación de base de datos

[jesús [itb] palma palma](mailto:jesus.palma.7e7@itb.cat)

→ [P0.0-ASIXc2B-JFR03](https://docs.google.com/document/d/1G2iMPUoTqZ9xYHPl9700BkoYyDBON7XAZxHKtVs94bY/edit?tab=t.ts0frwp0utc4) ←

→ [Github (Markdown)](https://github.com/ITB2526-FranMendoza/P0.0-ASIXc2B-G3) ←

## Sprint 1

| Tarea | Descripción |
|---|---|
| Preparar servidor BBDD B-JFR e instalar MySQL | Desplegar el equipo B-JFR e instalar MySQL |

### Preparar servidor BBDD B-JFR e instalar MySQL

Creación del usuario (ARCA):

<img width="480" height="111" alt="image" src="https://github.com/user-attachments/assets/878aaf3c-a181-4481-b55c-88a68bec561a" />

Pasos para la instalación correcta del (MySQL):

Instalación el paquete de MySQL Server:

<img width="631" height="381" alt="image" src="https://github.com/user-attachments/assets/a88d308d-e40c-45e5-bcfa-203d323ce628" />

Verificar que el servicio esté activo:

<img width="631" height="279" alt="image" src="https://github.com/user-attachments/assets/5563b65e-67e9-41cb-a6f2-d986e5a5c4d9" />

Ejecutar el asistente de seguridad inicial:

<img width="631" height="277" alt="image" src="https://github.com/user-attachments/assets/cc0a5e4c-1ac4-455d-8e77-f9b0187b454b" />

**Estos son los pasos que he seguido para la configuracion del MySQL**

1. New password for root (Contraseña para root): Te pedirá definir o confirmar una contraseña para el usuario administrador de MySQL (`root`). Escríbela y presiona Enter (los caracteres no se mostrarán al escribir por seguridad).
2. Remove anonymous users? (¿Eliminar usuarios anónimos?): Escribe y y presiona Enter.
3. Disallow root login remotely? (¿Deshabilitar acceso remoto a root?): Escribe y y presiona Enter.
4. Remove test database and access to it? (¿Eliminar base de datos de prueba?): Escribe y y presiona Enter.
5. Reload privilege tables now? (¿Recargar tablas de privilegios?): Escribe y y presiona Enter.

Explicacion:

**VALIDATE PASSWORD COMPONENT → Elegir N (o Y en producción)**

- **Para qué sirve:** Audita la complejidad de las contraseñas creadas dentro de MySQL (exigiendo mayúsculas, números, caracteres especiales y cierta longitud).
- **Por qué responder N en desarrollo:** Evita que el sistema rechace contraseñas sencillas mientras estás aprendiendo, programando o realizando pruebas locales. En entornos reales/servidores web se recomienda Y para forzar la seguridad.

**Remove anonymous users? → Elegir Y**

- **Para qué sirve:** Elimina cuentas de usuario anónimas creadas por defecto para pruebas iniciales.
- **Por qué hacerlo así:** Impide que personas no autenticadas puedan ingresar al servidor de base de datos sin un nombre de usuario y contraseña válidos.

**Disallow root login remotely? → Elegir Y**

- **Para qué sirve:** Bloquea la posibilidad de que el usuario administrador (root) de MySQL inicie sesión desde una máquina remota a través de la red.
- **Por qué hacerlo así:** Previene ataques externos por fuerza bruta. Obliga a que cualquier tarea administrativa se realice localmente dentro del propio servidor.

**Remove test database and access to it? → Elegir Y**

- **Para qué sirve:** Elimina la base de datos predeterminada llamada test a la que cualquiera tiene acceso.
- **Por qué hacerlo así:** Limpia el entorno de archivos de prueba innecesarios y elimina un posible punto de entrada no protegido.

**Reload privilege tables now? → Elegir Y**

- **Para qué sirve:** Recarga las tablas de permisos en la memoria RAM del servidor.
- **Por qué hacerlo así:** Aplica de inmediato todos los cambios de seguridad configurados en los pasos anteriores sin necesidad de reiniciar completamente el servicio MySQL.

Acceder a la consola de administración de MySQL:

<img width="632" height="249" alt="image" src="https://github.com/user-attachments/assets/60ab0625-fd3f-44ea-ac4f-772de91e646c" />


---

## Sprint 2

| Tarea | Descripción |
|---|---|
| Descargar y analizar el CSV de equipamientos de educación | Descargar el CSV de equipamientos de educación de Barcelona (Open Data BCN) y revisar sus columnas |

### Descargar y analizar el CSV de equipamientos de educación

**Obtener el archivo CSV de Open Data BCN**

Este se puede descargar desde la web oficial de Open Data BCN buscando el conjunto de datos "Equipaments d'educació i formació de la ciutat de Barcelona"

O bien también se puede descargar desde la línea de comando como lo he hecho yo: [https://opendata-ajuntament.barcelona.cat/data/es/dataset/equipament-educacio](https://opendata-ajuntament.barcelona.cat/data/es/dataset/equipament-educacio)

<img width="634" height="324" alt="image" src="https://github.com/user-attachments/assets/55211852-4190-49a0-9871-7c1e32b96c61" />

**Inspeccionar la estructura del archivo y sus columnas**

Ver los nombres de todas las columnas:

<img width="628" height="191" alt="image" src="https://github.com/user-attachments/assets/ec58ee5e-e07a-4d77-a906-8d2838c128ef" />

**Listado de columnas obtenidas del archivo:**

register_id: Identificador único del equipamiento.  
name: Nombre del centro de educación o formación.  
addresses_road_name: Calle o vía donde se ubica.  
addresses_district_name: Nombre del distrito de Barcelona.  
addresses_neighborhood_name: Nombre del barrio.  
geo_epgs_4326_lat / geo_epgs_4326_lon: Coordenadas geográficas de latitud y longitud.

Ver filas con datos reales (Para ver como lucen las entradas)

<img width="631" height="345" alt="image" src="https://github.com/user-attachments/assets/b111112e-d0f5-4ed8-bd94-85412c4543a1" />

**Contar cuántos registros/filas tiene el archivo:**

<img width="448" height="64" alt="image" src="https://github.com/user-attachments/assets/f943a755-c534-4b86-af62-b98bb8a4f642" />

### Configurar B-G3 y usuario bchecker

**Creación del usuario bchecker con la contraseña bchecker121 y privilegios (tanto en local como desde cualquier IP remota):**

<img width="583" height="230" alt="image" src="https://github.com/user-attachments/assets/80d2388f-828f-47cd-a19a-bff24d612e82" />

**Comprobamos del que el usuario se creado correctamente:**

<img width="583" height="214" alt="image" src="https://github.com/user-attachments/assets/fa8d82df-199c-4caf-bc2c-d7eee7a48503" />

### Cargar el CSV en la BBDD

**Convertir el CSV a UTF-8 y moverlo a la carpeta de MySQL**

Open Data BCN viene codificado en UTF-16 (por eso aparecían los símbolos `` en las tildes). Para convertirlo a UTF-8 hacemos estos comandos:

<img width="584" height="46" alt="image" src="https://github.com/user-attachments/assets/ce5c3f1d-9f25-4207-b3d4-37caf054a17d" />

**Crear la base de datos y la tabla principal**

Creamos la base de datos BDD_G3 la tabla equipamientos con las 40 columnas exactas del archivo CSV:

```sql
CREATE DATABASE IF NOT EXISTS BDD_G3 CHARACTER SET utf8mb4 COLLATE
utf8mb4_unicode_ci;
USE BDD_G3;

CREATE TABLE equipamientos (
   id INT AUTO_INCREMENT PRIMARY KEY,
   register_id VARCHAR(50),
   name VARCHAR(255),
   institution_id VARCHAR(50),
   institution_name VARCHAR(255),
   created VARCHAR(50),
   modified VARCHAR(50),
   addresses_roadtype_id VARCHAR(50),
   addresses_roadtype_name VARCHAR(100),
   addresses_road_id VARCHAR(50),
   addresses_road_name VARCHAR(255),
   addresses_start_street_number VARCHAR(50),
   addresses_end_street_number VARCHAR(50),
   addresses_neighborhood_id VARCHAR(50),
   addresses_neighborhood_name VARCHAR(100),
   addresses_district_id VARCHAR(50),
   addresses_district_name VARCHAR(100),
   addresses_zip_code VARCHAR(20),
   addresses_town VARCHAR(100),
   addresses_main_address VARCHAR(20),
   addresses_type VARCHAR(50),
   values_id VARCHAR(50),
   values_attribute_id VARCHAR(50),
   values_category VARCHAR(100),
   values_attribute_name VARCHAR(100),
   values_value VARCHAR(255),
   values_outstanding VARCHAR(20),
   values_description TEXT,
   secondary_filters_id VARCHAR(50),
   secondary_filters_name VARCHAR(255),
   secondary_filters_fullpath TEXT,
   secondary_filters_tree VARCHAR(50),
   secondary_filters_asia_id VARCHAR(50),
   geo_epgs_25831_x VARCHAR(50),
   geo_epgs_25831_y VARCHAR(50),
   geo_epgs_4326_lat VARCHAR(50),
   geo_epgs_4326_lon VARCHAR(50),
   estimated_dates VARCHAR(100),
   start_date VARCHAR(50),
   end_date VARCHAR(50),
   timetable TEXT
);
```

**Importar los datos del CSV**

```sql
USE BDD_G3;

SET SESSION sql_mode = '';

LOAD DATA INFILE '/var/lib/mysql-files/equipamientos_utf8.csv'
INTO TABLE equipamientos
CHARACTER SET utf8mb4
FIELDS TERMINATED BY ','
OPTIONALLY ENCLOSED BY '"'
ESCAPED BY ''
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
(
   register_id, name, institution_id, institution_name, created, modified,
   addresses_roadtype_id, addresses_roadtype_name, addresses_road_id,
addresses_road_name,
   addresses_start_street_number, addresses_end_street_number,
   addresses_neighborhood_id, addresses_neighborhood_name,
   addresses_district_id, addresses_district_name, addresses_zip_code,
addresses_town,
   addresses_main_address, addresses_type,
   values_id, values_attribute_id, values_category, values_attribute_name,
   values_value, values_outstanding, values_description,
   secondary_filters_id, secondary_filters_name, secondary_filters_fullpath,
   secondary_filters_tree, secondary_filters_asia_id,
   geo_epgs_25831_x, geo_epgs_25831_y, geo_epgs_4326_lat, geo_epgs_4326_lon,
   estimated_dates, start_date, end_date, timetable
);
```

**Comprobación de cuantos datos hay en la tabla:**

<img width="583" height="144" alt="image" src="https://github.com/user-attachments/assets/4ed8afe4-d8c6-4d26-8bcf-d79868e20dac" />

**Crear tablas normalizadas adicionales**

```sql
CREATE TABLE distritos (
   district_id VARCHAR(50) PRIMARY KEY,
   district_name VARCHAR(100) NOT NULL
);

INSERT IGNORE INTO distritos (district_id, district_name)
SELECT DISTINCT addresses_district_id, addresses_district_name
FROM equipamientos
WHERE addresses_district_id <> '';

CREATE TABLE barrios (
   neighborhood_id VARCHAR(50) PRIMARY KEY,
   neighborhood_name VARCHAR(100) NOT NULL,
   district_id VARCHAR(50),
   FOREIGN KEY (district_id) REFERENCES distritos(district_id)
);

INSERT IGNORE INTO barrios (neighborhood_id, neighborhood_name, district_id)
SELECT DISTINCT addresses_neighborhood_id, addresses_neighborhood_name,
addresses_district_id
FROM equipamientos
WHERE addresses_neighborhood_id <> '';
```

### Prueba de conexión web - BBDD

Comprobar conectividad de red y resolución de B-G3

B-G3:

<img width="628" height="200" alt="image" src="https://github.com/user-attachments/assets/2e1f8096-66f7-4c85-a713-03f322e5aecf" />

W-G3:

<img width="628" height="203" alt="image" src="https://github.com/user-attachments/assets/9b1729c5-6ec9-41d2-9feb-88c536c587b6" />

Probar conexión directa al puerto MySQL de B-G3

Desde la terminal de W-JFR, Nos conectamos de forma remota al servidor MySQL de B-G3 con el usuario bchecker (contraseña: bchecker121)

<img width="630" height="135" alt="image" src="https://github.com/user-attachments/assets/f012da48-915a-4e38-b437-465297fcbab6" />

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

<img width="633" height="375" alt="image" src="https://github.com/user-attachments/assets/26518536-2cf2-4853-ab14-6062fee413ac" />

