# 🛡️ Guía Definitiva de Comandos Nmap (Network Mapper)

> [!NOTE]
> Referencia rápida de comandos, técnicas de escaneo y ejemplos reales con Nmap para auditorías de seguridad, pentesting y análisis de redes (SEC101).

---

## 1. Escaneos Básicos y Descubrimiento de Hosts

* Escaneo de IP o Dominio simple:
  nmap 192.168.1.1
  *Ejemplo real:* nmap scanme.nmap.org (Verifica el estado del host y sus puertos principales).

* Escaneo de múltiples IPs o un rango (CIDR):
  nmap 192.168.1.1,5,10
  nmap 192.168.1.0/24
  *Ejemplo real:* nmap 192.168.1.15-25 (Escanea un rango específico de equipos en la red local).

* Escaneo desde un archivo de texto con objetivos:
  nmap -iL targets.txt
  *Ejemplo real:* nmap -iL scope_ips.txt (Útil en auditorías para cargar una lista de objetivos aprobados).

* Solo descubrimiento de hosts (sin escanear puertos - Ping Sweep):
  nmap -sn 192.168.1.0/24
  *Ejemplo real:* nmap -sn 10.10.10.0/24 (Identifica rápidamente qué máquinas están encendidas en la red sin perder tiempo escaneando puertos).

---

## 2. Tipos de Escaneo de Puertos

* Escaneo TCP SYN (Por defecto / Stealth Scan): Rápido y sigiloso, no completa el handshake de 3 vías. (Requiere privilegios de root)
  sudo nmap -sS 192.168.1.1
  *Ejemplo real:* sudo nmap -sS 192.168.1.50 (El escaneo estándar por excelencia para pentesters).

* Escaneo TCP Connect (Conexión completa): Utiliza la llamada al sistema connect() del SO. Se usa cuando no eres root.
  nmap -sT 192.168.1.1
  *Ejemplo real:* nmap -sT 192.168.1.50 (Ideal si ejecutas Nmap con un usuario sin privilegios de administrador).

* Escaneo UDP: Identifica servicios que corren sobre UDP (DNS, SNMP, DHCP). Suele ser lento.
  sudo nmap -sU 192.168.1.1
  *Ejemplo real:* sudo nmap -sU -p 53,161 10.10.10.5 (Verifica si los puertos de DNS o SNMP están abiertos en el objetivo).

* Escaneo FIN, XMAS y NULL: Técnicas avanzadas para evadir firewalls básicos basándose en flags TCP.
  sudo nmap -sF 192.168.1.1   # FIN scan
  sudo nmap -sX 192.168.1.1   # XMAS scan
  sudo nmap -sN 192.168.1.1   # NULL scan
  *Ejemplo real:* sudo nmap -sX -p 80 192.168.1.10 (Envía paquetes con flags alteradas para probar reglas de filtrado de paquetes).

---

## 3. Especificación de Puertos y Opciones de Rendimiento

* Escaneo de puertos específicos:
  nmap -p 80,443 192.168.1.1       # Puertos concretos
  nmap -p- 192.168.1.1             # Todos los puertos (1-65535)
  nmap -p- --top-ports 100 192.168.1.1 # Los 100 puertos más comunes
  *Ejemplo real:* nmap -p 22,80,443,3389 192.168.1.100 (Revisa rápidamente servicios críticos como SSH, Web y RDP).

* Plantillas de Temporización y Velocidad (-T0 a -T5):
  - T3: Normal (por defecto).
  - T4: Agilizado (ideal para redes rápidas).
  - T5: Insane (muy agresivo).
  *Ejemplo real:* nmap -T4 -p 80 192.168.1.0/24 (Acelera notablemente el escaneo masivo de una subred).

---

## 4. Detección de Versiones y Sistema Operativo (OS)

* Detección de Servicios y Versiones (-sV): Identifica la aplicación y versión exacta.
  nmap -sV 192.168.1.1
  *Ejemplo real:* nmap -sV -p 21 192.168.1.10 (Descubre la versión exacta del servidor FTP para buscar vulnerabilidades asociadas).

* Detección de Sistema Operativo (-O): Analiza la pila TCP/IP para adivinar el SO.
  sudo nmap -O 192.168.1.1
  *Ejemplo real:* sudo nmap -O 192.168.1.25 (Intenta determinar si el objetivo corre Linux, Windows o un dispositivo de red).

* Escaneo Agresivo (-A): Activa SO, versiones, scripts por defecto y traceroute.
  sudo nmap -A 192.168.1.1
  *Ejemplo real:* sudo nmap -A 192.168.1.5 (Realiza un perfilado completo del sistema en un solo comando).

---

## 5. Uso de Scripts de Nmap (NSE)

* Ejecutar scripts por categoría:
  nmap --script default 192.168.1.1      # Scripts seguros por defecto
  nmap --script vuln 192.168.1.1         # Comprobar vulnerabilidades conocidas
  *Ejemplo real:* nmap --script vuln 192.168.1.50 (Analiza fallos y CVEs conocidos en los servicios abiertos).

* Ejecutar un script específico:
  nmap --script http-title 192.168.1.1
  *Ejemplo real:* nmap --script http-title 192.168.1.80 (Extrae el título HTML de la página web alojada en el servidor).

---

## 6. Opciones de Guardado y Salida

* Guardar en múltiples formatos (Normal, Grepable y XML):
  nmap -oA resultado_escaneo 192.168.1.1
  *Ejemplo real:* nmap -sV -oA auditoria_servidor 192.168.1.10 (Genera archivos .nmap, .gnmap y .xml para reportes).

* Guardar únicamente en formato XML:
  nmap -oX salida.xml 192.168.1.1
  *Ejemplo real:* nmap -oX scan_results.xml 192.168.1.15 (Ideal para importar resultados en herramientas de análisis o bases de datos).
