# Surveiller le trafic réseau

La surveillance du trafic réseau à partir d'un téléphone est l'une des meilleures façons d'identifier les activités malveillantes sans interagir avec le téléphone mobile, ce qui permet de contourner tout mécanisme que le logiciel malveillant peut avoir développé pour contourner la détection. Mais cela nécessite un outil externe pour enregistrer le trafic et une certaine connaissance du réseau pour identifier le trafic suspect.

## Limitations

De nombreuses applications mobiles et logiciels malveillants permettent désormais le « SSL Pinning », qui complique pour nous le déchiffrement de leur trafic. La seule façon de contourner le SSL Pinning est de rooter/jailbreak l'appareil et d'utiliser des outils tels que Frida. Cependant, dans le but de l'analyse, nous ne devons le faire que si nous soupçonnons fortement qu'un trafic malveillant est caché avec un chiffrement SSL.

## Surveiller les requêtes DNS

Au lieu de surveiller le trafic réseau complet, ce qui nécessite une configuration complexe, nous pouvons simplement surveiller les requêtes DNS. La plupart des systèmes d'exploitation permettent aux utilisateurs de configurer leurs propres serveurs DNS. Nous pouvons configurer notre propre serveur DNS pour enregistrer les requêtes et pointer le périphérique contrôlé vers notre serveur.

Nous pouvons déployer notre propre serveur DNS avec :

* Pi-hole  
* Adguard Home

Ou utiliser un serveur DNS basé sur le cloud comme NextDNS. L'inconvénient des serveurs DNS basés sur le cloud est que le fournisseur du cloud sera en mesure de voir les requêtes des appareils.

NextDNS est un fournisseur de DNS en cloud qui vous permet de configurer un serveur DNS sans compte. NextDNS fournit également des applications pour les principaux systèmes d'exploitation afin de configurer le périphérique pour utiliser votre serveur personnalisé. Pour commencer, accédez à [https://my.nextdns.io/start](https://my.nextdns.io/start), qui créera un *ID de configuration* temporaire, que vous devrez saisir dans l'application NextDNS. Ensuite, toutes les requêtes DNS que le téléphone effectuera seront enregistrées dans l'onglet *Logs*.

![Requêtes DNS enregistrées dans NextDNS](../.gitbook/assets/Screenshot_20220506_165152.png)

## Surveiller le trafic réseau complet à partir d'un routeur Wi-Fi

