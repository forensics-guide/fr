# Revisar los procesos en ejecución

Una computadora infectada con programas espía debería tener algunos procesos maliciosos ejecutándose en todo momento, monitoreando el sistema y recolectando datos para ser transmitidos al servidor de [Comando y Control](https://securitywithoutborders.org/resources/digital-security-glossary.html#cnc) de los atacantes. Por consiguiente, otro paso necesario para evaluar una computadora con Windows sospechosa es extraer la lista de procesos en ejecución y averiguar si alguno de ellos muestra características sospechosas.

Existen un par de herramientas disponibles para ello.

**Advertencia:** los programas espía más sofisticados podrían ser capaces de evadir esta herramienta, ya sea ocultando sus propias entradas en el árbol de procesos, o tal vez finalizando de inmediato si detectan que se está ejecutando alguna de estas herramientas. En esta guía proporcionamos cierta metodología inicial y sugerencias para realizar una evaluación inicial. Sin embargo, una lista de procesos limpia no garantiza necesariamente un sistema limpio.

Antes de proceder a esta verificación, se recomienda que cierre todas las aplicaciones visibles en ejecución, con el fin de reducir al mínimo los resultados de las herramientas que va a ejecutar.

## Process Explorer

[Process Explorer](https://technet.microsoft.com/en-us/sysinternals/processexplorer.aspx) es otra herramienta de la [Sysinternals Suite](https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite) de Microsoft, y recoge todos los procesos que se están ejecutando en el sistema en un árbol:

![A screenshot of the Sysinternals Process Explorer. It diplays a list of processes. Some are not highlighted at all, whereas others are highlighted in red, with their sub-processes highlighted in cyan blue, dark grey, light purple, or not lighted at all. The explorer also shows how much CPU each process utilizes, how many private bytes it has, its working set, PID, description, company name, and VirusTotal score.](../.gitbook/assets/procexp.png)

La metodología para verificar la existencia de procesos en ejecución sospechosos es algo similar a la que describimos en la sección [Revisar programas que se ejecutan al encender la computadora](https://pellaeon.gitbook.io/mobile-forensics/windows/autoruns).

### 1. Verificar firmas de imágenes

De forma similar a Autoruns, Process Explorer también permite verificar las firmas de aplicaciones en ejecución haciendo clic en _Options_ y activando "_Verify Image Signatures_". Aquí se aplican también las mismas consideraciones y advertencias que describimos en la [sección anterior](https://pellaeon.gitbook.io/mobile-forensics/windows/autoruns). Más aún con los procesos en ejecución, el hecho de que una aplicación en proceso esté firmada no significa necesariamente que sea segura. El malware a menudo utiliza técnicas como [Process Hollowing](https://attack.mitre.org/techniques/T1093/) o [DLL Sideloading](https://attack.mitre.org/techniques/T1073/) para ejecutar código desde dentro del contexto de una aplicación legítima y firmada con el objetivo de impedir su detección.

![Another screenshot of Process Explorer. Somebody opened the Options menu and is selecting the Verify Image Signatures menu item therein.](../.gitbook/assets/procexp2.png)

### 2. Buscar scripts

En la actualidad, los atacantes suelen aprovechar las capacidades de scripting de Microsoft Windows, como PowerShell y Windows Script Host, debido a su flexibilidad e incluso a su capacidad para evadir la detección. Estas herramientas de scripting son utilizadas habitualmente por empresas para automatizar configuraciones de sistemas internos. Es menos común ver que las aplicaciones de consumo las utilicen, por lo que cualquier proceso relacionado que se ejecute debe ser inspeccionado más a fondo.

Estos procesos normalmente se llamarían `powershell.exe` o `wscript.exe`.

A continuación se muestra un ejemplo de Process Explorer mostrando un script PowerShell obviamente malicioso ejecutándose en el sistema:

![A screenshot of Process Explorer. It is displaying a tooltip over powershell.exe and its sub-process conhost.exe. The tooltip displays obfuscated PowerShell code, along with a URL that has been blanked out by its author.](../.gitbook/assets/procexp_powershell.png)

Al pasar el cursor por encima del nombre del proceso, se muestran los argumentos de la línea de comandos, donde se ve claramente que el script está intentando descargar y ejecutar algún código adicional. Observe también el uso de mayúsculas y minúsculas variadas, como "doWnLoAdfile": se trata de un truco muy básico que utilizan los atacantes para evadir patrones de detección igualmente básicos por parte del software de seguridad.

### 3. Buscar DLL en ejecución

El malware a veces se presenta también en forma de una [Biblioteca de Enlace Dinámico (DLL)](https://support.microsoft.com/en-us/help/815065/what-is-a-dll) que, a diferencia de una aplicación independiente (en otras palabras, un archivo `.exe`), necesita ser lanzada por un cargador. Windows ofrece algunos programas para lanzar DLL, como `regsvr32.exe` y `rundll32.exe`, que están firmados por Microsoft.

Busque si alguno de esos procesos se está ejecutando e intente determinar qué archivo DLL está ejecutando. Por ejemplo, en la siguiente captura de pantalla, podemos ver un sistema Windows infectado ejecutando un archivo DLL malicioso ubicado en `C:\Usuarios\<Nombredeusuario>\AppData\` mediante `regsvr32.exe`.

![A screenshot of Process Explorer. It is displaying a tooltip over regsvr32.exe. The tooltip displays a shell command to load a DLL.](../.gitbook/assets/procexp_regsvr.png)

### 4. Buscar procesos de aplicaciones que deberían ser visibles

Entre las muchas técnicas que suelen utilizar los atacantes se encuentra, por ejemplo, [Process Hollowing](https://attack.mitre.org/techniques/T1093/). Process Hollowing consiste en lanzar una aplicación legítima (como Internet Explorer o Google Chrome), vaciar su memoria y sustituirla por código malicioso, que luego será ejecutado. Esto generalmente se hace para ocultar el código malicioso, hacerlo aparecer como una aplicación legítima (que entonces sólo sería una cáscara vacía), evadir el firewall de aplicaciones y tal vez evadir algunos otros productos de seguridad.

Por ejemplo, si ve un proceso `iexplore.exe` en ejecución, cuando claramente no hay ninguna ventana de Internet Explorer abierta, debería considerarlo una señal preocupante.

![A screenshot of Process Explorer. A process that is called explore.exe and which has the Internet Explorer logo is selected.](../.gitbook/assets/procexp_iexplore.png)

### Opcional: 5. Buscar programas en VirusTotal

De forma similar a la sección [Autoruns](https://pellaeon.gitbook.io/mobile-forensics/windows/autoruns), Process Explorer también ofrece buscar procesos en ejecución en VirusTotal mediante la búsqueda del hash criptográfico de los respectivos archivos ejecutables. Esto puede activarse haciendo clic en _Options_ > _VirusTotal.com_ y habilitando _Check VirusTotal.com_.

![Another screenshot of Process Explorer. Somebody opened the Options menu, floated over the VirusTotal.com submenu, and is selecting the 'Check VirusTotal.com' menu item.](../.gitbook/assets/procexp3.png)

**Nota:** las mismas consideraciones y advertencias explicadas en la [sección anterior](https://pellaeon.gitbook.io/mobile-forensics/windows/autoruns) se aplican aquí también. Asegúrese de leerlas antes de proceder.

## CrowdInspect

[CrowdInspect](https://www.crowdstrike.com/resources/community-tools/crowdinspect-tool/) es una herramienta desarrollada por la empresa de seguridad estadounidense CrowdStrike. CrowdInspect es muy similar a Process Explorer, pero tiene algunas ventajas. En primer lugar, la información presentada tiende a ser más compacta. En segundo lugar, no sólo muestra los procesos que están activos en ese momento, sino que también puede mostrar los procesos que han terminado desde que se inició la herramienta (que tal vez usted haya pasado por alto porque se ejecutaron demasiado rápido). Por último, realiza una serie de verificaciones más que Process Explorer no soporta actualmente.

![A screenshot of CrowdInspect. It has several options highlighted, including TCP, UDP, Localhost, System, and Full Path. It displays a host of data about processes, including their names, PIDs, types of network connections, local and remote IP addresses.](../.gitbook/assets/crowdinspect.png)

### Verificar posibles inyecciones de procesos

Probablemente la característica más interesante que presenta CrowdInspect, es la capacidad de identificar cualquier [proceso inyectado](https://attack.mitre.org/techniques/T1055/). La inyección de procesos es una categoría de técnicas cuyo objetivo es ejecutar código malicioso en el contexto de una aplicación independiente, generalmente legítima (como `explorer.exe`). La inyección de procesos suele ser utilizada por los autores de malware para obtener privilegios adicionales en el sistema o, por ejemplo, para evadir la detección.

CrowdInspect le alertará de cualquier proceso inyectado mostrando un punto rojo visible bajo la columna "_Inject_". Los procesos inyectados son, por lo general, un muy buen indicador de que puede haber una infección activa en la computadora analizada.

![A screenshot of CrowdInspect, with a process called iexplore.exe selected, which has a red dot in the column marked Inject.](../.gitbook/assets/crowdinspect_injection.png)
