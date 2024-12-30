# Extraire des données pour permettre une analyse plus approfondie

[pcqf](https://github.com/botherder/pcqf) est un outil qui permet de vider les informations de démarrage et les processus actuels, ainsi que la mémoire pour permettre une analyse plus approfondie. Il est très utile, par exemple, si vous n'avez pas le temps de tout vérifier sur l'ordinateur et que vous voulez vérifier plus tard s'il y a des éléments suspects.

Vous devez exécuter ce programme à partir d'une clé USB avec suffisamment d'espace de stockage, double-cliquez sur le fichier binaire et suivez les instructions.

![A screenshot of a window with a commandline program running therein. The program is PCQF and is asking the user if they would like to take a memory snapshot.](../.gitbook/assets/pcqf1.PNG)

À moins d'avoir une bonne raison de le faire, il est recommandé de ne pas prendre un instantané de mémoire, car il contient beaucoup d'informations privées (il peut contenir des mots de passe par exemple).

Une fois terminé, il crée un dossier nommé par l'ID d'acquisition (une séquence de nombres hexadécimaux) qui contient :

* Un fichier `profile.json` contenant des informations de base sur le système de l'ordinateur.  
* Un fichier `process_list.json` contenant une liste des processus en cours d'exécution.  
* Un fichier `autoruns.json` contenant une liste de tous les éléments persistants sur le système.  
* Un dossier `autoruns_bins/` contenant des copies des fichiers et exécutables marqués pour la persistance dans le fichier JSON précédent.  
* Un dossier `process_bins/` contenant des copies de processus en cours d'exécution.  
* Si vous le demandez, un dossier `memory/` contiendra un instantané de la mémoire physique ainsi que des métadonnées.