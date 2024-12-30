# Examiner les connexions réseau

Les logiciels espions devront à un moment ou à un autre transmettre les données recueillies (telles que les captures d'écran, les mots de passe, les frappes au clavier, etc.) à un emplacement distant, le [serveur de commande et de contrôle](https://www.crowdstrike.com/cybersecurity-101/cyberattacks/command-and-control/). Bien qu'il ne soit pas possible de prédire quand une telle transmission se produira, il est possible que certains logiciels espions établissent une liaison permanente avec le serveur ou qu'ils se connectent assez souvent pour vous permettre de les détecter.

Pour vérifier les connexions en cours, vous pouvez par exemple enregistrer tout le trafic réseau à l'aide de [Wireshark](https://www.wireshark.org/) et ensuite examiner les résultats stockés. Cependant, une approche plus intéressante consiste à utiliser des outils qui non seulement surveillent l'activité du réseau, mais qui peuvent également les lier aux processus en cours. En général, vous devriez rechercher des processus inhabituels se connectant à des adresses IP suspectes.

Un outil populaire permettant cette fonction est [TCPView](https://technet.microsoft.com/en-us/sysinternals/tcpview.aspx), également issu de la suite Sysinternals de Microsoft.

![A screenshot of Sysinternals TCPView. It shows several columns, including Process, PID, Protocol (which displays TCP, UDP, TCPV6, and UDPV6), local address, local port, remote address, and remote port.](../.gitbook/assets/tcpview.png)

L'outil est assez simple : il énumère toutes les connexions réseau établies et fournit des informations sur le processus d'origine et la destination. Vous serez probablement surpris(e) d'observer la quantité de connexions réseau actives, même avec des systèmes apparemment inactifs. Le plus souvent, vous verrez une activité réseau à partir de processus en arrière-plan, par exemple, pour les services Microsoft, Google Chrome, Adobe Reader, Skype, etc.

CrowdInspect, que nous avons présenté dans la section précédente sur [l'examen des processus en cours d'exécution](https://pellaeon.gitbook.io/mobile-forensics/windows/processes), est un autre outil que nous pouvons utiliser pour observer les connexions réseau actives. Les informations fournies par CrowdInspect sont très similaires à celles fournies par TCPView.

![A screenshot of CrowdInspect, with a process called iexplore.exe selected, which has a red dot in the column marked Inject. That process has established a TCP connection. It is trying to connect to remote IP 216.6.0.28](../.gitbook/assets/crowdinspect_injection.png)

Par exemple, dans la capture d'écran ci-dessus, nous pouvons voir un processus `iexplore.exe` en cours d'exécution qui non seulement a été signalé comme injecté, mais semble tenter activement de se connecter à l'adresse IP distante `216.6.0.28`. Vu qu'il n'y a aucun navigateur Internet visible sur le système, il est très suspect de voir des connexions réseau actives à partir de ce processus. TCPView apparaîtrait comme suit sur le même système infecté :

![A screenshot of TCP view, with a process called iexplore.exe selected. That process is trying to connect to remote IP 216.6.0.28, the same IP as in the above screenshot.](../.gitbook/assets/tcpview_infected.png)

(Remarque : ces outils affichent les tentatives de connexion à des emplacements distants, même si l'ordinateur est actuellement déconnecté d'Internet).

Lorsque vous avez des doutes sur une connexion active, vous pouvez (de préférence à partir d'un ordinateur séparé) rechercher l'adresse IP et essayer de déterminer à qui elle appartient et si elle est connue comme étant inoffensive ou malveillante, en utilisant par exemple des outils en ligne comme [Central Ops](https://centralops.net/co/) ou [ipinfo](https://ipinfo.io/). Par exemple, une simple recherche WHOIS pour cette adresse IP renvoie :

```
NetRange:       216.6.0.0 \- 216.6.1.255  
CIDR:           216.6.0.0/23  
NetName:        SYRIAN-5  
NetHandle:      NET-216-6-0-0-2  
Parent:         TATAC-ARIN-9 (NET-216-6-0-0-1)  
NetType:        Reassigned  
OriginAS:  
Organization:   STE (Syrian Telecommunications Establishment) (SSTE)  
RegDate:        2005-07-21  
Updated:        2005-07-21  
Comment:        Fax-no-963 11 3739765  
Ref:            https://rdap.arin.net/registry/ip/216.6.0.0

OrgName:        STE (Syrian Telecommunications Establishment)  
OrgId:          SSTE  
Address:        Fayz Mansour St  
Address:        STE Building  
City:           Damascus  
StateProv:  
PostalCode:  
Country:        SY  
RegDate:        2005-07-21  
Updated:        2011-09-24  
Ref:            https://rdap.arin.net/registry/entity/SSTE
```

Cela suggère que le processus injecté `iexplore.exe` est très suspect et tente de se connecter à une adresse IP située en Syrie. En effet, pour la démonstration, nous avons utilisé une ancienne copie de DarkComet RAT qui a été utilisée en Syrie en 2011\.

Même une simple recherche de l'adresse IP sur votre moteur de recherche préféré pourrait révéler des informations utiles. En outre, vous pouvez envisager d'utiliser des services de recherche de menaces tels que [RiskIQ](https://community.riskiq.com/) ou [ThreatMiner](https://www.threatminer.org/) pour voir s'ils ont des informations sur les adresses IP ou les noms de domaine que vous rencontrez.