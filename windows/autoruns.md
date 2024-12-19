# Revisar programas que se ejecutan al encender la computadora

Normalmente, los programas espía necesitan encontrar una forma de ejecutarse al encender una computadora cuando ésta se reinicia. Por eso, revisar las aplicaciones que se inician automáticamente es uno de los primeros controles que hay que realizar cuando se buscan posibles infecciones. Las computadoras con Windows tienen diferentes formas para habilitar el inicio automático, y los programas espía suelen utilizar trucos para parecer legítimos y/o evitar los métodos más comunes.

[Sysinternals Autoruns](https://technet.microsoft.com/en-ca/sysinternals/bb963902.aspx) es una herramienta que permite hacer una lista exhaustiva de los programas que se ejecutan al iniciarse el sistema. Si es posible, usted debería ejecutar este programa como Administrador:

![A screenshot of Windows Sysinternals Autoruns, which allows you to select and study autorun entries. Each entry in this list is sorted by HKLM or HKCU. The program has many different tabs open, with a tab called 'everything' currently in focus.](../.gitbook/assets/autoruns.png)

Todos los resultados se mostrarán por defecto en la pestaña principal. Al hacer clic en las demás pestañas disponibles, los resultados se filtrarán por el tipo de ejecución automática correspondiente. En general, los más interesantes serían `Logon`, `Scheduled Tasks`, `Services`.

## Buscar patrones sospechosos

Autoruns no determina automáticamente qué archivos son maliciosos y cuáles no. Al igual que con el resto de esta metodología, es necesario que con el tiempo se familiarice lo suficiente con sus resultados para identificar rápidamente cualquier anomalía o entrada que no reconozca. Sin embargo, Autoruns puede proporcionar algunas indicaciones útiles.

A veces, Autoruns puede marcar una fila concreta con un fondo rojo. Esto puede justificar una inspección más detallada, ya que podría ser señal de una entrada inusual. Por otro lado, las entradas marcadas con fondo amarillo se refieren a archivos que ya no existen en la computadora. Por lo tanto, estas entradas están rotas.

A continuación se ofrecen algunas sugerencias sobre los patrones a tener en cuenta.

### 1. Verificar firmas de imágenes

En versiones modernas de Windows, generalmente se requiere que las aplicaciones legítimas estén "firmadas" con un certificado de desarrollador. Dichos certificados permiten verificar el productor de un programa en particular (como Microsoft, Google, Adobe, u otro). Las aplicaciones no firmadas suelen estar más controladas y examinadas por los mecanismos de seguridad de Windows (como su antivirus integrado, Windows Defender). Un primer control útil es verificar si todas las aplicaciones que se ejecutan automáticamente están efectivamente firmadas, y esto puede hacerse haciendo clic en *Options \> Scan Options* and enabling *Verify code signatures*.

![A dialog box that says 'Autoruns scan options'. Four checkboxes are present: 'Scan only per-user locations,' 'Verify code signatures,' 'Check VirusTotal.com,' and, nested under the VirusTotal one, 'Submit Unknown Images'. Only the 'Verify code signatures' check box is selected.](../.gitbook/assets/autoruns5.png)

Esto volverá a lanzar el análisis de Autoruns y añadirá una nueva columna llamada "Publisher". Las aplicaciones correctamente firmadas se marcarán como "(Verified)":

![A very similar screenshot of Windows Sysinternals Autoruns as before, except that the sorting menu has one more option, called 'Publisher'. Some applications have a label at the end of their name which says '(Verified)'](../.gitbook/assets/autoruns4.png)

**Atención**: No todas las entradas Autorun verificadas son necesariamente seguras. En ocasiones, los atacantes abusan a propósito de las aplicaciones verificadas legítimas para parecer menos sospechosos, y usarlas como lanzadores para luego cargar y ejecutar código malicioso. Esto se realiza a veces utilizando, por ejemplo, `rundll32.exe` u otras aplicaciones afectadas por lo que se conoce como [DLL Sideloading](https://attack.mitre.org/techniques/T1073/).

### 2. Verificar el nombre de la entrada de Autorun

La entrada de Autorun muestra el nombre asignado a la aplicación por sus desarrolladores. Aunque esta información puede ser falsificada, a veces los atacantes son tan perezosos que escriben mal nombres legítimos falsificados (por ejemplo, "Micorsoft Ofice" o "Crhome") o simplemente dejan caracteres y números aleatorios.

### **3. Compruebe la descripción del programa

Del mismo modo, éste no es un indicador fiable, pero las aplicaciones legítimas deberían tener, por lo general, una descripción del programa visible.

### 4. Verificar la ruta de la imagen

Windows proporciona algunas carpetas estándar desde las que normalmente se instalan y ejecutan las aplicaciones legítimas. Los servicios del propio sistema operativo suelen estar ubicados en `C:\Windows\`, mientras que las aplicaciones instaladas por el usuario suelen estar ubicadas en `C:\Program Files\` o `C:\Program Files (x86)\`. Puesto que la instalación de programas en esas carpetas debería requerir cierta confirmación por parte del usuario, los atacantes suelen colocar sus archivos maliciosos en carpetas menos típicas, como `C:\Users\<Username\>\AppData\` u otras subcarpetas de `C:\Users\`.

Ejemplo de entradas sospechosas:

* El [programa espía KeyBoy](https://citizenlab.ca/2016/11/parliament-keyboy/) crea una clave de registro en `HKEY\_CURRENT\_USER\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Winlogon\\shell` con el valor `explorer.exe,C:\\Windows\\system32\\rundll32.exe "%LOCALAPPDATA%\\cfs.dal" cfsUpdate`  
* Un malware en particular utilizado en Asia Central se basa en el uso de VBScripts, que Autoruns resalta con un fondo rojo, simulando ser software de Adobe y Google. Estos resultados justificarían sin duda una inspección más detallada. Además, los scripts se ubican en `C:\Users\<Username>\AppData\`:

![Another screenshot of Windows Sysinternals Autoruns. We are in the 'Everything' tab and several entries are selected. Under 'Task Scheduler', we see three entries highlighted in red. They are labeled 'Adobe Flash Player File,' 'Adobe Flash Player Key,' and 'GoogleUpdateTaskMachineKernel'](../.gitbook/assets/autoruns_script.png)

### Opcional: 5. Buscar programas en VirusTotal

Opcionalmente, Autoruns permite verificar los archivos binarios en [VirusTotal](https://www.virustotal.com/gui/home/upload), lo que ayuda a identificar inmediatamente cualquier programa malicioso conocido y ampliamente detectado por el software Antivirus (lea más sobre esto en la siguiente sección). Para activar esta verificación, vaya a *Options \> Scan Options* y habilite "*Check VirusTotal.com*". Asegúrese de no activar "Submit Unknown Files", ya que haría que Autoruns subiera automáticamente los archivos locales al servicio, en lugar de limitarse a buscar sus hashes criptográficos. VirusTotal es una empresa, ahora propiedad de Alphabet (la empresa matriz de Google), y proporciona acceso comercial a sus datos a investigadores de seguridad y clientes de todo el mundo. Quienes tienen acceso a los servicios comerciales de VirusTotal pueden buscar y descargar cualquier archivo cargado. Por lo tanto, es posible que desee evitar enviar inadvertidamente cualquier archivo que pueda ser confidencial.

![A dialog box that says 'Autoruns scan options'. Four checkboxes are present: 'Scan only per-user locations,' 'Verify code signatures,' 'Check VirusTotal.com,' and, nested under the VirusTotal one, 'Submit Unknown Images'. Only the 'CheckVirusTotal.com' check box is selected.](../.gitbook/assets/autoruns2.png)

Una vez habilitada la opción VirusTotal, los resultados tardarán algún tiempo en aparecer. Eventualmente, debería ver una columna de VirusTotal mostrando los resultados del escaneo Antivirus. Los resultados aparecen como un valor *X/Y*, donde *X* es el número de detecciones positivas y Y es la cantidad total de software Antivirus con la que se escaneó el archivo.

![A very similar screenshot of Windows Sysinternals Autoruns as the first one, except that the sorting menu has one more option, called 'VirusTotal'. Each entry has a VirusTotal score next to it.](../.gitbook/assets/autoruns3.png)

Si no aparece ningún resultado, significa que ese programa en particular no ha sido cargado previamente en VirusTotal, y podría justificar una inspección adicional. A veces, verá algunas aplicaciones con un número de detección bajo (1 o 2): a menudo se trata de falsos positivos. Los resultados de VirusTotal que muestran un número de detección más alto (por ejemplo, 5 o superior) suelen ser una señal confiable de que esa aplicación en particular es maliciosa. Haga clic en el enlace de la *X/Y* para abrir el navegador al análisis de VirusTotal, donde podrá ver más detalles, como los identificadores de malware utilizados por el software antivirus compatible.

**Tome en consideración**: [Como ya se ha indicado](https://github.com/pellaeon/guide-to-quick-forensics/blob/master/windows/safety.md), en circunstancias normales es preferible no conectar la computadora analizada a Internet. Sin conexión a Internet, no podrá realizar comprobaciones inmediatas con VirusTotal. Sin embargo, es posible guardar los resultados de Autoruns haciendo clic en *File \> Save*... y abrir posteriormente los resultados desde otra computadora con conexión a Internet.