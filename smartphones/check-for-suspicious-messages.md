# Vérifier la présence de messages suspects

Les messages contenant des liens sont un vecteur d'attaque courant.

![A screenshot of an online translation service, showing a message in Arabic and a translation into English. The translation says 'Turkey asks the Egyptian opposition channels to stop criticizing Egypt, and Cairo comments on the move...'](https://citizenlab.ca/wp-content/uploads/2021/12/Fig-7.png)

*Un message contenant un lien qui infecte la cible avec le logiciel malveillant Cytrox Predator. Source : Citizen Lab*

Les liens peuvent être envoyés à partir de n'importe quelle application de messagerie instantanée ou SMS. Il existe quelques types de liens malveillants :

| Cible du lien | Objectif du cybercriminel | Complexité | Atténuation |
| :---- | :---- | :---- | :---- |
| Site d'hameçonnage, comme une page Web qui ressemble à la page de connexion de Google | Inciter l'utilisateur à saisir des données personnelles ou des mots de passe | Faible | Vérifier les noms de domaine et les certificats SSL des pages Web |
| Téléchargement d'application | Convaincre l'utilisateur de télécharger et d'installer l'application | Faible | Ne pas installer les applications en dehors des magasins d'applications |
| Page Web contenant des exploitations de failles Web, tels que XSS (Cross Site Scripting) | Voler les cookies de session en ligne ou exploiter la session actuellement ouverte | Moyenne | Ne pas cliquer sur les liens envoyés par des inconnus |
| Page Web contenant une exploitation de faille de navigateur | Exploiter la vulnérabilité du navigateur ou de l'application | Élevée | Ne pas cliquer sur les liens envoyés par des inconnus |

### Enregistrement des messages

1. Copiez le message entier, y compris le lien dans le presse-papiers  
2. Vous pouvez également enregistrer une capture d'écran contenant l'entièreté du texte  
3. Si vous le pouvez, archivez le lien en utilisant la [Wayback Machine](https://web.archive.org/)

### Vérification des liens
Effectuez simplement une recherche du lien sur Google, ou collez le lien dans des sites comme VirusTotal.