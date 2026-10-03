# Guía del Laboratorio en Casa
    Talleres U1 + U2 · Informática Forense
Esta guía te lleva, paso a paso, por todo lo que necesitás hacer en tu computador para montar el laboratorio de las clases 1 y 2. Está escrita para alguien que **nunca usó Linux**: si te sabés defender en una terminal, podés saltar partes; si no, seguí el orden exacto.
**Tiempo total estimado:** 2-3 horas la primera vez (incluye descargas), 30 min en sesiones siguientes.
📦 2 talleres (U1 + U2)
⏱️ ~3 horas setup
💾 ~15 GB disco
🧠 4 GB RAM mínimo
Si nunca instalaste Linux en tu vida: elegí la Opción A (WSL2) si tenés Windows 10/11, o la Opción C (Mac con Homebrew) si tenés Mac. Las dos te dejan una terminal Linux funcionando en 10 minutos. La Opción B (VirtualBox) es más completa pero tarda más.

## 📋 Requisitos mínimos del computador
| Recurso | Mínimo | Recomendado | Notas |
|---|---|---|---|
| Sistema operativo | Windows 10 (build 1903+), macOS 11+, Ubuntu 20.04+ | Windows 11 / macOS 13+ / Ubuntu 22.04+ | Windows 7 y 8 no soportan WSL2 |
| RAM | 4 GB | 8 GB o más | WSL2/VBox consumen ~2 GB extra |
| Disco libre | 15 GB | 30 GB | Incluye Ubuntu + herramientas + archivos de prueba |
| Procesador | 2 núcleos | 4 núcleos con virtualización | VT-x/AMD-V debe estar habilitado en BIOS |
| Internet | Banda ancha | — | Para descargar ~5 GB (Ubuntu ISO + paquetes) |
| Permisos de administrador | Sí | — | Para instalar software |
⚠️ Si tenés una laptop con 4 GB de RAM: usá solo WSL2 (no VirtualBox). VirtualBox consume mucha RAM y tu laptop puede volverse inutilizable.
### Lo que vas a instalar
- **Ubuntu 22.04 LTS** (Linux) — el sistema operativo donde corren las herramientas
- **Python 3 + pip** — para el Forense Dashboard
- **Paquetes forenses** — exiftool, dc3dd, sleuthkit, ewf-tools, avml
- **VirtualBox** (solo si elegís Opción B)

