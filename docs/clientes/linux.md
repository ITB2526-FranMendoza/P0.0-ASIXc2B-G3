P0.0-ASIXc2B-G3 · PC clientes

Responsable: Fran Mendoza

Equipo	Sistema	Hostname	IP	Máscara
Cliente Linux	Ubuntu 24.04	PC-LIN-G3	192.168.3.21	/24
Cliente Windows	Windows 10	PC-WIN-G3	192.168.3.22	255.255.255.0

Las dos VM se crean en IsardVDI con la tarjeta de red ITB3 incluida en la configuración, para poder probar la conexión con los demás servidores.

Máquinas en Isard

Índice
Parte 1: Cliente Linux (PC-LIN-G3)
Parte 2: Cliente Windows (PC-WIN-G3)
Parte 1: Cliente Linux (PC-LIN-G3)
1.1 Hostname

Se edita el archivo /etc/hostname:

bash
sudo nano /etc/hostname      # contenido: PC-LIN-G3

Hostname Linux

1.2 Configuración de IP

La interfaz enp2s0 tiene la IP 192.168.3.21/24. Se comprueba con:

bash
ip a

IP Linux

1.3 Usuario bchecker

Usuario bchecker con contraseña bchecker121:

bash
sudo useradd bchecker
sudo passwd bchecker

bchecker Linux

1.4 Pruebas de conectividad
Destino	IP	Responsable	Ping	Servicio
R-G3 (router/gateway)	192.168.3.30	Rubén	OK	n/a
SSH-G3	192.168.3.40	Joel	OK	SSH OK
F-G3 (FTP)	192.168.3.41	Joel	OK	FTP OK (bchecker)
W-G3 (web)	192.168.3.12	Jesús	OK	Pendiente (HTTP)
B-G3 (BBDD)	192.168.3.11	Jesús	OK	Pendiente (MySQL)
Router / DNS / DHCP
bash
ping 192.168.3.30

Ping router

SSH
bash
ping 192.168.3.40
ssh 192.168.3.40

La primera conexión pide aceptar la huella del servidor (yes).

SSH Linux

FTP
bash
ping 192.168.3.41
ftp 192.168.3.41      # usuario: bchecker

El servidor responde 220 (vsFTPd 3.0.5) y 230 Login successful.

FTP Linux

Web
bash
ping 192.168.3.12

Ping web

Base de datos
bash
ping 192.168.3.11

Ping BBDD

1.5 Validación por clave pública/privada con GitHub

Se autentica el equipo contra GitHub con SSH y un par de claves ed25519, sin contraseña. La clave privada nunca sale del equipo; solo se sube la pública.

Generar la clave
bash
ssh-keygen -t ed25519 -C "PC-LIN-G3-bchecker"

Se crean:

~/.ssh/id_ed25519 → clave privada (no compartir)
~/.ssh/id_ed25519.pub → clave pública

ssh-keygen

Copiar la clave pública
bash
cat ~/.ssh/id_ed25519.pub

Clave pública

Añadirla en GitHub

Perfil → Settings → SSH and GPG keys → New SSH key. Alternativa: en el repo, Settings → Deploy keys → Add deploy key, marcando Allow write access si va a hacer push.

Clave en GitHub

Probar la validación
bash
ssh -T git@github.com

La primera vez pide confirmar la huella de github.com (yes). La respuesta correcta es:

Hi <usuario>! You've successfully authenticated, but GitHub does not provide shell access.

ssh -T

1.6 Flujo Git
Clonar por SSH
bash
git clone git@github.com:ITB2526-FranMendoza/P0.0-ASIXc2B-G3.git
cd P0.0-ASIXc2B-G3

git clone

Identificarse (solo la primera vez)

Si git commit da error de identidad:

bash
git config --global user.name "ITB2526-FranMendoza"
git config --global user.email "<tu_correo>"

git config

Crear, confirmar y subir cambios
bash
nano test.md
git add .
git commit -m "prueba de claves ssh"
git push
Comando	Qué hace
git clone	Descarga una copia del repositorio
git add .	Prepara los cambios para el commit
git commit -m "mensaje"	Guarda los cambios con un mensaje
git push	Sube los commits a GitHub
git pull	Baja los cambios de GitHub

git commit y push

El archivo test.md aparece en GitHub con el mensaje del commit.

test.md en GitHub

Comprobación nube → local
bash
git pull

Responde Ja està al dia. (ya está actualizado).

git pull

Subida a producción

Pendiente: se documentará cuando W-G3 tenga su propia clave SSH. Flujo previsto: git push desde el cliente y git pull en W-G3
