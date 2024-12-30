# Examiner les processus en cours

Un ordinateur infecté par des logiciels espions devrait avoir des processus malveillants en cours d'exécution à tout moment, qui surveillent le système et collectent les données à transmettre au serveur [de commande et de contrôle](https://securitywithoutborders.org/resources/digital-security-glossary.html#cnc) des cybercriminels. Par conséquent, une autre étape nécessaire pour le triage d'un ordinateur Windows suspect consiste à extraire la liste des processus en cours et de vérifier si l'un d'eux présente des caractéristiques suspectes.

Il existe quelques outils qui permettent de le faire.

**Avertissement** : certains logiciels espions plus sophistiqués pourraient être capables d'échapper à cet outil en cachant leurs entrées de l'arborescence, ou en mettant fin immédiatement à l'exécution s'ils observent le lancement de l'un de ces outils. Dans ce guide, nous présentons une première méthodologie et des suggestions pour effectuer une évaluation initiale. Une liste de processus propres n'est pas nécessairement une garantie d'un système propre.

Avant de procéder à cette vérification, il est conseillé de fermer toutes les applications en cours d'exécution visibles, afin de réduire au strict minimum les résultats des outils que vous exécuterez.

## Process Explorer

[Process Explorer](https://technet.microsoft.com/en-us/sysinternals/processexplorer.aspx) est un autre outil de la [suite Sysinternals](https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite) de Microsoft qui liste tous les processus en cours d'exécution sur le système dans une arborescence :

![A screenshot of the Sysinternals Process Explorer. It diplays a list of processes. Some are not highlighted at all, whereas others are highlighted in red, with their sub-processes highlighted in cyan blue, dark grey, light purple, or not lighted at all. The explorer also shows how much CPU each process utilizes, how many private bytes it has, its working set, PID, description, company name, and VirusTotal score.](../.gitbook/assets/procexp.png)

La méthodologie de vérification des processus en cours d'exécution suspects est quelque peu similaire à celle décrite dans la section [Examen des programmes lancés au démarrage](https://pellaeon.gitbook.io/mobile-forensics/windows/autoruns).

### 1. Vérifier les signatures d'images

Comme pour Autoruns, Process Explorer permet également de vérifier les signatures des applications en cours d'exécution en cliquant sur *Options* et en activant « *Vérifier les signatures d'image* ». Les mêmes considérations et avertissements que nous avons décrits dans la [section précédente](https://pellaeon.gitbook.io/mobile-forensics/windows/autoruns) s'appliquent ici aussi. Encore plus avec les processus en cours, le fait qu'une demande de processus soit signée ne signifie pas nécessairement qu'elle est sûre. Les logiciels malveillants utilisent souvent des techniques telles que le [Process Hollowing](https://attack.mitre.org/techniques/T1093/) ou le [DLL Sideloading](https://attack.mitre.org/techniques/T1073/) pour exécuter du code à partir du contexte d'une application légitime et signée afin de contrecarrer la détection.

![Another screenshot of Process Explorer. Somebody opened the Options menu and is selecting the Verify Image Signatures menu item therein.](../.gitbook/assets/procexp2.png)

### 2. Rechercher des scripts

Les cybercriminels utilisent souvent les capacités de script de Microsoft Windows, comme PowerShell et Windows Script Host, en raison de leur flexibilité et de leur capacité à échapper à la détection. Ces moteurs de script sont couramment utilisés par les entreprises pour automatiser les configurations des systèmes internes. Il est moins courant de voir des applications grand public les utiliser, donc tout processus en cours qui y est associé devrait être inspecté plus en détail.

Ces processus sont normalement appelés `powershell.exe` ou `wscript.exe`.

Voici un exemple de Process Explorer affichant un script PowerShell manifestement malveillant exécuté sur le système :

![A screenshot of Process Explorer. It is displaying a tooltip over powershell.exe and its sub-process conhost.exe. The tooltip displays obfuscated PowerShell code, along with a URL that has been blanked out by its author.](../.gitbook/assets/procexp_powershell.png)

Le survol du nom du processus avec le curseur de la souris montre les arguments de la ligne de commande où nous pouvons clairement voir que le script tente de télécharger et d'exécuter du code supplémentaire. Notez également l'utilisation de la casse alternée, comme « doWnLoAdfile » : il s'agit d'une astuce très basique utilisée par les cybercriminels pour échapper aux schémas de détection de base des logiciels de sécurité.

### 3. Rechercher des DLL en cours d'exécution

Les logiciels malveillants se présentent parfois sous la forme d'une [bibliothèque de liens dynamiques (DLL)](https://support.microsoft.com/en-us/help/815065/what-is-a-dll) qui, à l'opposé d'une application autonome (autrement dit, un fichier `.exe`), doit être lancée par un chargeur. Windows fournit quelques programmes pour lancer les DLL, communément `regsvr32.exe` et `rundll32.exe`, qui sont signés par Microsoft.

Recherchez l'un de ces processus en cours d'exécution et essayez de déterminer quel fichier DLL il exécute. Par exemple, dans la capture d'écran ci-dessous, nous pouvons voir un système Windows infecté exécutant un fichier DLL malveillant situé dans `C:\Users\<Username>\AppData\` à l'aide de `regsvr32.exe`.

![A screenshot of Process Explorer. It is displaying a tooltip over regsvr32.exe. The tooltip displays a shell command to load a DLL.](../.gitbook/assets/procexp_regsvr.png)

### 4. Rechercher les processus d'applications qui devraient être visibles

Parmi les nombreuses techniques souvent utilisées par les cybercriminels, il y a, par exemple, le [Process Hollowing](https://attack.mitre.org/techniques/T1093/). Le Processus Hollowing consiste à lancer une application légitime (comme Internet Explorer ou Google Chrome), à vider sa mémoire et à la remplacer par un code malveillant, qui sera ensuite exécuté. Cela est normalement fait pour cacher le code malveillant, faire en sorte qu'il apparaisse comme une application légitime (qui ne serait alors qu'une coquille vide), contourner le pare-feu des applications et peut-être contourner d'autres produits de sécurité.

Par exemple, si vous voyez un processus `iexplore.exe` en cours d'exécution, alors qu'il n'y a visiblement aucune fenêtre ouverte d'Internet Explorer, vous devriez considérer cela comme un signal inquiétant.

![A screenshot of Process Explorer. A process that is called explore.exe and which has the Internet Explorer logo is selected.](../.gitbook/assets/procexp_iexplore.png)


### Facultatif : 5. Recherche de programmes sur VirusTotal

Comme pour la section [Autoruns](https://pellaeon.gitbook.io/mobile-forensics/windows/autoruns), l'explorateur de processus propose également de rechercher les processus en cours sur VirusTotal en recherchant le hachage cryptographique des fichiers exécutables respectifs. Ceci peut être activé en cliquant sur *Options \> VirusTotal.com* et en activant *Vérifier sur VirusTotal.com*.

![Another screenshot of Process Explorer. Somebody opened the Options menu, floated over the VirusTotal.com submenu, and is selecting the 'Check VirusTotal.com' menu item.](../.gitbook/assets/procexp3.png)

**Remarque :** les mêmes considérations et mises en garde que celles expliquées dans la [section précédente](https://pellaeon.gitbook.io/mobile-forensics/windows/autoruns) s'appliquent ici aussi. Assurez-vous de les lire avant de continuer.

## CrowdInspect

[CrowdInspect](https://www.crowdstrike.com/resources/community-tools/crowdinspect-tool/) est un outil produit par la société de sécurité CrowdStrike située aux États-Unis. CrowdInspect est très similaire à Process Explorer, mais présente quelques avantages. Premièrement, les informations présentées tendent à être plus compactes. Deuxièmement, il ne montre pas seulement les processus actuellement actifs, mais il peut aussi montrer des processus qui ont été arrêtés depuis leur lancement (que vous auriez peut-être manqué s'ils ont été exécutés trop rapidement). Enfin, il effectue un certain nombre de vérifications que Process Explorer ne prend pas en charge actuellement.

![A screenshot of CrowdInspect. It has several options highlighted, including TCP, UDP, Localhost, System, and Full Path. It displays a host of data about processes, including their names, PIDs, types of network connections, local and remote IP addresses.](../.gitbook/assets/crowdinspect.png)

### Vérifier la présence d'injections de processus

La fonctionnalité la plus intéressante proposée par CrowdInspect est probablement la capacité d'identifier les [processus injectés](https://attack.mitre.org/techniques/T1055/). L'injection de processus est une catégorie de techniques dont l'objectif est d'exécuter du code malveillant dans le contexte d'une application distincte, généralement légitime (comme `explorer.exe`). L'injection de processus est souvent utilisée par les auteurs de logiciels malveillants afin d'obtenir des privilèges supplémentaires sur le système ou, par exemple, pour échapper à la détection.

CrowdInspect vous avertit de tout processus injecté en affichant un point rouge visible sous la colonne « *Inject* ». Les processus injectés sont généralement un très bon indicateur qu'il pourrait y avoir une infection active sur l'ordinateur testé.

![A screenshot of CrowdInspect, with a process called iexplore.exe selected, which has a red dot in the column marked Inject.](../.gitbook/assets/crowdinspect_injection.png)