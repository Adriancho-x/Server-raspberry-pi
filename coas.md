# Configuración y Optimización de Antenas USB en Raspberry Pi 🚀

Guía paso a paso para garantizar un funcionamiento estable y optimizado de antenas USB de alta potencia en Raspberry Pi.

---

### **1. Asegúrate de tener suficiente energía**
- Las antenas USB 3.0 de alta potencia pueden requerir más energía de la que la Raspberry Pi puede proporcionar a través de sus puertos USB. Si experimentas problemas, considera usar:
  - Un hub USB alimentado externamente.
  - Una fuente de alimentación para la Raspberry Pi que tenga al menos **3A** (15W).

---

### **2. Configura el nombre persistente de las interfaces**
- A veces, las interfaces (`wlan0`, `wlan1`) pueden cambiar de nombre después de un reinicio, lo que genera problemas. Para evitar esto:
  1. Encuentra la dirección MAC de las interfaces:
     ```bash
     ip link
     ```
  2. Crea un archivo de reglas de udev:
     ```bash
     sudo nano /etc/udev/rules.d/70-persistent-net.rules
     ```
  3. Escribe algo como:
     ```plaintext
     SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="XX:XX:XX:XX:XX:XX", NAME="wlan0"
     SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="YY:YY:YY:YY:YY:YY", NAME="wlan1"
     ```
     Reemplaza `XX:XX:XX:XX:XX:XX` y `YY:YY:YY:YY:YY:YY` con las direcciones MAC reales de tus interfaces.

---

### **3. Optimiza la señal Wi-Fi del punto de acceso**
- **Selecciona un canal adecuado**: Usa herramientas como `iwlist` para encontrar canales menos saturados:
  ```bash
  sudo iwlist wlan1 scan | grep Frequency
