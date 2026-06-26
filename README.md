
# 🛠️ TOBU - Multi-Tool de Reconocimiento y Utilidades OSINT
Una potente herramienta de consola con interfaz interactiva que reúne utilidades de auditoría de red, recopilación de información (OSINT) y análisis de seguridad en un solo lugar.

## 🚀 Características Principales:

 * **🌐 Escaneo de Puertos:** Escaneo ágil de rangos TCP con barra de progreso en tiempo real.

 * **📍 Geolocalización & Reputación IP:** Rastreo de ubicación (país, ciudad, ISP) y detección de VPNs, Proxies o Hosting.
 
* **👤 OSINT en Redes Sociales:** Buscador masivo de nombres de usuario en plataformas como GitHub, Instagram, X, TikTok, YouTube y Reddit.

 * **📱 Análisis de Dispositivo & Telefonía:** Extracción de info de hardware/sistema (Android, RAM, kernel) y validación avanzada de números telefónicos (operadora y país).
 
* **🛡️ Seguridad de Credenciales:** Generador dinámico de *wordlists* personalizadas para test de fuerza bruta y analizador de seguridad de contraseñas.

* 🎨 Personalización Visual:
** Banner ASCII dinámico, animaciones de carga y 4 temas visuales interactivos (*Cyber Blue, Matrix Green, Blood Red, Purple Hacker*).

## 🖐️REQUISITOS:
TERMUX (emulador de terminal en android)
y descargar phonenumbers y Python en este:
```bash 
pkg install python3 && pip install phonenumbers
```

