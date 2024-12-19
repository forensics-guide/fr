---
description: >-
  Esta página sirve como una nota para configurar el monitoreo del tráfico de red en Linux.
---

# Nota: Monitorear el tráfico de red en Linux

## Configurar el uso compartido de WiFi

Usando KDE

1. Conecte un adaptador wifi que soporte el modo AP.  
2. Haga clic con el botón derecho en el icono Red de la barra de tareas. Haga clic en *Configurar conexiones de red*.  
3. Haga clic en Agregar, seleccione *Wi-Fi (Compartido)*.  
4. En la pestaña Wi-Fi, configure:  
   1. SSID: el que desee  
   2. Limitar dispositivo: seleccione el adaptador que acaba de conectar.  
   3. Seguridad inalámbrica: configure una contraseña personal WPA2  
   4. IPv4: *Método: Compartir con otras computadoras*  
5. Elija un nombre de conexión (básicamente el nombre del perfil de red)  
6. Guarde  
7. Haga clic con el botón izquierdo en el icono Red, haga clic en Conectar en el nombre de conexión que acaba de crear.  
8. Ahora el AP debería estar activado y debería ver desde otros dispositivos el SSID que acaba de configurar.

## Configurar la redirección hacia un proxy de interceptación

Para el análisis forense móvil, generalmente no es necesario interceptar el tráfico SSL, ya que para interceptar el tráfico SSL normalmente habría que configurar una autoridad de certificación (CA) SSL autofirmada para el dispositivo móvil. Sin embargo, la mayoría de las aplicaciones no confiarían en una CA SSL importada por el usuario. Para hacer que las aplicaciones confíen en la CA autofirmada habría que hacer root el dispositivo Android, lo que no se recomienda porque al realizar análisis forense no se debe alterar el dispositivo sujeto.
