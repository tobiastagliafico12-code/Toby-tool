
# 🛠️ TOBU - Multi-Tool de Reconocimiento, Utilidades OSINT y mas
Una potente herramienta de consola con interfaz interactiva que reúne utilidades de auditoría de red, recopilación de información (OSINT) y análisis de seguridad en un solo lugar.

# [ ESTA ES LA VERSION DE PRUEBA ]
### [ LA VERSION [*PRO*] ESTA A LA VENTA POR DM ]

# ⚡PREVIA:
<p align="center">
  <img src=Screenshot_20260627-025600.jpg width="400" alt="TOBU corriendo">
</p>

## 🚀 Características Principales:

 * **🌐 Escaneo de Puertos:** Escaneo ágil de rangos TCP con barra de progreso en tiempo real.

 * **📍 Geolocalización & Reputación IP:** Rastreo de ubicación (país, ciudad, ISP) y detección de VPNs, Proxies o Hosting.
 
* **👤 OSINT en Redes Sociales:** Buscador masivo de nombres de usuario en plataformas como GitHub, Instagram, X, TikTok, YouTube y Reddit.

 * **📱 Análisis de Dispositivo & Telefonía:** Extracción de info de hardware/sistema (Android, RAM, kernel) y validación avanzada de números telefónicos (operadora y país).
 
* **🛡️ Seguridad de Credenciales:** Generador dinámico de *wordlists* personalizadas para test de fuerza bruta y analizador de seguridad de contraseñas.

* 🎨 Personalización Visual:
** Banner ASCII dinámico, animaciones de carga y 4 temas visuales interactivos (*Cyber Blue, Matrix Green, Blood Red, Purple Hacker*).

* ♥️ Se va actualizando para obtener mejoras.




## 🖐️REQUISITOS:
TERMUX (emulador de terminal en android)
y descargar algunas cosas (para la version de prueba y la PRO)
```bash 
pkg update && pkg upgrade -y && pkg install bash python git curl wget jq -y && pip install requests -y
pkg install pip
pip install phonenumbers
```

