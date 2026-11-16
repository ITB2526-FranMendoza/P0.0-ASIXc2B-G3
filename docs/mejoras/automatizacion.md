# Automatización del despliegue (`deploy.sh`)

> **Objetivo:** montar cualquier equipo del proyecto (R-G3, B-G3, W-G3, SSH-G3, F-G3) con **un solo comando**, en segundos y de forma repetible.
> **Script:** [`scripts/deploy.sh`](../scripts/deploy.sh) · **Log de cada ejecución:** `/var/log/deploy-g3.log`

## Qué hace el script

| Rol | Equipo | IP | Qué automatiza |
|-----|--------|----|----------------|
| `r` | R-G3 | 192.168.3.30 | `ip_forward` + NAT con nftables (masquerade por `enp1s0`) |
| `b` | B-G3 | 192.168.3.11 | MySQL, usuario `bchecker`, descarga del CSV, conversión a UTF-8 y carga en `equipaments.equipaments_educacio` |
| `w` | W-G3 | 192.168.3.12 | Apache2 + página de prueba (no pisa una web ya existente) |
| `ssh` | SSH-G3 | 192.168.3.40 | OpenSSH + usuario `bchecker` |
| `f` | F-G3 | 192.168.3.41 | vsftpd (sin anónimos, usuarios locales enjaulados) |

**En todos los roles:** hostname + `/etc/hosts`, usuario `bchecker` (`bchecker121`) en `sudo`, SSH activo, IP fija por netplan y una **verificación final** automática.

**Es idempotente:** se puede ejecutar varias veces y el resultado es el mismo.

**Orden:** primero `r` (si no, los demás no tienen Internet para instalar paquetes), después el resto.

---

## Paso 1 · Preparar el script en el repositorio

En **PC-LIN-G3**, dentro del repo clonado:

```bash
cd ~/P0.0-ASIXc2B-G3
mkdir -p scripts docs
cp ~/deploy.sh scripts/deploy.sh      # ajusta la ruta de origen si es otra
bash -n scripts/deploy.sh && echo "SINTAXIS OK"
ls -l scripts
```

📸 **CAPTURA 1:** salida con `SINTAXIS OK` y el `ls -l scripts`.

**Texto bajo la captura:**
> Copiamos el script a la carpeta `scripts/` del repositorio y comprobamos con `bash -n` que no tiene errores de sintaxis antes de usarlo.

---

## Paso 2 · Subirlo a GitHub

```bash
git add scripts/deploy.sh
git commit -m "Añadido script de despliegue automatizado deploy.sh"
git push
```

📸 **CAPTURA 2:** salida del `git push` (y, si quieres, el archivo visible en GitHub).

**Texto bajo la captura:**
> Versionamos el script con `git add`, `git commit -m` y `git push` (autenticación por clave pública/privada). Así cualquier equipo puede obtenerlo con `git clone` o `git pull`.

---

## Paso 3 · Copiar el script al router y ejecutarlo (rol `r`)

Desde PC-LIN-G3:

```bash
scp scripts/deploy.sh bchecker@192.168.3.30:~/
ssh bchecker@192.168.3.30
```

Ya dentro de R-G3:

```bash
sudo bash deploy.sh r
```

> Si la sesión SSH parpadea al aplicar la red es normal: la IP no cambia, solo se reaplica netplan.

📸 **CAPTURA 3:** el `scp` y el inicio de la sesión SSH en R-G3.
📸 **CAPTURA 4:** el final de la ejecución: bloque `--- VERIFICACIÓN (r) ---` y `===== RESUMEN R-G3 =====`.

**Texto bajo la captura 3:**
> Copiamos el script al router por SCP con el usuario `bchecker` y entramos por SSH.

**Texto bajo la captura 4:**
> El script ha puesto el hostname `R-G3`, la IP `192.168.3.30/24`, ha activado `ip_forward` y ha creado la regla NAT (`masquerade` por `enp1s0`). La verificación final confirma `ip_forward: 1` y la regla en nftables.

---

## Paso 4 · Comprobar que el NAT funciona desde un cliente

En **PC-LIN-G3** (con gateway `192.168.3.30`):

```bash
ip route get 8.8.8.8
ping -c 3 8.8.8.8
```

📸 **CAPTURA 5:** salida de ambos comandos.

**Texto bajo la captura:**
> El tráfico sale por `192.168.3.30` (R-G3) y hay respuesta de Internet: el router hace NAT correctamente.

---

## Paso 5 · Base de datos (rol `b`)

```bash
scp scripts/deploy.sh bchecker@192.168.3.11:~/
ssh bchecker@192.168.3.11
sudo bash deploy.sh b
```

📸 **CAPTURA 6:** líneas `[OK] MySQL...`, `[OK] CSV descargado (codificación original: ...)` y `[OK] Datos cargados: N filas ...` (el script también muestra las líneas de datos del CSV para comparar).

Comprobación de los datos, ya en B-G3:

```bash
mysql -u bchecker -pbchecker121 -e "SELECT COUNT(*) FROM equipaments.equipaments_educacio; SELECT name, addresses_district_name FROM equipaments.equipaments_educacio LIMIT 5;"
```

