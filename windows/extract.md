# Extraer datos para su análisis posterior**

[pcqf](https://github.com/botherder/pcqf) es una herramienta que permite volcar la información de inicio y los procesos reales, junto con la memoria para su análisis posterior. Es muy útil, por ejemplo, si no tiene tiempo para revisar todo lo que hay en la computadora y desea volver a verificar si hay algo sospechoso más adelante.

Debe ejecutar este programa desde una memoria USB con suficiente espacio de almacenamiento, haga doble clic en el archivo binario y siga las instrucciones.

![A screenshot of a window with a commandline program running therein. The program is PCQF and is asking the user if they would like to take a memory snapshot.](../.gitbook/assets/pcqf1.PNG)

A menos que tenga una buena razón para hacerlo, se recomienda no tomar una instantánea de memoria, ya que contiene mucha información privada (por ejemplo, puede contener contraseñas).

Una vez finalizado, creará una carpeta con el nombre del ID de adquisición (una secuencia de números hexadecimales), que contiene:

* Un archivo `profile.json` que contiene información básica sobre el sistema de la computadora.  
* Un archivo `process_list.json` que contiene una lista de procesos en ejecución.  
* Un archivo `autoruns.json` que contiene una lista de todos los elementos con persistencia en el sistema.  
* Una carpeta `autoruns_bins/` que contiene copias de los archivos y ejecutables marcados para persistencia en el archivo JSON anterior.  
* Una carpeta `process_bins/` que contiene copias de los procesos en ejecución.  
* Si se solicita, una carpeta `memory/` que contendrá un volcado de memoria física junto con algunos metadatos.