# ⭐SCRIPT: 
```bash
#!/data/data/com.termux/files/usr/bin/bash
A='\033[0;34m'; R='\033[0;31m'; V='\033[0;32m'; AM='\033[1;33m'; C='\033[0;36m'; B='\033[1;37m'; M='\033[0;35m'; X='\033[0m'

startup_screen(){
clear
for i in {1..30}; do
    COLOR=$((RANDOM % 6 + 31))
    echo -e "\033[0;${COLOR}m$(cat /dev/urandom | tr -dc '01@#$%&*' | head -c 70)${X}"
    sleep 0.02
done
clear
echo -e "${A}████████╗ ██████╗ ██████╗ ██╗ ██╗"
echo "╚══██╔══╝██╔═══██╗██╔══██╗██║ ██║"
echo " ██║ ██║ ██║██████╔╝██║ ██║"
echo " ██║ ██║ ██║██╔══██╗██║ ██║"
echo " ██║ ╚██████╔╝██████╔╝╚██████╔╝"
echo " ╚═╝ ╚═════╝ ╚═════╝ ╚═════╝ "
echo -e "${X}"
echo -e "${B} Herramienta de escaneo v4.2${X}"
echo -e "${AM} by TOBU${X}\n"
sleep 1
clear
}

glitch_banner(){
clear
for i in {1..2}; do
    echo -e "${R}████████╗ ██████╗ ██████╗ ██╗ ██╗${X}"
    sleep 0.05
    clear
    echo -e "${B}████████╗ ██████╗ ██████╗ ██╗ ██╗${X}"
    sleep 0.05
done
echo -e "${A}████████╗ ██████╗ ██████╗ ██╗ ██╗"
echo "╚══██╔══╝██╔═══██╗██╔══██╗██║ ██║"
echo " ██║ ██║ ██║██████╔╝██║ ██║"
echo " ██║ ██║ ██║██╔══██╗██║ ██║"
echo " ██║ ╚██████╔╝██████╔╝╚██████╔╝"
echo " ╚═╝ ╚═════╝ ╚═════╝ ╚═════╝ "
echo -e "${X}"
echo -e "${B} Herramienta de escaneo v4.2${X}"
echo -e "${AM} by TOBU${X}\n"
}

banner(){ glitch_banner; }

matrix(){ for i in {1..8}; do echo -e "${V}$(cat /dev/urandom | tr -dc '01' | head -c 50)${X}"; sleep 0.05; done; clear; }
cargando(){ clear; matrix; echo -e "${R}[ CARGANDO ]${X}"; sleep 0.7; }

pro_lock(){
cargando
echo -e "${AM}╔══════════════╗${X}"
echo -e "${AM}║  ${R}[🔒 DISPONIBLE EN LA VERSIÓN PRO]${AM}  ║${X}"
echo -e "${AM}╚══════════════╝${X}\n"
read -p "Enter para volver..."
}

startup_screen

while true; do
banner;
echo -e "${AM}╔════════╗${X}"; sleep 0.03;
echo -e "${AM}║ ${B}1${AM} │ ${C}🌐 HERRAMIENTAS POR IP${AM} ║${X}"; sleep 0.03;
echo -e "${AM}║ ${B}2${AM} │ ${C}📱 Escaneo de Dispositivo${AM} ║${X}"; sleep 0.03;
echo -e "${AM}║ ${B}3${AM} │ ${C}👤 User Finder Masivo${AM} ║${X}"; sleep 0.03;
echo -e "${AM}║ ${B}4${AM} │ ${C}📞 Numero Telefonico${AM} ║${X}"; sleep 0.03;
echo -e "${AM}║ ${B}5${AM} │ ${C}💥 Generador Wordlists${AM} ║${X}"; sleep 0.03;
echo -e "${AM}║ ${B}6${AM} │ ${C}🔒 Verificacion seguridad${AM} ║${X}"; sleep 0.03;
echo -e "${AM}║ ${B}7${AM} │ ${C}🛡️ Detector VPN${AM} ║${X}"; sleep 0.03;
echo -e "${AM}║ ${B}8${AM} │ ${C}📧 Breach Check Email${AM} ║${X}"; sleep 0.03;
echo -e "${AM}║ ${B}9${AM} │ ${B}💀 DEMO: Simulador de Ataque${AM} ║${X}";
echo -e "${AM}║ ${B}10${AM} │ ${M}⚙️ Configuración${AM} ║${X}";
echo -e "${AM}║${X} ${AM}║${X}"; sleep 0.03;
echo -e "${AM}║ ${B}0${AM} │ ${R}❌ Salir${AM} ║${X}";
echo -e "${AM}╚════════╝${X}"; echo "";

read -p "Opcion: " o;
echo ""

# ===== CALAVERA ASCII =====
echo -e "${R}"
cat << 'EOF'
   __
  / _\.-''-.
 / _ \
 | \\ / |
 | |
 | |
  \_/ \___/
   \___/
   TOBU
EOF
echo -e "${X}\n"

case $o in
1|2|3|4|5|6|7|8|9|10) pro_lock ;; # TODAS BLOQUEADAS
0) clear; echo -e "${R}Saliendo...${X}"; sleep 1; clear; exit 0 ;;
*) echo -e "${R}Invalida${X}"; sleep 1 ;;
esac;
done
```
* Tan solo es copiar y pegar el script en Termux.👌
* puedes crear un archivo nano y ejecutarlo con bash si se te traba ✅
# AVISOS Y RECOMENDACIONES SOBRE EL USO:
* ❗Al seleccionar la opción "0": "SALIR", se cerrará automáticamente la herramienta y la app.

* ❗Al colocar un número telefónico en la opcion "4", Le recomendamos que use un código de país, ejemplo: "+54"xxx [version pro]

* ❗Esta es la version de PRUEBA, la version PRO esta en venta (escribir al Dm si esta interesado)

  



