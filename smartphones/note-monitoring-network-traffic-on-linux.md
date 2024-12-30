---
description: >-
  Cette page sert de remarque pour la configuration de la surveillance du trafic réseau sur Linux.
---

# Remarque : surveillance du trafic réseau sur Linux

## Configuration du partage Wi-Fi

Utiliser KDE

1. Branchez un adaptateur Wi-Fi qui prend en charge le mode AP.  
2. Cliquez avec le bouton droit de la souris sur l'icône Réseau dans la barre des tâches. Cliquez sur *Configurer les connexions réseau*.  
3. Cliquez sur Ajouter, sélectionnez *Wi-Fi (partagé)*.  
4. Sous l'onglet Wi-Fi, configurez :  
  1. SSID : ce que vous voulez  
  2. Périphérique : sélectionnez l'adaptateur que vous venez de brancher.  
  3. Sécurité sans fil : configurez un mot de passe personnel WPA2  
  4. IPv4 : *Méthode : partager avec les autres ordinateurs*  
5. Choisissez un nom de connexion (le nom du profil réseau)  
6. Enregistrez  
7. Cliquez avec le bouton gauche de la souris sur l'icône du réseau, puis cliquez sur Connecter sur le nom de connexion que vous venez de créer.  
8. L'AP devrait être démarré et vous devriez le voir à partir d'autres appareils avec le SSID que vous venez de configurer.

## Configuration d'une redirection vers un proxy d'interception

Pour les analyses mobiles, il n'est généralement pas nécessaire d'intercepter le trafic SSL, car pour y parvenir, il faudrait normalement configurer une autorité de certification (CA) SSL autosignée pour l'appareil mobile et la plupart des applications ne font pas confiance aux CA SSL importés par l'utilisateur. Pour que les applications fassent confiance à l'autorité de certification autosignée, il faudrait rooter le périphérique Android, ce qui n'est pas recommandé, car lors de la conduite d'une analyse approfondie, le périphérique en question ne devrait pas être modifié.
