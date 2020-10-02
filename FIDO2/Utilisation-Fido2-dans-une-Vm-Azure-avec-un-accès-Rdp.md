

Microsoft à introduit l'utilisation de clé Fido2 dans Azure Active Directory encore en preview. Cependant il est aussi primordial d'utiliser la double authentification sur l'ensemble de nos accès.

Nous avons bien sûr la possibilité d'utiliser Microsoft Authentificator cependant si nous intégrons la technologie Fido2 il est intéressant aussi d'utiliser les Clés dans l'ensemble des accès.

Pour travailler sur la problématique le constructeur #Feitian m'a laissé à disposition des clés BioPass FIDO2.

Sans paramétrage spécifique coté client et sur Azure les clés Fido2 étaient inexistantes sur le client RDP.

# Premier axe de résolution : Ingénieur Microsoft

J'ai contacté le support Microsoft qui m'a indiqué qu'il y avait une limitation de conception du produit qui à l'heure actuelle ne peut être modifiée.

# Deuxième axe : logiciel de partage Usb pour RDP

Les logiciels détectent bien les clés FIDO2 cependant elle restaient inexistantes sur les vm Azure.

# Troisième axe : Utilisation de RemoteFX

La solution fonctionne parfaitement cependant elle demande du paramétrage coté client et serveur. Je vous détaille l'installation pour la mise en place de la solution.

Vm Azure :

Windows serveur 2016/2019

Windows 10

Dans gpedit modifier la règle suivante :

Computer Configuration\Administrative Templates\Windows Components\Remote Desktop Services\Remote Desktop Session Host\Device and Resource Redirection\Do not allow supported Plug and Play device redirection

Disabled

Lancer Cmd et réaliser un gpupdate /force

Pc Client :

Lancer regedit : Ordinateur\HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services\Client

    Ajouter la valeur suivante Dword nom : fUsbRedirectionEnableMode - Valeur 2

    Ajouter les valeurs GUID de la clé FIDO dans le répertoire de registre :

Ordinateur\HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services\Client\UsbSelectDeviceByInterfaces

Ajouter nouvelle valeur de chaine avec GUID de la clé, pour pouvoir le trouver simplement avec une clé Feitian vous pouvez utiliser l’outil : FidoBasicInfoMonitor disponible sur le site du constructeur.

Il faudra cependant convertir la clé ligne 3 en : {77010bd7-212a-4fc9-b236-d2ca5e9d4084}

Après avoir ajouté seulement la clé Fido2 le menu Usb RemoteFx restait indisponible lors d’une connexion en RDP.

Pour pouvoir faire apparaitre le menu j’ai ajouté un périphérique local présent sur le poste exemple « Camera » et le menu est apparu dans le client lourd.

Lancer le client bureau à distance vérifier que le menu Usb Remote Fx est bien présent

Réaliser la connexion, vous avez maintenant dans le menu du client la possibilité de connecter votre clé.


La solution reste compliquée pour une intégration en production cependant elle fonctionne parfaitement.

J'ai été convaincu par la qualité des produits Feitian, l'utilisation des clés couplée avec l'empreinte digitale est vraiment très intéressante. Je vous ferais un retour complet des différentes clés dans un prochain article.