# ⭐SCRIPT: 
```bash
#!/data/data/com.termux/files/usr/bin/bash
A='\033[0;34m'; R='\033[0;31m'; V='\033[0;32m'; AM='\033[1;33m'; C='\033[0;36m'; B='\033[1;37m'; X='\033[0m'

cambiar_tema(){ 
clear; banner; echo -e "${AM}=== 🎨 SELECCIONAR TEMA ===${X}\n"; 
echo -e "${B}1${X} │ ${A}🔵 Cyber Blue${X}"; 
echo -e "${B}2${X} │ ${V}💚 Matrix Green${X}"; 
echo -e "${B}3${X} │ ${R}🔴 Blood Red${X}"; 
echo -e "${B}4${X} │ ${C}🟣 Purple Hacker${X}"; 
echo -e "${B}0${X} │ ${AM}⬅️ Volver${X}\n"; 
read -p "Opcion: " t; 
case $t in 
1) A='\033[0;34m'; R='\033[0;31m'; V='\033[0;32m'; AM='\033[1;33m'; C='\033[0;36m'; B='\033[1;37m' ;; 
2) A='\033[0;32m'; R='\033[0;32m'; V='\033[0;32m'; AM='\033[0;32m'; C='\033[0;32m'; B='\033[1;32m' ;; 
3) A='\033[0;31m'; R='\033[0;31m'; V='\033[0;31m'; AM='\033[0;31m'; C='\033[0;31m'; B='\033[1;31m' ;; 
4) A='\033[0;35m'; R='\033[0;35m'; V='\033[0;35m'; AM='\033[1;35m'; C='\033[0;35m'; B='\033[1;35m' ;; 
0) return ;; 
esac; 
echo -e "\n${V}Tema aplicado ✓${X}"; sleep 1; 
}

banner(){ clear; echo -e "${A}████████╗ ██████╗ ██████╗ ██╗   ██╗"; echo "╚══██╔══╝██╔═══██╗██╔══██╗██║   ██║"; echo "   ██║   ██║   ██║██████╔╝██║   ██║"; echo "   ██║   ██║   ██║██╔══██╗██║   ██║"; echo "   ██║   ╚██████╔╝██████╔╝╚██████╔╝"; echo "   ╚═╝    ╚═════╝ ╚═════╝  ╚═════╝ "; echo -e "${X}"; echo -e "${B}     Herramienta de escaneo v3.3${X}"; echo ""; }

matrix(){ for i in {1..15}; do echo -e "${V}$(cat /dev/urandom | tr -dc '01' | head -c 60)${X}"; sleep 0.04; done; clear; }

cargando(){ clear; matrix; echo -e "${R}[ $1 CARGANDO ]${X}"; sleep 1; }

barra(){ total=$2; actual=$1; ancho=20; lleno=$((actual*ancho/total)); vacio=$((ancho-lleno)); perc=$((actual*100/total)); printf "\r${C}[${V}"; for i in $(seq 1 $lleno); do printf "█"; done; for i in $(seq 1 $vacio); do printf "░"; done; printf "${C}] %3d%% Puerto %d${X}" $perc $actual; }

portscan(){ cargando "ESCANEO DE PUERTOS"; banner; read -p "IP a escanear: " ip; read -p "Rango puertos ej 20-100: " rango; echo ""; IFS=- read min max <<< "$rango"; total=$((max-min+1)); found=0; for p in $(seq $min $max); do barra $p $total; timeout 0.3 bash -c "echo >/dev/tcp/$ip/$p" 2>/dev/null && echo -e "\n${V}[PUERTO $p ABIERTO] \a${X}" && found=1; done; echo ""; if [ $found -eq 0 ]; then echo -e "${AM}No se encontraron puertos abiertos${X}"; fi; echo ""; read -p "Enter para volver..."; }

geoip(){ cargando "UBICACION POR IP"; banner; read -p "Ingresa la IP: " ip; echo ""; echo -e "${C}Obteniendo datos...${X}"; echo ""; res=$(curl -s --connect-timeout 5 "http://ip-api.com/json/$ip?lang=es&fields=status,country,regionName,city,isp,lat,lon,timezone,query" 2>/dev/null); if [ -z "$res" ]; then echo -e "${R}Error: Sin conexion o IP invalida${X}"; else echo "$res" | python3 -m json.tool 2>/dev/null || echo "$res"; fi; echo ""; read -p "Enter para volver..."; }

ip_rep(){ cargando "IP REPUTATION"; banner; read -p "Ingresa la IP: " ip; echo ""; echo -e "${C}Analizando reputacion...${X}"; echo ""; data=$(curl -s --connect-timeout 5 "http://ip-api.com/json/$ip?fields=status,query,country,as,proxy,hosting" 2>/dev/null); if [ -z "$data" ]; then echo -e "${R}Error: Sin conexion${X}"; else echo "$data" | python3 -m json.tool 2>/dev/null || echo "$data"; echo ""; if echo "$data" | grep -q '"proxy":true'; then echo -e "${R}ALERTA: Proxy/VPN detectado${X}"; elif echo "$data" | grep -q '"hosting":true'; then echo -e "${R}ALERTA: IP de Hosting${X}"; else echo -e "${V}IP residencial normal${X}"; fi; fi; echo ""; read -p "Enter para volver..."; }

wordlist_gen(){ cargando "GENERADOR WORDLISTS"; banner; read -p "Palabra base ej admin: " base; echo ""; echo -e "${C}Generando combinaciones...${X}\n"; sufijos=("123" "2026" "!" "@" "01" "admin" "qwerty" "1234"); for s in "${sufijos[@]}"; do echo "${base}${s}"; echo "${base^}${s}"; done > wordlist_$base.txt; cat wordlist_$base.txt; echo -e "\n${V}Guardado en: wordlist_$base.txt${X}"; echo ""; read -p "Enter para volver..."; }

pass_check(){ cargando "VERIFICACION PASS"; banner; read -p "Ingresa la contraseña a analizar: " pass; len=${#pass}; fuerza=0; [[ $pass =~ [a-z] ]] && ((fuerza++)); [[ $pass =~ [A-Z] ]] && ((fuerza++)); [[ $pass =~ [0-9] ]] && ((fuerza++)); [[ $pass =~ [^a-zA-Z0-9] ]] && ((fuerza++)); echo ""; echo -e "${B}Longitud: ${X}$len caracteres"; echo -e "${B}Mayusculas: ${X}$([[ $pass =~ [A-Z] ]] && echo 'Si' || echo 'No')"; echo -e "${B}Numeros: ${X}$([[ $pass =~ [0-9] ]] && echo 'Si' || echo 'No')"; echo -e "${B}Simbolos: ${X}$([[ $pass =~ [^a-zA-Z0-9] ]] && echo 'Si' || echo 'No')"; echo ""; if [ $len -ge 12 ] && [ $fuerza -eq 4 ]; then echo -e "${V}FUERTE 🔒 - Muy dificil de crackear${X}"; elif [ $len -ge 8 ] && [ $fuerza -ge 3 ]; then echo -e "${AM}MEDIA ⚠️ - Aceptable pero mejorable${X}"; else echo -e "${R}DEBIL 💀 - Facil de crackear${X}"; fi; echo ""; read -p "Enter para volver..."; }

herramientas_ip(){ while true; do banner; echo -e "${AM}=== 🌐 HERRAMIENTAS POR IP ===${X}"; echo ""; echo -e "${AM}1${X} │ ${C}🔍 Escaneo de Puertos${X}"; echo -e "${AM}2${X} │ ${C}📍 Ubicacion por IP${X}"; echo -e "${AM}3${X} │ ${C}🛡️ IP Reputation${X}"; echo -e "${AM}0${X} │ ${R}⬅️ Volver al menu${X}"; echo ""; read -p "Opcion: " op; case $op in 1) portscan ;; 2) geoip ;; 3) ip_rep ;; 0) break ;; *) echo -e "${R}Invalida${X}"; sleep 1 ;; esac; done; }

dispositivo(){ cargando "DISPOSITIVO"; banner; echo -e "${V}Info del telefono:${X}"; echo ""; echo -e "${B}Modelo: ${X}$(getprop ro.product.model 2>/dev/null)"; echo -e "${B}Marca: ${X}$(getprop ro.product.brand 2>/dev/null)"; echo -e "${B}Android: ${X}$(getprop ro.build.version.release 2>/dev/null)"; echo -e "${B}Kernel: ${X}$(uname -r)"; echo -e "${B}RAM: ${X}$(free -h 2>/dev/null | grep Mem | awk '{print $2}')"; echo ""; read -p "Enter para volver..."; }

userfinder(){ cargando "USER FINDER MASIVO"; banner; read -p "Usuario a buscar: " u; echo ""; echo -e "${AM}Buscando en redes...${X}"; echo ""; echo -e "${B}[+] GitHub: ${X}github.com/$u"; echo -e "${B}[+] Instagram: ${X}instagram.com/$u"; echo -e "${B}[+] Twitter/X: ${X}x.com/$u"; echo -e "${B}[+] TikTok: ${X}tiktok.com/@$u"; echo -e "${B}[+] YouTube: ${X}youtube.com/@$u"; echo -e "${B}[+] Reddit: ${X}reddit.com/user/$u"; echo ""; read -p "Enter para volver..."; }

telefono(){ cargando "NUMERO TELEFONICO"; banner; read -p "Numero con codigo pais ej +54911: " n; echo ""; python3 -c "import phonenumbers; from phonenumbers import geocoder,carrier; z=phonenumbers.parse('$n',None); print('Valido:',phonenumbers.is_valid_number(z)); print('Pais:',geocoder.description_for_number(z,'es')); print('Operadora:',carrier.name_for_number(z,'es'))" 2>/dev/null || echo "Instala: pip install phonenumbers"; echo ""; read -p "Enter para volver..."; }

while true; do banner; echo -e "${AM}╔════════════╗${X}"; sleep 0.08; echo -e "${AM}║ ${B}1${AM} │ ${C}🌐 HERRAMIENTAS POR IP${AM}          ║${X}"; sleep 0.08; echo -e "${AM}║ ${B}2${AM} │ ${V}📱 Escaneo de Dispositivo${AM}       ║${X}"; sleep 0.08; echo -e "${AM}║ ${B}3${AM} │ ${C}👤 User Finder Masivo${AM}           ║${X}"; sleep 0.08; echo -e "${AM}║ ${B}4${AM} │ ${AM}📞 Numero Telefonico${AM}            ║${X}"; sleep 0.08; echo -e "${AM}║ ${B}5${AM} │ ${C}💥 Test fuerza bruta: wordlists${AM} ║${X}"; sleep 0.08; echo -e "${AM}║ ${B}6${AM} │ ${V}🔒 Verificacion seguridad pass${AM}  ║${X}"; sleep 0.08; echo -e "${AM}║ ${B}7${AM} │ ${C}🎨 Cambiar tema colores${AM}         ║${X}"; sleep 0.08; echo -e "${AM}║ ${B}0${AM} │ ${R}❌ Salir${AM}                        ║${X}"; echo -e "${AM}╚════════════╝${X}"; echo ""; read -p "Opcion: " o; case $o in 1) herramientas_ip ;; 2) dispositivo ;; 3) userfinder ;; 4) telefono ;; 5) wordlist_gen ;; 6) pass_check ;; 7) cambiar_tema ;; 0) clear; echo -e "${R}Saliendo...${X}"; sleep 1; clear; exit 0 ;; *) echo -e "${R}Invalida${X}"; sleep 1 ;; esac; done
```
* Tan solo es copiar y pegar el script en Termux 👌

# AVISOS Y RECOMENDACIONES SOBRE EL USO:
* ❗Al seleccionar la opción "0": "SALIR", se cerrará automáticamente la herramienta y la app.

* ❗Al colocar un número telefónico en la opcion "4", Le recomendamos que use un código de país, ejemplo: "+54"xxx

* ❗Las opciones del submenú de herramientas de IP "1", Pueden dejar de funcionar dependiendo de tu dispositivo.



