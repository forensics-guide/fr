# Monitorear el tráfico de red

Monitorear el tráfico de red desde un teléfono es una de las mejores formas de identificar actividades maliciosas sin interactuar directamente con el teléfono móvil, lo que evita cualquier mecanismo que el malware pueda haber desarrollado para evadir la detección. Sin embargo, esto requiere una herramienta externa para registrar el tráfico y ciertos conocimientos de la red para identificar el tráfico sospechoso.

## Limitaciones

Actualmente, muchas aplicaciones móviles y malware activan "SSL Pinning", lo que dificulta la tarea de descifrar su tráfico. La única forma de evitar el SSL Pinning es hacer root/jailbreak al dispositivo y usar herramientas como Frida. Sin embargo, para fines de análisis forense, sólo necesitamos hacerlo si sospechamos seriamente que el tráfico malicioso está oculto con el cifrado con SSL Pinning.

## Monitorear las consultas DNS

En lugar de monitorear todo el tráfico de red, lo cual requiere una configuración compleja, podemos simplemente monitorear las consultas DNS. La mayoría de los sistemas operativos permiten a los usuarios configurar sus propios servidores DNS. Podemos configurar un servidor DNS propio para registrar las consultas y apuntar el dispositivo bajo prueba a nuestro servidor.

Podemos implementar nuestro propio servidor DNS con:

* Pi-hole  
* Adguard Home

O usar un servidor DNS basado en la nube como NextDNS. La desventaja de los servidores DNS basados en la nube es que el proveedor de la nube podrá ver las consultas de los dispositivos.

