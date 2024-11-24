# Instalación de paquetes necesarios

Para configurar el punto de acceso, primero debes instalar algunos paquetes que son esenciales:

1. Hostapd: Este paquete se encarga de gestionar el punto de acceso WiFi.
2. Dnsmasq: Se utiliza como servidor DHCP para asignar direcciones IP a los dispositivos que se conecten al punto de acceso.
```bash
sudo apt update
sudo apt install hostapd dnsmasq
```
# Configuración de hostapd (Servidor de Acceso WiFi)
El siguiente paso es configurar hostapd para que tu Raspberry Pi actúe como un punto de acceso.

1. Abre el archivo de configuración de hostapd:
```bash
sudo nano /etc/hostapd/hostapd.conf
```
2. Agrega las siguientes configuraciones:
```bash
interface=wlan0
driver=nl80211
ssid=domotica
hw_mode=g
channel=7
wmm_enabled=0
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=domotica
wpa_key_mgmt=WPA-PSK
rsn_pairwise=CCMP
```
3. Configura hostapd para que cargue esta configuración en el archivo de configuración global:
```bash
sudo nano /etc/default/hostapd
```
Cambia la línea #DAEMON_CONF="" a:
```bash
DAEMON_CONF="/etc/hostapd/hostapd.conf"
```
# Configuración de DHCP (dnsmasq)
El siguiente paso es configurar dnsmasq para que asigne direcciones IP a los dispositivos que se conecten.

1. Abre el archivo de configuración de dnsmasq:

```bash
sudo nano /etc/dnsmasq.conf
```
Agrega las siguientes líneas al final del archivo para definir el rango de direcciones IP asignadas a los dispositivos conectados:
```
interface=wlan0
dhcp-range=192.168.4.2,192.168.4.20,255.255.255.0,24h
```
# Asignación de IP estática a wlan0
Para asegurarte de que la Raspberry Pi siempre use la misma dirección IP en el punto de acceso, asigna una IP estática a wlan0.

1Abre el archivo de configuración de dhcpcd:

```bash
sudo nano /etc/dhcpcd.conf
```
Agrega las siguientes líneas al final del archivo:
```
interface wlan0
static ip_address=192.168.4.1/24
nohook wpa_supplicant
```
# Configuración de iptables (NAT y reenvío de IP)
Si deseas compartir la conexión a Internet con los dispositivos conectados al punto de acceso, configura NAT (Network Address Translation).

Crea una regla de NAT para compartir la conexión de eth0 (Ethernet) a wlan0 (WiFi):

```
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```
Guarda las reglas de iptables para que se restauren automáticamente al reiniciar:

```
sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
```
# Activación automática al inicio
Para asegurarte de que todo se inicie automáticamente después de un reinicio, habilita los servicios y edita rc.local.

Habilita el servicio hostapd:

```
sudo systemctl enable hostapd
```
Habilita el servicio dnsmasq:

```
sudo systemctl enable dnsmasq
```
Asegúrate de que rc.local ejecute la restauración de las reglas de iptables y reinicie hostapd:

```
sudo nano /etc/rc.local
```
Asegúrate de que se añadan estas líneas antes de exit 0:

```
iptables-restore < /etc/iptables.ipv4.nat
systemctl restart hostapd
```
Dale permisos ejecutables a rc.local:

```
sudo chmod +x /etc/rc.local
```
9. Reiniciar la Raspberry Pi
Reinicia la Raspberry Pi para aplicar todas las configuraciones:

bash
Copiar código
sudo reboot
10. Verificación del servicio
Después de reiniciar, asegúrate de que el servicio hostapd esté en ejecución:

bash
Copiar código
sudo systemctl status hostapd
Puedes verificar que el punto de acceso está funcionando correctamente al buscar el SSID (domotica) desde otro dispositivo.

## Tabla de Contenidos
- [Instalación](#instalación)
- [Uso](#uso)
- [Contribuciones](#contribuciones)
- [Licencia](#licencia)

## Instalación

Describe los pasos necesarios para instalar y ejecutar el proyecto. Por ejemplo:

1. Clona el repositorio:
   ```bash
   git clone https://github.com/tu_usuario/tu_repositorio.git
   ```
2. Instala las dependencias:
   ```bash
   npm install
   ```

## Uso

Instrucciones sobre cómo usar el proyecto una vez esté instalado.

```bash
node app.js