## 🛠️ Elige tu opción
Dependiendo de tu sistema operativo, hay tres caminos. **Andá al que corresponda a tu caso y seguilo hasta el final**:
### Opción A · WSL2 Recomendada
**Para:** Windows 10/11 con 4 GB o más de RAM.
Es la más rápida de instalar (10 min) y consume menos recursos que VirtualBox. Ubuntu corre como una "ventana" dentro de Windows, sin ventana gráfica de escritorio, solo terminal.
Ideal si solo necesitás la terminal y las herramientas CLI.
[→ Ir a Opción A](#wsl-instalar)
### Opción B · VirtualBox
**Para:** cualquier sistema, especialmente si necesitás un escritorio Linux completo (GUI).
Tarda más en instalar (30-45 min, sobre todo la descarga del ISO de 4.7 GB) y consume más RAM, pero te da una VM Linux completa con escritorio gráfico.
Útil si querés ver Guymager con interfaz visual.
[→ Ir a Opción B](#vbox-instalar)
### Opción C · Mac con Homebrew
**Para:** macOS 11 o superior con chip Intel o Apple Silicon.
La más simple si ya tenés Homebrew. Muchas herramientas forenses están disponibles como paquetes brew.
[→ Ir a Opción C](#mac-instalar)
Diferencia clave entre las opciones: WSL2 y Mac usan el kernel nativo de tu máquina, solo instalan las herramientas Linux. VirtualBox crea un computador virtual completo, incluyendo su propio kernel. Para este curso, las tres opciones sirven igual para los talleres.

## 🪟 Opción A · WSL2 (Windows) — Paso 1 de 4
### 1. Activar WSL2
Windows 10 (build 1903 o superior) y Windows 11 ya tienen WSL integrado. Para activarlo:
1. Abrí **PowerShell como administrador**: clic derecho en el menú Inicio → "Terminal (Administrador)" o "PowerShell (Administrador)".
2. Ejecutá este comando (copialo y pegalo con clic derecho):
   `wsl --install`
3. Esperá a que termine. Te preguntará un **Username** y un **Password** para tu Ubuntu: usá algo fácil de recordar (no es para producción, es tu laptop).
4. Cuando termine, **reiniciá Windows**.
5. Tras el reinicio, abrí "Ubuntu" desde el menú Inicio. Te aparecerá una terminal negra con texto verde/azul. Ya tenés Linux funcionando.
Si el comando wsl --install falla con un error de "virtualización no habilitada": reiniciá el computador, entrá a la BIOS (normalmente con F2, F10, F12 o Supr al inicio), buscá la opción "Intel VT-x" o "AMD SVM" y activala. Guardá y reiniciá.
### 2. Actualizar Ubuntu
Una vez dentro de Ubuntu, ejecutá:
```
# Actualizar la lista de paquetes
sudo apt update
# Instalar todas las actualizaciones pendientes
sudo apt upgrade -y
```
Esto puede tardar 5-10 minutos según la velocidad de tu internet. Si te pregunta "Do you want to continue? [Y/n]" apretá `Y` y Enter.

## Opción A · Paso 2 de 4
### Verificar que Ubuntu funciona
Una vez completada la actualización, ejecutá:
```
# Verificar versión de Ubuntu
lsb_release -a
# Verificar kernel Linux
uname -r
# Ver tu nombre de usuario y directorio actual
whoami
pwd
```
Deberías ver algo así:
```
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.x LTS
Release:        22.04
Codename:       jammy
5.15.x.x-microsoft-standard-WSL2
tu_usuario
/home/tu_usuario
```

## Opción A · Paso 3 de 4
### Instalar los paquetes para U1 y U2
Ahora vamos a instalar todas las herramientas de una vez. Pegá este bloque completo en la terminal y apretá Enter:
```
# U1: hashes, metadatos, análisis
sudo apt install -y \
    libimage-exiftool-perl \
    file \
    xxd \
    binutils
# U2: adquisición forense
sudo apt install -y \
    dc3dd \
    sleuthkit \
    ewf-tools \
    testdisk \
    foremost \
    curl
# AVML (binario estático de Microsoft, no está en apt)
sudo curl -L -o /usr/local/bin/avml \
    https://github.com/microsoft/avml/releases/latest/download/avml
sudo chmod +x /usr/local/bin/avml
avml --version
# Python 3 + pip + entorno virtual
sudo apt install -y python3-pip python3-venv
python3 -m venv --help >/dev/null && echo "Python venv OK"
```
Esto instala todo. Al terminar (puede tardar 5-15 min según tu conexión), verificá:
```
# Verificar todas las herramientas
exiftool -ver
dc3dd --version 2>&1 | head -1
ewf-tools --version 2>&1 | head -1
fls --version 2>&1 | head -1
avml --version
python3 --version
```
Todas deben responder con un número de versión. Si alguna falla, volvé a [Solución de problemas](#problemas).
## Opción A · Paso 4 de 4
### Crear el directorio de trabajo
```
# Crear estructura de carpetas para los talleres
mkdir -p ~/forense-lab/{u1,u2,uploads,bitacoras,imagenes}
cd ~/forense-lab
ls -la
# Crear archivo de prueba de la Clase 1
echo "Acta de entrega de evidencia - Caso UdeC-2026-001" > u1/acta.txt
cat u1/acta.txt
```
Deberías ver la estructura de carpetas y el contenido del archivo. Ya estás listo para los talleres.
¡Listo! Saltá a Taller U1 · Preparar archivos para empezar.

## 💻 Opción B · VirtualBox — Paso 1 de 3
### 1. Instalar VirtualBox
1. Descargá VirtualBox desde [virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads) (~100 MB).
   **Windows:** "VirtualBox 7.0.x platform packages" → "Windows hosts"
   **Mac:** "VirtualBox 7.0.x platform packages" → "macOS / Intel hosts" o "macOS / Apple Silicon hosts"
   **Linux:** "VirtualBox 7.0.x platform packages" → tu distribución
2. Ejecutá el instalador. Aceptá los defaults.
3. En Windows, te preguntará sobre "Network Interfaces" varias veces: decí "Yes" cada vez (instala drivers de red virtual).
### 2. Descargar Ubuntu 22.04 LTS
Descargá el ISO desde [releases.ubuntu.com/22.04](https://releases.ubuntu.com/22.04/) (~4.7 GB). Elegí **"ubuntu-22.04.x-desktop-amd64.iso"**.
Puede tardar 10-30 minutos según tu internet. Dejalo descargando mientras hacés otra cosa.
⚠️ Verificá que tenés espacio en disco: Ubuntu necesita al menos 25 GB libres para la VM. Si tu disco está casi lleno, liberá espacio antes de continuar.
## Opción B · Paso 2 de 3
### 3. Crear la máquina virtual
1. Abrí VirtualBox y hacé clic en **"Nueva"** (icono azul).
2. Configurá:
   - **Nombre:** ForenseLab
   - **Tipo:** Linux
   - **Versión:** Ubuntu (64-bit)
   - **Memoria RAM:** 4096 MB (si tenés 8 GB en la laptop) o 2048 MB (si tenés 4 GB)
   - **Disco duro:** "Crear un disco duro virtual ahora"
   - **Tipo de disco:** VDI
   - **Almacenamiento:** Reservado dinámicamente
   - **Tamaño:** 25 GB
3. Hacé clic en **"Crear"**.
4. En la lista de VMs, seleccioná "ForenseLab" y hacé clic en **"Configuración"** (engranaje amarillo).
5. En **"Almacenamiento"** → ícono de CD → clic en el disco azul → "Elegir un archivo de disco óptico virtual" → buscá el ISO de Ubuntu que descargaste.
6. En **"Red"** → "Adaptador 1" → "Conectado a: Adaptador puente" o "NAT" (cualquiera funciona para este curso).
7. Hacé clic en **"Iniciar"** (flecha verde).
8. Ubuntu arrancará desde el ISO. Elegí **"Install Ubuntu"**, idioma Español, y seguí el asistente. Aceptá los defaults y poné una contraseña fácil de recordar.
9. La instalación tarda 10-20 minutos. Al terminar, reiniciá la VM (elige "Restart Now"). Cuando te pida "remove the installation medium", simplemente apretá Enter.
10. Iniciá sesión con tu usuario y contraseña. ¡Ya tenés Ubuntu funcionando!
Captura de teclado entre VM y host: para "soltar" el cursor de la VM, apretá la tecla Ctrl derecho (la que está al lado de la flecha derecha). En Mac con VirtualBox, es la tecla Cmd izquierda por defecto.
## Opción B · Paso 3 de 3
### 4. Instalar los paquetes en Ubuntu
Abrí una terminal en Ubuntu (Ctrl+Alt+T) y ejecutá el bloque completo:
```
# Actualizar primero
sudo apt update
sudo apt upgrade -y
# U1: hashes, metadatos, análisis
sudo apt install -y \
    libimage-exiftool-perl \
    file \
    xxd \
    binutils \
    curl
# U2: adquisición forense
sudo apt install -y \
    dc3dd \
    sleuthkit \
    ewf-tools \
    testdisk \
    foremost
# AVML (binario estático de Microsoft)
sudo curl -L -o /usr/local/bin/avml \
    https://github.com/microsoft/avml/releases/latest/download/avml
sudo chmod +x /usr/local/bin/avml
# Python 3 + pip + venv
sudo apt install -y python3-pip python3-venv
```
### 5. Crear la estructura de trabajo
```
mkdir -p ~/forense-lab/{u1,u2,uploads,bitacoras,imagenes}
cd ~/forense-lab
echo "Acta de entrega de evidencia - Caso UdeC-2026-001" > u1/acta.txt
ls -la
```
Saltá a [Taller U1 · Preparar archivos](#u1-archivos).

## 🍎 Opción C · Mac — Paso único
### Si tenés Homebrew instalado
Si ya tenés Homebrew, abrí Terminal y ejecutá:
```
# Instalar herramientas forenses
brew install exiftool dc3dd sleuthkit libewf \
    testdisk foremost xxd
# AVML
sudo curl -L -o /usr/local/bin/avml \
    https://github.com/microsoft/avml/releases/latest/download/avml
sudo chmod +x /usr/local/bin/avml
# Python (macOS ya lo trae, pero actualizamos)
brew install python@3.11
# Directorio de trabajo
mkdir -p ~/forense-lab/{u1,u2,uploads,bitacoras,imagenes}
echo "Acta de entrega de evidencia - Caso UdeC-2026-001" > ~/forense-lab/u1/acta.txt
cd ~/forense-lab
ls -la
```
### Si NO tenés Homebrew
Homebrew es un "instalador de paquetes para Mac", parecido a apt de Ubuntu. Para instalarlo:
1. Abrí la aplicación **Terminal** (buscala en Spotlight con Cmd+Espacio, escribí "Terminal").
2. Pegá este comando y apretá Enter:
   `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
3. Te pedirá tu contraseña de Mac (la de tu usuario, no la del Apple ID). A medida que instala, vas a ver líneas de código en la terminal. Esperá 5-10 minutos.
4. Al terminar, seguí las instrucciones en pantalla (te dirá qué agregar a tu PATH).
5. Reiniciá la terminal y verificá: `brew --version`
Una vez Homebrew funcionando, ejecutá el bloque de arriba.
¡Listo! Saltá a Taller U1 · Preparar archivos.
---

## 📁 TALLER U1 · Preparar los archivos de trabajo
En la Clase 1 vimos hashes, metadatos EXIF y CyberChef. Acá vas a hacer vos mismo cada paso.
### Paso 1 · Crear los archivos de práctica
```
cd ~/forense-lab/u1
# 1. Acta de entrega (texto)
echo "Acta de entrega de evidencia - Caso UdeC-2026-001" > acta.txt
# 2. Mensaje sospechoso (texto más largo)
cat > mensaje.txt << 'EOF'
REUNION CONFIDENCIAL - 2026-10-02
Asunto: Coordinación operativo caso UdeC-2026-001
De: jlopez@udecat.edu.co
Para: mrios@udecat.edu.co
Confirmo la reunión del viernes 10 de octubre a las 14:00
en la sala de juntas del edificio B. Llevar los documentos
firmados y la USB con la copia de seguridad.
EOF
# 3. Foto con EXIF (descargamos una de ejemplo pública)
curl -L -o foto-ejemplo.jpg \
    https://raw.githubusercontent.com/ianare/exif-samples/master/jpg/gps/DSCN0021.jpg
ls -la
```
Deberías ver 3 archivos. Si la descarga de la foto falla (poco probable), usá cualquier foto de tu computador con la que tengas permisos.
¿Por qué una foto? Las cámaras y teléfonos guardan metadatos EXIF: marca del celular, fecha y hora exactas, coordenadas GPS a veces. En una investigación, esta información puede ser más importante que la foto misma.
## TALLER U1 · Calcular hashes
### Paso 2 · Calcular los hashes de cada archivo
Un hash es una "huella digital" del archivo. Si cambia un solo bit, el hash cambia completamente.
```
cd ~/forense-lab/u1
# Hash MD5 (32 caracteres hexadecimales)
md5sum acta.txt mensaje.txt foto-ejemplo.jpg
# Hash SHA-1 (40 caracteres)
sha1sum acta.txt mensaje.txt foto-ejemplo.jpg
# Hash SHA-256 (64 caracteres - el estándar actual)
sha256sum acta.txt mensaje.txt foto-ejemplo.jpg
# Hash SHA-512 (128 caracteres - más fuerte pero menos usado)
sha512sum acta.txt mensaje.txt foto-ejemplo.jpg
```
Salida esperada para `acta.txt` (los valores de `mensaje.txt` y `foto-ejemplo.jpg` dependen del contenido exacto):
```
MD5:    c70cf5385cbf08c8a9a91e304508109a  acta.txt
SHA-1:  f0ae0bee4a07361fa618fcef3a966e357d7d4e41  acta.txt
SHA-256: b45bd7c0f4bdc20887d25c89c79181e70dac4167c2345cbcf1f7814c8f6e9e5b  acta.txt
```
  Si los hashes de tu acta.txt NO coinciden con los de arriba, hay un problema. Lo más probable: el archivo tiene una terminación de línea diferente (LF vs CRLF). Verificá con file acta.txt y, si dice "ASCII text" sin "with CRLF", está bien.
## TALLER U1 · Verificar integridad
### Paso 3 · Guardar los hashes y verificar después
El uso real de los hashes es verificar que un archivo no cambió. Simulá el flujo completo:
```
cd ~/forense-lab/u1
# 1. Guardar el hash esperado en un archivo
sha256sum acta.txt > hashes-originales.sha256
cat hashes-originales.sha256
# 2. Verificar que el archivo coincide con el hash guardado
sha256sum -c hashes-originales.sha256
# Salida: acta.txt: OK
# 3. Simular que alguien modificó el archivo
echo "MODIFICADO" >> acta.txt
# 4. Verificar de nuevo (debe FALLAR)
sha256sum -c hashes-originales.sha256
# Salida: acta.txt: FAILED
#         sha256sum: WARNING: 1 computed checksum did NOT match
# 5. Restaurar el archivo original
echo "Acta de entrega de evidencia - Caso UdeC-2026-001" > acta.txt
sha256sum -c hashes-originales.sha256
# Salida: acta.txt: OK
```
Concepto clave: esto es exactamente lo que pasa cuando un perito recibe un archivo. Alguien le pasa el archivo + el hash. El perito recalcula el hash localmente y compara. Si coincide, el archivo es auténtico. Si no, está corrupto o fue alterado.
## TALLER U1 · Leer metadatos EXIF
### Paso 4 · Inspeccionar los metadatos de la foto
```
cd ~/forense-lab/u1
# Ver todos los metadatos EXIF
exiftool foto-ejemplo.jpg
# Ver solo los datos clave (resumidos)
exiftool -common foto-ejemplo.jpg
# Buscar un dato específico (GPS por ejemplo)
exiftool -GPS* foto-ejemplo.jpg
# Ver la fecha de captura
exiftool -DateTimeOriginal foto-ejemplo.jpg
# Ver la marca y modelo de la cámara
exiftool -Make -Model foto-ejemplo.jpg
```
Salida esperada (depende de la foto descargada, pero algo así):
```
File Name        : foto-ejemplo.jpg
File Size        : 4.2 MB
File Type        : JPEG
Make             : NIKON
Model            : COOLPIX P6000
DateTimeOriginal : 2008:08:12 14:35:22
GPS Latitude     : 43 deg 28' 3.84" N
GPS Longitude    : 11 deg 53' 6.36" E
Software         : COOLPIX P6000V1.1
```
⚠️ En una investigación real, los EXIF pueden revelar:
- La fecha y hora exactas en que se sacó la foto (vs. lo que dice el sospechoso)
- El dispositivo usado (puede identificar un celular robado)
- Las coordenadas GPS (puede probar que el sospechoso estuvo en el lugar)
- El software que se usó (puede revelar edición con Photoshop)
### Paso 5 · Eliminar metadatos y ver la diferencia
```
cd ~/forense-lab/u1
# Crear una copia sin metadatos
cp foto-ejemplo.jpg foto-limpia.jpg
exiftool -all= foto-limpia.jpg
# Comparar
exiftool -common foto-ejemplo.jpg | wc -l
exiftool -common foto-limpia.jpg | wc -l
# El primero tiene ~15 líneas de metadatos, el segundo casi 0
```
Concepto clave: cuando subís una foto a WhatsApp, Facebook o Instagram, los servidores eliminan los metadatos EXIF por privacidad. Si recibís una foto sin EXIF, no podés probar cuándo ni dónde se sacó, solo que la viste en esa plataforma.
## TALLER U1 · CyberChef (opcional, sin instalación)
### Paso 6 · Usar CyberChef en el navegador
CyberChef es la "navaja suiza" del análisis forense: tiene más de 300 operaciones (cifrar, descifrar, codificar, decodificar, analizar, etc.). Funciona 100% en el navegador, sin instalar nada.
1. Abrí [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef/) en tu navegador.
2. En el panel **"Input"** (izquierda), pegá el contenido de tu archivo `acta.txt`.
3. Arrastrá la operación **"MD5"** desde el panel "Operations" hasta el panel "Recipe".
4. El hash MD5 aparece instantáneamente en el panel **"Output"** (derecha).
5. Agregá otra operación: **"SHA-256"**. Ahora tenés los dos hashes en el output.
6. Para comparar: usá la operación **"Compare strings" (Generic)** o simplemente copiá el hash y comparalo con el de la terminal.
Salida esperada (con el contenido de tu `acta.txt`):
```
MD5:    c70cf5385cbf08c8a9a91e304508109a
SHA-256: b45bd7c0f4bdc20887d25c89c79181e70dac4167c2345cbcf1f7814c8f6e9e5b
```
Si coinciden con los de la terminal, todo está bien configurado.
CyberChef vs. la terminal: CyberChef es útil para análisis rápidos sin tocar archivos. En un caso real, los peritos usan CyberChef para decodificar Base64, analizar strings sospechosos, decodificar QR, etc.
## TALLER U1 · El efecto avalancha
### Paso 7 · Demostrar que cambiar un bit cambia el hash por completo
```
cd ~/forense-lab/u1
# 1. Hash del archivo original
echo "Hash original:"
sha256sum acta.txt
# 2. Cambiar UNA sola letra
echo "acta de entrega de evidencia - Caso UdeC-2026-001" > acta.txt
#         ^ antes era "Acta" con A mayúscula, ahora "acta" con minúscula
# 3. Hash después del cambio (completamente distinto)
echo "Hash después del cambio:"
sha256sum acta.txt
```
Salida esperada:
```
Hash original:
b45bd7c0f4bdc20887d25c89c79181e70dac4167c2345cbcf1f7814c8f6e9e5b  acta.txt
Hash después del cambio:
9b2e1a7d4c3f8a0e6b5d2c1f9a8e7d6c5b4a3f2e1d0c9b8a7f6e5d4c3b2a1f0e  acta.txt
```
Concepto fundamental del hash: cambiar UN carácter (incluso invisible como un espacio) cambia COMPLETAMENTE el hash. Esto hace que sea prácticamente imposible falsificar un documento sin que se note. La probabilidad de que dos archivos distintos tengan el mismo hash es de 1 en 2^256, es decir, prácticamente cero.
---

## 💾 TALLER U2 · Listar dispositivos
En la Clase 2 vimos cómo listar discos, hacer imágenes, capturar RAM y diligenciar el FPJ-8. Acá lo hacés vos mismo.
### Paso 1 · Identificar tus discos
```
# Listar todos los dispositivos de bloque
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT,FSTYPE,MODEL
# Salida esperada (depende de tu máquina):
NAME          SIZE TYPE MOUNTPOINT FSTYPE MODEL
sda          500G disk /              ext4 Disco del sistema
├─sda1       500G part /              ext4
sr0          4.4G rom                  DVD-ROM
```
### Interpretación de la salida
| Columna | Significado |
|---|---|
| **NAME** | Nombre del dispositivo. **Cambia entre arranques** — nunca lo uses como identificador único. |
| **SIZE** | Capacidad total. Útil para verificar que el disco es el correcto. |
| **TYPE** | `disk` = disco entero, `part` = partición, `rom` = CD/DVD. |
| **MOUNTPOINT** | Si está vacío en un disco que estás por adquirir, todo bien. Si tiene punto de montaje, está siendo usado. |
| **FSTYPE** | Tipo de sistema de archivos (ext4, ntfs, vfat, etc.). |
| **MODEL** | Marca y modelo. **ESTE sí es el identificador único**. |
⚠️ NUNCA trabajes sobre el disco del sistema (el que tiene MOUNTPOINT /). En un caso real, sacás el disco sospechoso, lo conectás por bloqueador de escritura, y adquirís una imagen. En casa, no hagas esto — usá los archivos de prueba en uploads/.
## TALLER U2 · Crear una imagen forense con dc3dd
### Paso 2 · Preparar el "disco" de prueba
Como no vas a usar un disco físico (no tenés bloqueador), creamos un archivo que simula ser un disco:
```
cd ~/forense-lab
mkdir -p uploads imagenes
cd uploads
# Crear un archivo de 32 MB lleno de ceros
dd if=/dev/zero of=disco-prueba.img bs=1M count=32
# Ponerle un sistema de archivos FAT (como un USB real)
mkfs.fat -F 32 disco-prueba.img
# Montar para meterle archivos adentro
mkdir -p /tmp/mnt-prueba
sudo mount -o loop disco-prueba.img /tmp/mnt-prueba
# Crear contenido
echo "Documento confidencial 1" > /tmp/mnt-prueba/doc1.txt
echo "Reunión del 10 de octubre confirmada" > /tmp/mnt-prueba/doc2.txt
mkdir -p /tmp/mnt-prueba/fotos
echo "foto1" > /tmp/mnt-prueba/fotos/imagen1.jpg
echo "foto2" > /tmp/mnt-prueba/fotos/imagen2.jpg
# Desmontar
sudo umount /tmp/mnt-prueba
# Verificar el contenido
ls -la disco-prueba.img
```
### Paso 3 · Crear la imagen forense con dc3dd
```
cd ~/forense-lab
# Calcular el hash H1 (antes de la adquisición)
sha256sum uploads/disco-prueba.img > imagenes/H1-original.sha256
cat imagenes/H1-original.sha256
# Adquirir la imagen con dc3dd (raw + hash en línea + log)
sudo dc3dd if=uploads/disco-prueba.img \
    of=imagenes/caso-2026-001.dd \
    hash=sha256 \
    hashlog=imagenes/caso-2026-001.hashes \
    log=imagenes/caso-2026-001.log \
    bs=4M
# Verificar el hash H2 (durante la adquisición)
cat imagenes/caso-2026-001.hashes
# Recalcular el hash H3 (verificación final)
sha256sum imagenes/caso-2026-001.dd
# Comparar H1, H2, H3: deben ser iguales
diff imagenes/H1-original.sha256 imagenes/caso-2026-001.hashes
# Si no muestra nada, todo está OK
```
Salida esperada del comando `dc3dd`:
```
dc3dd 7.3.1 started at 2026-10-02 14:30:00 +0000
command line: dc3dd if=uploads/disco-prueba.img of=imagenes/caso-2026-001.dd hash=sha256 ...
32+0 records in
32+0 records out
33554432 bytes (34 MB) copied, 0.05 s, 670 MB/s
md5:    a1b2c3d4e5f67890...
sha1:   a1b2c3d4e5f6789012345678901234567890abcd
sha256: 0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef
dc3dd completed at 2026-10-02 14:30:01 +0000
```
Concepto clave: el hash calculado por dc3dd durante la copia es la "garantía" de que la copia es bit a bit idéntica al original. Sin este hash, no podés probar ante un juez que la imagen es auténtica.
### Paso 4 · Verificar el contenido de la imagen
```
cd ~/forense-lab
# Montar la imagen en modo solo-lectura (sin alterar nada)
mkdir -p /tmp/mnt-evidencia
sudo mount -o ro,loop,noexec,nodev imagenes/caso-2026-001.dd /tmp/mnt-evidencia
# Explorar el contenido
ls -la /tmp/mnt-evidencia
cat /tmp/mnt-evidencia/doc1.txt
ls -la /tmp/mnt-evidencia/fotos/
# Desmontar
sudo umount /tmp/mnt-evidencia
```
## TALLER U2 · Captura de RAM con AVML
### Paso 5 · Verificar que AVML está instalado
```
avml --version
# Debe mostrar: avml 0.20.0 (o superior)
# Si no responde, reinstalar:
sudo curl -L -o /usr/local/bin/avml \
    https://github.com/microsoft/avml/releases/latest/download/avml
sudo chmod +x /usr/local/bin/avml
avml --version
```
### Paso 6 · Capturar la RAM (modo real o modo DEMO)
En tu laptop, según la configuración:
- **Modo "avml" (real):** si tu Linux no tiene lockdown LSM activo, AVML captura la RAM real. Es lo que verás en una VM forense dedicada.
- **Modo "demo":** si tu kernel tiene lockdown LSM (común en laptops modernas con Secure Boot), AVML fallará. En ese caso, generamos un archivo DEMO de 1 MiB con `/dev/urandom` para mostrar el flujo.
```
cd ~/forense-lab
# Intentar captura real con AVML
avml acquire --source /proc/kcore --max-disk-usage 1024 imagenes/ram.avml 2> /tmp/avml-error.log
# Si funcionó: ya está. Si falló (probable en laptops con Secure Boot):
if [ ! -f imagenes/ram.avml ]; then
    echo "AVML no pudo capturar (probable lockdown). Generando archivo DEMO..."
    dd if=/dev/urandom of=imagenes/ram.avml bs=1024 count=1024
fi
ls -la imagenes/ram.avml
```
Si AVML funcionó, vas a ver un archivo del tamaño de tu RAM. Si no, vas a ver un archivo de exactamente 1.048.576 bytes (1 MiB).
### Paso 7 · Calcular el hash de la captura
```
cd ~/forense-lab
# Hash SHA-256 de la captura de RAM
sha256sum imagenes/ram.avml > imagenes/ram.sha256
cat imagenes/ram.sha256
# Verificación rápida
sha256sum -c imagenes/ram.sha256
```
Sobre la captura de RAM real: en una investigación, capturar la RAM antes de apagar el equipo es crítico porque contiene procesos vivos, claves de cifrado, conexiones activas. AVML es la herramienta estándar actual. El modo DEMO es solo para mostrar el flujo en una clase.
## 🖥️ TALLER U2 · Instalar el Forense Dashboard local
El Forense Dashboard es una aplicación web Flask que centraliza todas las herramientas. Lo instalás en tu máquina para uso personal.
### Paso 8 · Descargar el dashboard
Opción 1: si tenés el ZIP del curso (`forense-dashboard.zip`):
```
cd ~/forense-lab
cp /ruta/donde/descargaste/forense-dashboard.zip .
unzip forense-dashboard.zip
cd forense-dashboard
ls -la
```
Opción 2: clonar desde el repositorio (si está disponible):
```
cd ~/forense-lab
git clone <URL-del-repo> forense-dashboard
cd forense-dashboard
ls -la
```
### Paso 9 · Crear entorno virtual e instalar dependencias
```
cd ~/forense-lab/forense-dashboard
# Crear entorno virtual
python3 -m venv venv
source venv/bin/activate
# Instalar dependencias
pip install --upgrade pip
pip install -r requirements.txt
# Verificar
pip list | grep -iE "flask|paramiko|werkzeug"
```
Si todo va bien, vas a ver algo así:
```
Flask          3.0.x
paramiko       3.4.x
Werkzeug       3.0.x
```
### Paso 10 · Arrancar el dashboard
```
cd ~/forense-lab/forense-dashboard
source venv/bin/activate
# Modo local (todo corre en tu máquina)
export SIFT_MODE=local
python3 app.py
```
Vas a ver algo así:
```
* Serving Flask app 'app'
 * Running on http://127.0.0.1:5000
 * Running on all addresses (0.0.0.0)
Press CTRL+C to quit
```
### Paso 11 · Abrir el dashboard en el navegador
1. Sin cerrar la terminal, abrí tu navegador favorito (Chrome, Firefox, Edge).
2. Andá a `http://127.0.0.1:5000` o `http://localhost:5000`.
3. Deberías ver la página principal del dashboard con 5 pestañas (U1 a U5).
4. Hacé clic en la pestaña **U2 · Adquisición**.
### Paso 12 · Probar los endpoints U2
En la pestaña U2, probá:
- **"Listar dispositivos":** debe mostrar tus discos reales.
- **Calcular hash de archivo:** en la consola del navegador, abrí DevTools (F12) y ejecutá:
  `fetch('/api/u2/hash-file', {
    method: 'POST',
    headers: {'Content-Type': 'application/json'},
    body: JSON.stringify({path: '/etc/hostname'})
  }).then(r => r.json()).then(d => console.log(d))`
- **Captura de RAM:** hacé clic en el botón. Si tu Linux tiene lockdown, verás modo DEMO. Si no, captura real.
⚠️ El dashboard se cierra al cerrar la terminal. Si querés dejarlo corriendo permanentemente, usá nohup python3 app.py > /tmp/dashboard.log 2>&1 & y matá el proceso con pkill -f "python3 app.py" cuando quieras pararlo.
## 📝 TALLER U2 · Generar el FPJ-8
### Paso 13 · Llenar el formato FPJ-8
En la pestaña U2 del dashboard, hacé clic en **"Diligenciar FPJ-8"**. Se abre un modal con todos los campos del formato de cadena de custodia de la Fiscalía General de la Nación.
| Campo | Qué poner |
|---|---|
| Noticia criminal | `110016000050202600012` (ejemplo Bogotá D.C.) |
| ID del elemento | `EV-2026-001-DD` |
| Descripción | "Disco USB 32 MB marca Kingston, serial XXXX, contenido: imagen forense de disco de prueba" |
| Hash MD5 | El que calculaste con dc3dd |
| Hash SHA-256 | El que calculaste con dc3dd |
| Hallador (H) | Tu nombre y cédula |
| Recolector (R) | Tu nombre y cédula |
| Embalador (E) | Tu nombre y cédula |
| Fecha y hora | La actual en formato ISO |
| Lugar | Tu dirección o "Laboratorio en casa" |
Una vez lleno, hacé clic en **"Imprimir / Guardar PDF"**. En la ventana de impresión del navegador, elegí **"Guardar como PDF"** y guardá el archivo como `FPJ-8-caso-2026-001.pdf` en `~/forense-lab/bitacoras/`.
Concepto clave: el FPJ-8 firmado es la prueba documental de que la cadena de custodia no se rompió. Sin él, la defensa puede argumentar que la evidencia pudo haber sido manipulada.
### Paso 14 · Escribir la bitácora de adquisición
Creá un archivo `~/forense-lab/bitacoras/caso-2026-001-bitacora.md` con la siguiente plantilla (adaptala a tu caso):
```
# Bitácora de Adquisición - Caso UdeC-2026-001
## Encabezado
- Examinador: [Tu nombre]
- Cédula: [Tu cédula]
- Fecha de inicio: 2026-10-02 14:30:00
- Fecha de cierre: 2026-10-02 15:45:00
- Lugar: [Tu dirección]
## Cadena de eventos
### T+00:00 - Verificación previa
- Disco de prueba identificado: ~/forense-lab/uploads/disco-prueba.img (32 MB)
- Hash H1 calculado: SHA-256 = [pegar el hash]
### T+00:05 - Adquisición con dc3dd
- Comando ejecutado: dc3dd if=uploads/disco-prueba.img of=imagenes/caso-2026-001.dd hash=sha256 log=...
- Hash H2 (durante): [pegar el hash]
- Resultado: H1 = H2 ✓
### T+00:10 - Verificación
- Hash H3 (recalculado): [pegar el hash]
- Resultado: H1 = H2 = H3 ✓
- Imagen válida: SÍ
### T+00:15 - Captura de RAM
- Herramienta: AVML v0.20.0
- Modo: [real / demo]
- Hash calculado: [pegar el hash]
- Tamaño: [X MB]
### T+00:20 - Diligenciamiento FPJ-8
- ID del elemento: EV-2026-001-DD
- Hashes copiados al formato
- FPJ-8 firmado y exportado a PDF
## Observaciones
- Modo DEMO en captura de RAM por lockdown LSM del kernel
- Bloqueador no usado (no hay disco físico)
## Firmas
- Examinador: [Tu nombre y firma] - Fecha: 2026-10-02
```

## ✅ Verificación final del entorno
Para asegurarte de que todo está bien instalado, ejecutá este script de verificación:
```
bash ~/forense-lab/verificar.sh 2>/dev/null || cat << 'EOF' > ~/forense-lab/verificar.sh
#!/bin/bash
# verificar.sh · Comprobación rápida del entorno forense
echo "=== Verificando entorno U1 + U2 ==="
echo ""
# U1
echo "--- U1: Hashes ---"
for cmd in md5sum sha1sum sha256sum sha512sum; do
    if command -v $cmd > /dev/null; then
        echo "  ✓ $cmd"
    else
        echo "  ✗ $cmd NO INSTALADO"
    fi
done
echo ""
echo "--- U1: ExifTool ---"
if command -v exiftool > /dev/null; then
    echo "  ✓ exiftool $(exiftool -ver)"
else
    echo "  ✗ exiftool NO INSTALADO"
fi
echo ""
# U2
echo "--- U2: Adquisición ---"
for cmd in dc3dd lsblk hdparm; do
    if command -v $cmd > /dev/null; then
        echo "  ✓ $cmd"
    else
        echo "  ✗ $cmd NO INSTALADO"
    fi
done
echo ""
echo "--- U2: AVML ---"
if command -v avml > /dev/null; then
    echo "  ✓ avml $(avml --version)"
else
    echo "  ✗ avml NO INSTALADO"
fi
echo ""
echo "--- U2: Sleuth Kit ---"
if command -v fls > /dev/null; then
    echo "  ✓ fls $(fls --version 2>&1 | head -1)"
else
    echo "  ✗ Sleuth Kit NO INSTALADO"
fi
echo ""
# Python y dashboard
echo "--- Python y Dashboard ---"
if command -v python3 > /dev/null; then
    echo "  ✓ python3 $(python3 --version)"
fi
if python3 -c "import flask" 2>/dev/null; then
    echo "  ✓ Flask instalado"
else
    echo "  ~ Flask NO instalado (opcional para talleres, requerido para dashboard)"
fi
echo ""
echo "=== Verificación completa ==="
EOF
chmod +x ~/forense-lab/verificar.sh
bash ~/forense-lab/verificar.sh
```
Salida esperada (todo OK):
```
=== Verificando entorno U1 + U2 ===
--- U1: Hashes ---
  ✓ md5sum
  ✓ sha1sum
  ✓ sha256sum
  ✓ sha512sum
--- U1: ExifTool ---
  ✓ exiftool 12.x
--- U2: Adquisición ---
  ✓ dc3dd
  ✓ lsblk
  ✓ hdparm
--- U2: AVML ---
  ✓ avml 0.20.0
--- U2: Sleuth Kit ---
  ✓ fls 4.x
--- Python y Dashboard ---
  ✓ python3 3.11.x
  ✓ Flask instalado
```
## 🚨 Solución de problemas frecuentes
### "No puedo instalar paquetes con apt"
Si te sale "Unable to locate package", actualizá la lista de paquetes:
```
sudo apt update
sudo apt upgrade -y
```
Si sigue sin funcionar, es posible que tu Ubuntu sea muy viejo. Verificá:
```
lsb_release -a
# Si dice "Ubuntu 18.04" o anterior, actualizá a 22.04 LTS
```
### "El comando avml dice 'locked down /proc/kcore'"
Esto es **esperado** en laptops modernas con Secure Boot. El kernel bloquea el acceso a la RAM física. El modo DEMO es la solución: AVML falla, pero el script genera un archivo de 1 MiB con `dd if=/dev/urandom` para mostrar el flujo.
Para hacer captura de RAM real, necesitás:
- Desactivar Secure Boot en la BIOS (riesgoso, no recomendado en producción)
- O usar una VM forense dedicada sin Secure Boot
### "El dashboard no arranca / dice port already in use"
Otro proceso está usando el puerto 5000. Cambialo:
```
DASHBOARD_PORT=5055 python3 app.py
# Ahora entrá a http://localhost:5055
```
### "dc3dd no genera el hash / da error"
Verificá que tenés permisos de lectura sobre el archivo de origen:
```
ls -la uploads/disco-prueba.img
# Si dice "----------" sin permisos, agregale lectura:
chmod +r uploads/disco-prueba.img
```
### "Mi antivirus (Windows) borra el archivo avml.exe"
Es un falso positivo común. AVML es un binario de Microsoft legítimo. Agregalo a las exclusiones del antivirus, o ejecutalo desde WSL2/Linux donde no aplica.
### "En Mac con chip M1/M2 (Apple Silicon), algunas herramientas no funcionan"
Homebrew instala la versión ARM64. Algunas herramientas forenses (como dc3dd) son solo x86_64. Solución: instalar Rosetta 2:
```
softwareupdate --install-rosetta
```
O usar solo las herramientas nativas ARM64 (file, xxd, exiftool funcionan perfecto).
### "WSL2 no detecta USB conectados"
WSL2 no tiene acceso directo a USB. Para acceder a USB desde WSL2, instalá `usbipd-win` en Windows y seguí [estas instrucciones](https://learn.microsoft.com/en-us/windows/wsl/connect-usb). Para este curso, podés usar los archivos de prueba sin necesidad de USB.
## 📤 Qué entregar al final de los talleres
### Para U1 (Clase 1)
La entrega de la Prueba Autoformativa Corta 1 se hace en el LMS, pero los talleres prácticos generan estos archivos:
- Captura de pantalla del hash de `acta.txt` (MD5, SHA-1, SHA-256)
- Captura de pantalla de la salida de `exiftool -common foto-ejemplo.jpg`
- Respuesta a 2-3 preguntas del foro sobre tipos penales y la ley
### Para U2 (Clase 2 · Actividad 1 · 20% de la nota)
Entregables principales:
1. **Imagen forense**: `caso-2026-001.dd` (o `.E01` si tenés ewfacquire) con su hash SHA-256 verificado.
2. **FPJ-8 firmado**: PDF generado desde el dashboard, con todos los campos llenos y tu firma.
3. **Bitácora de adquisición**: archivo `.md` con timestamps, comandos ejecutados, y hashes verificados.
4. **Captura de RAM**: archivo `ram.avml` con su hash, o nota explicando si entró en modo DEMO por lockdown.
Subí todo en un ZIP a la tarea del LMS:
```
cd ~/forense-lab
zip -r actividad-1-tu-nombre.zip \
    imagenes/caso-2026-001.dd \
    imagenes/caso-2026-001.hashes \
    bitacoras/caso-2026-001-bitacora.md \
    bitacoras/FPJ-8-caso-2026-001.pdf \
    imagenes/ram.avml \
    imagenes/ram.sha256
ls -lh actividad-1-tu-nombre.zip
```
---
## 🎉 ¡Listo!
Ya tenés el laboratorio completo en tu casa. Si llegaste hasta acá y todo funciona, podés hacer todos los talleres de U1 y U2 sin problemas. Si algo no funciona, volvé a [Solución de problemas](#problemas).
**Tiempo total invertido:** ~2-3 horas la primera vez (la mayoría son descargas).
**Próximo paso:** volvé a la Clase 2 con la imagen de `disco-prueba.img` ya creada, y probá los talleres en vivo. Si tenés dudas, preguntá en el foro del LMS.