# Architecture des systèmes de smartphones

Les smartphones sont essentiellement de petits ordinateurs portables, la principale différence architecturale par rapport aux ordinateurs étant :

* Systèmes plus verrouillés :  
  * Chargeurs d'amorçage non personnalisables  
* Ajout de coprocesseurs à but spécialisé  
  * Enclave sécurisée  
  * Bande de base  
  * Traitement des images  
  * Accélération IA

### Coprocesseurs

Les coprocesseurs sont des processeurs utilisés uniquement à une fin particulière autre que l'exploitation du système. Les coprocesseurs aident le processeur principal à gérer certaines tâches et doivent donc communiquer avec lui. Les coprocesseurs utilisent souvent leur propre système d'exploitation et application intégrés, et ne peuvent pas être directement contrôlés par l'utilisateur. Les coprocesseurs ont souvent un accès privilégié aux ressources du système et aux données des utilisateurs, de sorte qu'ils sont parfois la cible d'exploitations plus sophistiquées. En raison de leur nature verrouillée, il est difficile d'effectuer des audits ou d'obtenir des données d'analyse à partir des coprocesseurs. Pour les besoins de ce guide, vous devez seulement savoir que :

* Les coprocesseurs peuvent présenter des failles qui peuvent être exploitées  
* Les exploitations de failles des coprocesseurs sont sophistiquées et peu communes

Le reste de ce guide se concentrera donc sur les logiciels fonctionnant sur le processeur d'application.

### Système d'exploitation

Les systèmes d'exploitation des smartphones diffèrent des systèmes d'exploitation des ordinateurs en ce sens qu'ils mettent en œuvre davantage de contrôles et d'isolement entre les différents composants du système et les applications, de sorte qu'un composant compromis ne pourrait pas facilement affecter l'ensemble du système.

![An illustration of the Android software stack. For an explanation of all its components, check out https://developer.android.com/guide/platform](https://developer.android.com/guide/platform/images/android-stack_2x.png)

Il est possible pour les cybercriminels d'exploiter des vulnérabilités du noyau, mais ces vulnérabilités sont assez rares et nécessitent généralement des techniques sophistiquées pour être exploitées.

### Applications du système

Les applications du système peuvent contenir des vulnérabilités. Une fois exploitées, elles peuvent causer plus de dommages que l'exploitation des applications de l'utilisateur, car elles ont généralement plus de privilèges pour apporter des modifications au système sous-jacent. Un exemple courant est le navigateur intégré, qui est souvent exploité.

### Applications de l'utilisateur

Les applications de l'utilisateur ont moins de privilèges. Toutefois, si les autorisations sont accordées, elles peuvent accéder à des informations personnelles de l'utilisateur, ce qui peut toujours causer beaucoup de tort. Parfois, elles peuvent également tromper ou exploiter le système sous-jacent pour obtenir plus de contrôle.