Un proveedor de DNS en la nube es NextDNS, que permite configurar un servidor DNS sin necesidad de tener una cuenta. NextDNS también ofrece aplicaciones para los principales sistemas operativos con el fin de configurar el dispositivo para que utilice su servidor personalizado. Primero, vaya a [https://my.nextdns.io/start](https://my.nextdns.io/start), donde se creará un *ID de configuración* temporal, que deberá ingresar en la aplicación NextDNS. Luego, todas las consultas DNS que realice el teléfono quedarán registradas en la pestaña *Registros*.

![A screenshot of NextDNS, showing several DNS queries being collected. Many of the queries have a favicon next to them that implies they came from Apple](../.gitbook/assets/Screenshot_20220506_165152.png)

## Monitorear todo el tráfico de red desde un router Wifi

Una forma de monitorear el tráfico de red desde su teléfono es registrar el tráfico directamente desde un router WiFi utilizado por su teléfono móvil. Dependiendo de su router WiFi, puede ser posible registrar el tráfico directamente desde él (usando una herramienta como [tcpdump](https://www.tcpdump.org/)).

Si no puede registrar el tráfico utilizando su router WiFi real, puede crear fácilmente un router utilizando una [Raspberry Pi](https://www.raspberrypi.org/) y el software [RaspAP](https://raspap.com/) (consulte [aquí](https://howtoraspberrypi.com/create-a-wi-fi-hotspot-in-less-than-10-minutes-with-pi-raspberry/) un tutorial sobre cómo instalarlo).

Una vez instalado el router, puede conectarse a él mediante ssh y empezar a capturar el tráfico utilizando [tcpdump](https://www.tcpdump.org/). A continuación, conecte el teléfono móvil que desea probar a esta red wifi y registre el tráfico durante 30 minutos a una hora.

Cuando finalice la captura, descargue el archivo pcap en su computadora y revise el tráfico de la red utilizando [WireShark](https://www.wireshark.org/). Verifique especialmente los siguientes elementos:

* Conexiones a direcciones IP sin solicitudes DNS  
* Conexiones a dominios que no figuran en los [listados principales de Alexa](https://www.alexa.com/siteinfo)  
* Conexiones en puertos distintos de 80 y 443

Si encuentra algún tráfico sospechoso hacia una dirección IP o un dominio, puede utilizar las siguientes herramientas para obtener más información:

* [CentralOps](https://centralops.net/co/) permite ver el propietario de una IP o el whois de un dominio (no requiere una cuenta)  
* [RiskIQ](https://community.riskiq.com/home) permite ver el historial DNS y tiene muchas indicaciones sobre IP y dominios maliciosos (requiere una cuenta, número limitado de consultas con cuenta gratuita)  
* [AlienVault OTX](https://otx.alienvault.com/) es una base de datos extensa de indicadores maliciosos, donde puede buscar esta IP o dominio para ver si hay actividad maliciosa conocida que provenga de allí (requiere cuenta, gratuita)

Para configurar un entorno para el análisis del tráfico, consulte [esta guía](https://mobile-security.gitbook.io/mobile-security-testing-guide/general-mobile-app-testing-guide/0x04f-testing-network-communication).

## Monitorear el tráfico de red con aplicaciones en el dispositivo

Estas aplicaciones generalmente funcionan creando un servidor VPN en el dispositivo y configurando el sistema para que utilice dicho servidor.

* [NetGuard](https://netguard.me/): para Android, código abierto  
* [Lockdown](https://lockdownprivacy.com/firewall): para iOS, código abierto  
* [Blokada](https://apps.apple.com/us/app/blokada/id1508341781): para iOS, propietario

### ¿Cómo detectar el tráfico sospechoso?

* Los siguientes comportamientos son considerados más sospechosos:  
* Aplicaciones o conexiones que generan mucho tráfico de datos.  
* Aplicaciones o conexiones que suben más datos de los que descargan.  
* Conexiones que ocurren periódicamente.  
* Nombres de dominio extraños. Cuando los encuentre, simplemente búsquelos en Google o VirusTotal.  
* Puertos poco comunes.  
* Envío o recepción de datos cuando el usuario no está utilizando el teléfono (por ejemplo, mientras duerme).

## Utilizar el VPN de emergencia de Civil Sphere

El [Proyecto Civil Sphere](https://www.civilsphereproject.org/what-we-do), una organización nacida del [Stratosphere Research Laboratory](https://www.stratosphereips.org/) en la Czech Technical University de Praga, ofrece un servicio [VPN de emergencia](https://www.civilsphereproject.org/emergency-vpn) para identificar dispositivos comprometidos.

El servicio de VPN de emergencia permite analizar e identificar tráfico sospechoso de un dispositivo móvil utilizado por periodistas y ONG. [Bajo solicitud](https://www.civilsphereproject.org/get-started), el equipo de Civil Sphere le enviará unas credenciales para iniciar sesión en su VPN con su teléfono. Mientras utiliza su VPN, registrarán todo el tráfico saliente de su teléfono durante un máximo de tres días. Al finalizar el registro, analizarán el tráfico utilizando técnicas automatizadas y manuales para identificar actividad maliciosa o problemas de seguridad en las aplicaciones que se ejecutan en su teléfono, y le enviarán el resultado de este análisis por correo electrónico.

![A graphic of Emergency VPN, giving users a step-by-step guide on how it works. It asks them to 1. download the application 2. request a VPN account 3. import the new profile into your OpenVPN application 4. install and authorize the profile in your OpenVPN application 5. connect to the VPN for up to 3 days to give us enough data to analyze 6. receive a report from the team about any security threats or concerns you should be aware of](../.gitbook/assets/emergencyvpn.png)

Una de las limitaciones de esta técnica es que tiene que proporcionar el tráfico de red de su teléfono móvil a una organización externa (Civil Sphere). Usted [puede contactarlos](https://www.civilsphereproject.org/get-started) para obtener más información sobre cómo se almacenan y utilizan estos datos. La ventaja de esta solución es que puede confiar en ellos para analizar el tráfico de red, lo cual puede ser un proceso complejo y largo.

Visite [su página web](https://www.civilsphereproject.org/emergency-vpn) para obtener más información.