Une des façons de surveiller le trafic réseau à partir de votre téléphone consiste à enregistrer le trafic directement à partir d'un routeur Wi-Fi utilisé par votre téléphone mobile. Selon votre routeur Wi-Fi, il peut être possible d'enregistrer le trafic directement à partir de celui-ci (en utilisant un outil comme [tcpdump](https://www.tcpdump.org/)).

Si vous ne pouvez pas enregistrer le trafic en utilisant votre routeur Wi-Fi actuel, vous pouvez facilement créer un routeur à l'aide d'un [Raspberry Pi](https://www.raspberrypi.org/) et du logiciel [RaspAP](https://raspap.com/) (vous trouverez [ici](https://howtoraspberrypi.com/create-a-wi-fi-hotspot-in-less-than-10-minutes-with-pi-raspberry/) un tutoriel sur la façon de l'installer).

Une fois votre routeur installé, vous pouvez vous y connecter en utilisant ssh et commencer à capturer le trafic en utilisant [tcpdump](https://www.tcpdump.org/). Ensuite, connectez le téléphone mobile que vous souhaitez tester à ce réseau Wi-Fi et enregistrez le trafic pendant 30 minutes à une heure.

Au terme de la capture, téléchargez le fichier pcap sur votre ordinateur et passez en revue le trafic réseau à l'aide de [WireShark](https://www.wireshark.org/). Vérifiez particulièrement les éléments suivants :

* Connexions aux adresses IP sans requêtes DNS  
* Connexions à des domaines qui ne sont pas répertoriés dans les listes [Alexa Top](https://www.alexa.com/siteinfo)  
* Connexions à des ports autres que 80 et 443

Si vous trouvez un trafic suspect vers une adresse IP ou un domaine, vous pouvez utiliser les outils suivants pour obtenir plus d'informations à ce sujet :

* [CentralOps](https://centralops.net/co/) permet de voir le propriétaire d'une IP ou le whois d'un domaine (ne nécessite pas de compte)  
* [RiskIQ](https://community.riskiq.com/home) permet de voir l'historique DNS et offre beaucoup d'indications sur les adresses IP et les domaines malveillants (nécessite un compte, nombre limité de requêtes avec un compte gratuit)  
* [AlienVault OTX](https://otx.alienvault.com/) est une grande base de données d'indicateurs malveillants, vous pouvez y rechercher l'IP ou le domaine pour voir s'ils sont associés à des activités malveillantes connues (nécessite un compte, gratuit)

Pour configurer un environnement d'analyse du trafic, reportez-vous à [ce guide](https://mobile-security.gitbook.io/mobile-security-testing-guide/general-mobile-app-testing-guide/0x04f-testing-network-communication).

## Surveiller le trafic réseau avec des applications installées sur l'appareil

Ces applications fonctionnent généralement en créant un serveur VPN sur l'appareil et en configurant le système pour utiliser le serveur VPN.

* [NetGuard](https://netguard.me/) : pour Android, open source  
* [Lockdown](https://lockdownprivacy.com/firewall) : pour iOS, open source  
* [Blokada](https://apps.apple.com/us/app/blokada/id1508341781) : pour iOS, propriétaire

### Comment rechercher un trafic suspect ?

* Les comportements suivants sont plus suspects :  
* Les applications ou connexions qui génèrent beaucoup de trafic de données.  
* Les applications ou connexions qui envoient plus de données qu'elles n'en reçoivent.  
* Les connexions qui se produisent périodiquement.  
* Les noms de domaine étranges. Si vous en rencontrez, il suffit de les rechercher sur Google ou VirusTotal.  
* Les ports inhabituels.  
* L'envoi ou la réception de données lorsque l'utilisateur n'utilise pas le téléphone. (P. ex., lorsqu'il dort.)

## Utiliser un VPN d'urgence de Civil Sphere

Le projet [Civil Spher](https://www.civilsphereproject.org/what-we-do)e, une organisation née au sein du [Stratosphere Research Laboratory](https://www.stratosphereips.org/) à l'Université technique tchèque de Prague, offre un service [VPN d'urgence](https://www.civilsphereproject.org/emergency-vpn) pour identifier les appareils compromis.

Le service VPN d'urgence permet d'analyser et de repérer les trafics suspects à partir des appareils mobiles utilisés par les journalistes et les ONG. [Sur demande](https://www.civilsphereproject.org/get-started), l'équipe de Civil Sphere vous enverra des identifiants pour vous connecter à leur VPN avec votre téléphone. Via leur VPN, ils enregistreront l'ensemble du trafic sortant de votre téléphone pendant un maximum de trois jours. À la fin de l'enregistrement, ils analyseront le trafic en utilisant des techniques automatisées et manuelles afin d'identifier les activités malveillantes ou les problèmes de sécurité dans les applications exécutées sur votre téléphone, et ils vous enverront le résultat de cette analyse par courrier électronique.

![A graphic of Emergency VPN, giving users a step-by-step guide on how it works. It asks them to 1. download the application 2. request a VPN account 3. import the new profile into your OpenVPN application 4. install and authorize the profile in your OpenVPN application 5. connect to the VPN for up to 3 days to give us enough data to analyze 6. receive a report from the team about any security threats or concerns you should be aware of](../.gitbook/assets/emergencyvpn.png)

Une des limitations de cette technique est que vous devez fournir le trafic réseau de votre téléphone mobile à une organisation tierce (Civil Sphere), vous [pouvez les contacter](https://www.civilsphereproject.org/get-started) pour avoir plus d'informations sur la façon dont ces données sont stockées et utilisées. L'avantage de cette solution est que vous pouvez compter sur eux pour effectuer l'analyse du le trafic réseau, ce qui peut être un processus complexe et long.

Consultez [leur site Web](https://www.civilsphereproject.org/emergency-vpn) pour obtenir plus d'informations.