📸 **CAPTURA 7:** resultado del `SELECT`.

Comprobación del acceso remoto, desde PC-LIN-G3:

```bash
sudo apt install -y mysql-client
mysql -h 192.168.3.11 -u bchecker -pbchecker121 -e "SHOW DATABASES;"
```

📸 **CAPTURA 8:** aparece la base `equipaments`.

**Texto bajo las capturas:**
> El script instala MySQL, crea `bchecker` (local y remoto), descarga el CSV de Open Data BCN, detecta su codificación (UTF-16/UTF-8/Latin-1), lo convierte a UTF-8, crea la tabla a partir de la cabecera y carga los datos con `LOAD DATA INFILE`. El número de filas coincide con las líneas de datos del CSV. Con `bind-address = 0.0.0.0` la base es accesible desde la Intranet.

---

## Paso 6 · Servidor web (rol `w`)

```bash
scp scripts/deploy.sh bchecker@192.168.3.12:~/
ssh bchecker@192.168.3.12
sudo bash deploy.sh w
```

Desde PC-LIN-G3:

```bash
curl -I http://192.168.3.12
```

📸 **CAPTURA 9:** final de la ejecución (`apache2: active`).
📸 **CAPTURA 10:** `curl -I` con `HTTP/1.1 200 OK` (o la web en el navegador).

**Texto bajo las capturas:**
> Se instala Apache y se activa al arranque. El script solo sustituye la página por defecto: si ya existe una web propia (la app de Jesús), no la pisa. El `200 OK` desde el cliente confirma el acceso a través de la red.

---

## Paso 7 · SSH y FTP (roles `ssh` y `f`)

```bash
# SSH-G3
scp scripts/deploy.sh bchecker@192.168.3.40:~/
ssh bchecker@192.168.3.40 "sudo bash deploy.sh ssh"

# F-G3
scp scripts/deploy.sh bchecker@192.168.3.41:~/
ssh bchecker@192.168.3.41 "sudo bash deploy.sh f"
```

> Si `sudo` pide la contraseña por una sesión sin terminal, entra por SSH normal y ejecuta el comando dentro.

Prueba del FTP desde el cliente:

```bash
ftp 192.168.3.41
```
(usuario `bchecker`, contraseña `bchecker121`)

📸 **CAPTURA 11:** verificación final de `ssh` y de `f` (`vsftpd: active`, puerto 21).
📸 **CAPTURA 12:** login FTP con `230 Login successful`.

**Texto bajo las capturas:**
> Cada equipo queda con su hostname e IP del diagrama y el usuario `bchecker`. El FTP rechaza usuarios anónimos y enjaula a los locales; el `230 Login successful` demuestra que funciona.

---

## Paso 8 · Demostrar que es idempotente

Vuelve a ejecutar un rol sobre un equipo ya desplegado (por ejemplo el web):

```bash
sudo bash deploy.sh w
```

📸 **CAPTURA 13:** segunda ejecución sin errores y con el mismo resultado.

**Texto bajo la captura:**
> Al repetir el script no hay errores ni duplicados (usuario, hostname y servicios no se recrean). Se puede relanzar siempre que haga falta.

---

## Paso 9 · Log de despliegues

```bash
tail -n 25 /var/log/deploy-g3.log
```

📸 **CAPTURA 14:** el log con fecha, rol y host.

**Texto bajo la captura:**
> Cada ejecución queda registrada en `/var/log/deploy-g3.log`, lo que sirve como evidencia y para depurar.

---

## Paso 10 · Subir esta documentación

```bash
cd ~/P0.0-ASIXc2B-G3
git add docs/automatizacion.md
git commit -m "Documentación de la automatización del despliegue"
git push
```

📸 **CAPTURA 15:** `git push` correcto.

---

## Variables y problemas habituales

| Variable | Por defecto | Para qué |
|----------|-------------|----------|
| `LAN_IF` | `enp2s0` | Tarjeta de la red compartida |
| `WAN_IF` | `enp1s0` | Tarjeta de salida a Internet (router) |
| `DNS` | `8.8.8.8,1.1.1.1` | Cambiar a `192.168.3.31,8.8.8.8` cuando el DNS del grupo funcione |

Ejemplo: `LAN_IF=enp3s0 sudo -E bash deploy.sh w`

| Problema | Causa / solución |
|----------|------------------|
| `No existe enp2s0` | El nombre de la tarjeta es otro: mira `ip -br a` y usa `LAN_IF=...` |
| Aviso `Despliega primero el router` | R-G3 no está desplegado o apagado |
| Aviso `tiene 2 IPs` | `50-cloud-init.yaml` aporta otra IP (netplan suma las listas). Edita ese fichero y quita la IP sobrante |
| Fallo al descargar el CSV | Sin Internet o DNS: comprueba el NAT del router y `ping google.com` |
| `bad interpreter` / `\r` | El script se editó en Windows: `sed -i 's/\r$//' deploy.sh` |
