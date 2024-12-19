# Verificar los mensajes sospechosos

Mensajes que contienen enlaces son un vector de ataque muy común.

![A screenshot of an online translation service, showing a message in Arabic and a translation into English. The translation says 'Turkey asks the Egyptian opposition channels to stop criticizing Egypt, and Cairo comments on the move...'](https://citizenlab.ca/wp-content/uploads/2021/12/Fig-7.png)

Los enlaces pueden enviarse desde cualquier aplicación de mensajería o SMS. Existen varios tipos de enlaces maliciosos:

| Objetivo del enlace | Finalidad del atacante | Sofisticación | Mitigación |
| :---- | :---- | :---- | :---- |
| Sitio web de phishing, como una página web que parece la página de inicio de sesión de Google | Engañar al usuario para que ingrese datos personales o contraseñas | Bajo | Verificar los nombres de dominio de las páginas web y los certificados SSL |
| Descarga de aplicaciones | Convencer al usuario para descargar e instalar la aplicación | Bajo | No instalar aplicaciones fuera de las tiendas de aplicaciones |
| Página web que contenga explotación web, como XSS (Cross Site Scripting) | Robar las cookies de la sesión en línea u operar la sesión abierta actual | Media | No hacer clic en enlaces enviados por desconocidos |
| Página web que contenga una explotación del navegador | Explotar la vulnerabilidad del navegador o de la aplicación | Alto | No hacer clic en enlaces enviados por desconocidos |

### Guardar mensajes

1. Copie el mensaje completo, incluido el enlace, en el portapapeles  
2. Alternativamente, guarde una captura de pantalla que contenga el texto completo  
3. Si es posible, archive el enlace utilizando [Wayback Machine](https://web.archive.org/)

### Verificar enlaces  

Simplemente realice una búsqueda en Google del enlace, o pegue el enlace en sitios como VirusTotal.
