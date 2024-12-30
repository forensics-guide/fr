# Examen des programmes lancés au démarrage

Normalement, les logiciels espions doivent trouver un moyen d'être exécutés au démarrage de l'ordinateur. La vérification des applications démarrant automatiquement est donc l'un des premiers contrôles à effectuer lors de la recherche d'infections potentielles. Les ordinateurs Windows ont différentes façons d'activer le lancement automatique, et les logiciels espions utilisent souvent des astuces pour paraître légitimes et/ou éviter les méthodes les plus courantes.

[Sysinternals Autoruns](https://technet.microsoft.com/en-ca/sysinternals/bb963902.aspx) est un outil qui permet d'énumérer de manière exhaustive les programmes lancés au démarrage. Si possible, vous devriez exécuter ce programme en tant qu'administrateur :

![A screenshot of Windows Sysinternals Autoruns, which allows you to select and study autorun entries. Each entry in this list is sorted by HKLM or HKCU. The program has many different tabs open, with a tab called 'everything' currently in focus.](../.gitbook/assets/autoruns.png)

Tous les résultats seront affichés par défaut dans l'onglet principal. En cliquant sur les autres onglets disponibles, vous filtrerez les résultats par type de lancement automatique. Les plus intéressantes seraient généralement `Ouverture de session`, `Tâches planifiées` et `Services`.

## Recherche de motifs suspects

Autoruns ne détermine pas automatiquement pour vous quels fichiers sont malveillants et lesquels ne le sont pas. Comme pour le reste de cette méthodologie, il est nécessaire que vous soyez suffisamment familiarisé(e) avec ses résultats pour repérer rapidement les anomalies ou les entrées que vous ne reconnaissez pas. Cependant, Autoruns peut fournir des indications utiles.

Parfois, Autoruns peut marquer une ligne particulière avec un fond rouge. Ces éléments pourraient justifier une inspection plus approfondie, car il pourrait s'agir d'un signe d'entrée inhabituelle. Les entrées marquées d'un fond jaune font plutôt référence à des fichiers qui n'existent plus sur l'ordinateur. Ces entrées sont donc brisées.

Voici quelques suggestions de modèles à surveiller.

### 1. Vérifier les signatures d'images

Dans les versions modernes de Windows, les applications légitimes doivent généralement être « signées » avec un certificat de développeur. Ces certificats permettent de vérifier le producteur d'un programme particulier (comme Microsoft, Google, Adobe ou autre). Les applications qui ne sont pas signées normalement sont plus contrôlées et examinées par les mécanismes de sécurité de Windows (comme son antivirus intégré, Windows Defender). Une première vérification utile consiste à vérifier si toutes les applications lancées automatiquement sont bien signées, et cela peut être fait en cliquant sur *Options \> Options d'analyse* et en activant *Vérifier les signatures de code*.

![A dialog box that says 'Autoruns scan options'. Four checkboxes are present: 'Scan only per-user locations,' 'Verify code signatures,' 'Check VirusTotal.com,' and, nested under the VirusTotal one, 'Submit Unknown Images'. Only the 'Verify code signatures' check box is selected.](../.gitbook/assets/autoruns5.png)

Cela relancera l'analyse d'Autoruns et ajoutera une nouvelle colonne appelée « Éditeur ». Les applications correctement signées seront marquées comme « (Vérifiée) » :

![A very similar screenshot of Windows Sysinternals Autoruns as before, except that the sorting menu has one more option, called 'Publisher'. Some applications have a label at the end of their name which says '(Verified)'](../.gitbook/assets/autoruns4.png)

**Attention** : toutes les entrées vérifiées du démarrage automatique ne sont pas nécessairement sûres. Parfois, les cybercriminels utilisent délibérément des applications vérifiées légitimes pour paraître moins suspectes, et les utilisent comme lanceurs pour charger et exécuter du code malveillant. Cela se fait parfois en utilisant, par exemple, Microsoft `rundll32.exe` ou d'autres applications affectées par ce qu'on appelle le DLL [Sideloading](https://attack.mitre.org/techniques/T1073/).

### 2. Vérifier le nom de l'entrée du démarrage automatique

L'entrée de démarrage automatique affiche le nom qui a été donné à l'application par ses développeurs. Ces informations peuvent être falsifiées, mais parfois les cybercriminels sont assez paresseux pour soit imiter des noms légitimes (p. ex., « Micorsoft Ofice » ou « Crhome ») soit simplement laisser des caractères et des numéros aléatoires.

### 3. Vérifier la description du programme

De même, bien qu'il ne s'agisse pas d'un indicateur fiable, les applications légitimes devraient généralement avoir une description du programme visible.

### 4. Vérifier le chemin d'accès de l'image

Windows fournit des dossiers standard où les applications légitimes sont normalement installées et exécutées. Les services du système d'exploitation lui-même sont normalement situés dans `C:\Windows\`, tandis que les applications installées par l'utilisateur sont généralement situées dans `C:\Program Files\` ou `C:\Program Files (x86)\`. Vu que l'installation de programmes dans ces dossiers devrait nécessiter une certaine confirmation de l'utilisateur, les cybercriminels placent souvent leurs fichiers malveillants dans des dossiers moins typiques, tels que `C:\Users\<Username>\AppData\` ou d'autres sous-répertoires dans `C:\Users\`.

Exemple d'entrée suspecte :

* Le [logiciel espion KeyBoy](https://citizenlab.ca/2016/11/parliament-keyboy/) crée une clé de registre dans `HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\shell` avec la valeur `explorer.exe,C:\Windows\system32\rundll32.exe "%LOCALAPPDATA%\cfs.dal" cfsUpdate`  
* Un logiciel malveillant particulier utilisé en Asie centrale repose sur l'utilisation de VBScripts, qui sont mis en évidence par Autoruns avec un fond rouge, car ils se font passer pour des logiciels Adobe et Google. Ces résultats justifieraient certainement une inspection plus approfondie. En outre, les scripts sont situés dans `C:\Users\<Username>\AppData\`:

![Another screenshot of Windows Sysinternals Autoruns. We are in the 'Everything' tab and several entries are selected. Under 'Task Scheduler', we see three entries highlighted in red. They are labeled 'Adobe Flash Player File,' 'Adobe Flash Player Key,' and 'GoogleUpdateTaskMachineKernel'](../.gitbook/assets/autoruns_script.png)

### Facultatif : 5. Recherche de programmes sur VirusTotal

En option, Autoruns permet de vérifier les fichiers binaires avec [VirusTotal](https://www.virustotal.com/gui/home/upload), ce qui contribue à identifier immédiatement tout programme malveillant bien connu et largement détecté par le logiciel antivirus (vous pouvez en apprendre davantage à ce sujet dans la section ci-dessous). Pour activer cette vérification, accédez à *Options \> Options d'analyse* et activez « *Consulter VirusTotal.com* ». Faites attention de ne pas activer « Soumettre des fichiers inconnus », car cela ferait en sorte qu'Autoruns envoie automatiquement les fichiers locaux vers le service, plutôt que de simplement chercher leurs hachages cryptographiques. VirusTotal est une société, maintenant détenue par Alphabet (la maison mère de Google), qui fournit un accès commercial à ses données aux chercheurs en sécurité et aux clients du monde entier. Les personnes ayant accès aux services commerciaux de VirusTotal peuvent consulter et télécharger n'importe quel fichier envoyé. Par conséquent, vous devriez éviter de soumettre par inadvertance des fichiers qui pourraient être confidentiels.

![A dialog box that says 'Autoruns scan options'. Four checkboxes are present: 'Scan only per-user locations,' 'Verify code signatures,' 'Check VirusTotal.com,' and, nested under the VirusTotal one, 'Submit Unknown Images'. Only the 'CheckVirusTotal.com' check box is selected.](../.gitbook/assets/autoruns2.png)

Une fois l'option VirusTotal activée, il faudra un certain temps pour que les résultats apparaissent. Vous devriez voir une colonne VirusTotal affichant les résultats de l'analyse antivirus. Les résultats apparaissent sous la forme d'une valeur *X/Y*, où *X* est le nombre de détections positives et Y est la quantité totale de logiciels antivirus avec lesquels le fichier a été analysé.

![A very similar screenshot of Windows Sysinternals Autoruns as the first one, except that the sorting menu has one more option, called 'VirusTotal'. Each entry has a VirusTotal score next to it.](../.gitbook/assets/autoruns3.png)

Si aucun résultat n'est affiché, cela signifie que ce programme particulier n'a pas été téléversé précédemment sur VirusTotal et qu'il peut justifier une inspection supplémentaire. Parfois, vous verrez des applications avec un faible nombre de détection (1 ou 2) : il s'agit souvent de fausses alertes. Les résultats de VirusTotal montrant un nombre de détection plus élevé (par exemple, 5 et plus) sont généralement un signe fiable que cette application particulière est malveillante. En cliquant sur le lien du *X/Y*, vous ouvrirez le navigateur sur l'analyse VirusTotal, où vous pourrez voir plus de détails, tels que les identifiants de logiciels malveillants utilisés par le logiciel antivirus pris en charge.

**Veuillez noter** : [tel que mentionné précédemment](https://github.com/pellaeon/guide-to-quick-forensics/blob/master/windows/safety.md), dans des circonstances normales, vous devriez éviter de connecter l'ordinateur testé à Internet. Sans connexion à Internet, vous ne pouvez pas effectuer de vérification immédiate avec VirusTotal. Il est toutefois possible d'enregistrer les résultats de l'analyse en cliquant sur *Fichier \> Enregistrer*... et d'ouvrir ultérieurement les résultats à partir d'un ordinateur séparé avec une connexion à Internet.