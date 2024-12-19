# Estructura del sistema de un smartphone

Los smartphones son básicamente pequeñas computadoras portátiles que se distinguen principalmente de las computadoras por su estructura:

* Sistemas más restringidos:  
  * Gestores de arranque no personalizables  
* Inclusión de coprocesadores de propósito especial  
  * Enclave seguro  
  * Banda base  
  * Procesamiento de imágenes  
  * Aceleración de IA

### Coprocesadores

Los coprocesadores son procesadores utilizados únicamente para un propósito concreto distinto del sistema. Los coprocesadores ayudan al procesador principal de la aplicación a gestionar determinadas tareas, por lo que deben comunicarse con él. Con frecuencia, los coprocesadores ejecutan su propio sistema operativo y aplicaciones integradas y no pueden ser controlados directamente por el usuario. Los coprocesadores suelen tener acceso privilegiado a los recursos del sistema y a los datos del usuario, por lo que a veces son objetivo de explotaciones más sofisticadas. Debido a su naturaleza restringida, es difícil auditar u obtener datos forenses de los coprocesadores. Para los fines de esta guía, usted sólo necesitará saber que:

* Los coprocesadores pueden ser explotados  
* Las explotaciones de coprocesadores son sofisticadas y poco comunes

Por lo tanto, el resto de esta guía se centrará en los softwares que se ejecutan en el procesador de aplicaciones.

### Sistema operativo

Los sistemas operativos de los smartphones difieren de los sistemas operativos de las computadoras en que implementan más controles y aislamiento entre los diferentes componentes del sistema y las aplicaciones, de modo que un componente comprometido no podría afectar fácilmente a todo el sistema.

![An illustration of the Android software stack. For an explanation of all its components, check out https://developer.android.com/guide/platform](https://developer.android.com/guide/platform/images/android-stack_2x.png)

Es posible que los atacantes exploten las vulnerabilidades del núcleo, sin embargo estas vulnerabilidades son bastante raras y, por lo general, se requieren técnicas sofisticadas para ser explotadas.

### Aplicaciones del sistema

Las aplicaciones del sistema pueden contener vulnerabilidades. Cuando son explotadas, pueden causar más daño que si se explotan aplicaciones de usuario, ya que suelen tener más privilegios para realizar cambios en el sistema subyacente. Un ejemplo común es el navegador integrado, que con frecuencia es objeto de explotaciones.

### Aplicaciones de usuario

Las aplicaciones de usuario son las menos privilegiadas. Sin embargo, si se les conceden los permisos, pueden acceder a información sensible del usuario, por lo que aún pueden causar daños considerables. Además, a veces pueden engañar o explotar el sistema subyacente para obtener más control.