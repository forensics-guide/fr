# Revisar conexiones de red

El programa espía necesitará en algún momento transmitir los datos recopilados (como capturas de pantalla, contraseñas, pulsaciones de teclas, etc.) a una ubicación remota, el servidor de [Comando y Control](https://www.crowdstrike.com/cybersecurity-101/cyberattacks/command-and-control/). Aunque no es posible predecir exactamente cuándo se producirá dicha transmisión, es posible que algún programa espía establezca una conexión permanente con el servidor, o que se conecte con suficiente frecuencia como para que usted pueda detectarlo.

Para verificar si existen conexiones en curso puede, por ejemplo, registrar todo el tráfico de la red utilizando [Wireshark](https://www.wireshark.org/) e inspeccionar posteriormente los resultados almacenados. Sin embargo, un enfoque más interesante es usar herramientas que no sólo monitoreen la actividad de la red, sino que también puedan asociarlas a procesos en ejecución. En general, debería buscar procesos inusuales que se conecten a direcciones IP sospechosas.

Una herramienta popular para este propósito es [TCPView](https://technet.microsoft.com/en-us/sysinternals/tcpview.aspx), también de la Sysinternals Suite by Microsoft.

![A screenshot of Sysinternals TCPView. It shows several columns, including Process, PID, Protocol (which displays TCP, UDP, TCPV6, and UDPV6), local address, local port, remote address, and remote port.](../.gitbook/assets/tcpview.png)

Esta herramienta es bastante sencilla: presenta una lista de todas las conexiones de red establecidas y proporciona información sobre el proceso de origen y el destino. Probablemente se sorprenderá al observar la cantidad de conexiones de red activas incluso con sistemas aparentemente inactivos. Lo más frecuente es que vea actividad de red procedente de procesos en segundo plano, por ejemplo para servicios de Microsoft, Google Chrome, Adobe Reader, Skype, etc.

Otra herramienta que podemos usar para observar las conexiones de red activas es CrowdInspect, que mostramos en la sección anterior sobre la [revisión de los procesos en ejecución](https://pellaeon.gitbook.io/mobile-forensics/windows/processes). La información que proporciona CrowdInspect es muy similar a la que ofrece TCPView.

![A screenshot of CrowdInspect, with a process called iexplore.exe selected, which has a red dot in the column marked Inject. That process has established a TCP connection. It is trying to connect to remote IP 216.6.0.28](../.gitbook/assets/crowdinspect_injection.png)

Por ejemplo, en la captura de pantalla anterior podemos ver un proceso `iexplore.exe` en ejecución que no sólo ha sido marcado como inyectado, sino que parece estar intentando conectarse activamente a la IP remota `216.6.0.28`. Dado que no hay ningún Internet Explorer visible ejecutándose en el sistema, es definitivamente sospechoso ver conexiones de red activas provenientes de este proceso. TCPView mostraría lo siguiente en el mismo sistema infectado:

![A screenshot of TCP view, with a process called iexplore.exe selected. That process is trying to connect to remote IP 216.6.0.28, the same IP as in the above screenshot.](../.gitbook/assets/tcpview_infected.png)

(Nota: estas herramientas muestran intentos de conexión a ubicaciones remotas incluso si la computadora está en ese momento desconectada de Internet).

Cuando sospeche de una conexión activa, puede (preferiblemente desde otra computadora) buscar la dirección IP e intentar determinar a quién pertenece y si se sabe que es buena o mala, utilizando por ejemplo herramientas en línea [como Central Ops](https://centralops.net/co/) o [ipinfo](https://ipinfo.io/). Por ejemplo, una simple búsqueda WHOIS para esa dirección IP devolvería:

```
NetRange:       216.6.0.0 - 216.6.1.255
CIDR:           216.6.0.0/23
NetName:        SYRIAN-5
NetHandle:      NET-216-6-0-0-2
Parent:         TATAC-ARIN-9 (NET-216-6-0-0-1)
NetType:        Reassigned
OriginAS:
Organization:   STE (Syrian Telecommunications Establishment) (SSTE)
RegDate:        2005-07-21
Updated:        2005-07-21
Comment:        Fax-no-963 11 3739765
Ref:            https://rdap.arin.net/registry/ip/216.6.0.0

OrgName:        STE (Syrian Telecommunications Establishment)
OrgId:          SSTE
Address:        Fayz Mansour St
Address:        STE Building
City:           Damascus
StateProv:
PostalCode:
Country:        SY
RegDate:        2005-07-21
Updated:        2011-09-24
Ref:            https://rdap.arin.net/registry/entity/SSTE
```

Esto sugiere que el `iexplore.exe` inyectado estaba intentando conectarse de forma muy sospechosa a una dirección IP ubicada en Siria. De hecho, con fines demostrativos, utilizamos una copia antigua de DarkComet RAT que se encontró utilizada en Siria alrededor de 2011.

Incluso una simple búsqueda de la dirección IP a través de su motor de búsqueda preferido podría revelar información útil. Además, podría considerar el uso de servicios de investigación de amenazas como [RiskIQ](https://community.riskiq.com/) o [ThreatMiner](https://www.threatminer.org/) para verificar si tienen alguna información sobre las direcciones IP o los nombres de dominio con los que se encuentre.
