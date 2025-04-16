# Challenge cybersecurite WCS  
---
 ## :tada: 1. Intro  
Bienvenue dans ce challenge cybersécurité de fin de session TSSR.
Tu peux utiliser tous les outils, techniques, sites web, logiciels, que tu souhaites pour arriver
à tes fins.
Les fichiers ou documents fournis peuvent t'aider, ou pas...
Soit inventif et perspicace !
BONNE CHANCE !

---
<details>
<summary><h1>:tada: 2. INIT<h1></summary>
  
### Instructions :  
* 8 caractères  
* Début : Az  
* Fin : 7  
* Sans : M et 5  

Nous avons donc un mot de passe dans ce genre :  
#### ``A z _ _ _ _ _ 7``  
Il y a 5 caractères à trouver : nombres (sauf "5", et lettres minuscules et majuscules à priori sans caractère spécial).  

Nombre de caractères possibles (sans M ni 5) :  
Majuscules (A-Z sauf M) → 25  
Minuscules (a-z) → 26  
Chiffres (0-9 sauf 5) → 9  
→ Total = 25 + 26 + 9 = 60  
**60^5 = 777 600 000 combinaisons possibles en tout**  

### :arrow_forward: Kali  
Afin de me simplifier la vie et d'avoir pas mal de logiciels de pentest installés je vais travailler sur une VM Kali :
  
![Capture d'écran 2025-04-13 112924](https://github.com/user-attachments/assets/88f55206-1b8e-4bc3-821a-d83f84601a72)  


### :arrow_forward: Crunch : Wordlist Generator  
Il serait possible de générer un script Bash assez technique pour générer une wordlist, mais avec 777 600 000 possibilités, le temps pour la créer serait énorme. Après quelques recherches sur internet, il existe un programme sur Linux appelé **Crunch** qui permet de générer facilement et très rapidement des wordlists.  
Je vérifie qu'il est bien installé :   

![Capture d'écran 2025-04-13 113246](https://github.com/user-attachments/assets/872b9ef9-d8c1-40a6-ac60-36e112e22a4a)


Son utilisation est finalement simple :  
Crunch est un générateur de wordlist : Un générateur de wordlist est un petit logiciel permettant de créer à partir de certains caractères définis la totalité des combinaisons possibles !  
L’ensemble des caractères qui seront utilisés pour générer tous les mots possibles s’appelle le **charset**.  
Je vais éditer en **root** ce fichier **charset.txt** pour créer ma propre liste.  
  
Ce fichier se trouve dans `/usr/share/crunch/charset.lst`.  
![Capture d'écran 2025-04-13 204153](https://github.com/user-attachments/assets/96e4ba0b-9931-45bf-b1a7-0c734187d81d)  

Je nomme mon nouveau "charset" `TSSRchallenge` comme défini au début, c'est à dire contenant les caractères : `abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLNOPQRSTUVWXYZ012346789` (sans "M" ni "5").  

:heavy_exclamation_mark: Attention à ne pas mettre de tabulation, uniquement des espaces, autrement il ne va pas réussir charger le charset.  

![Capture d'écran 2025-04-13 223144](https://github.com/user-attachments/assets/b7c8643a-0021-4f36-ae5d-a1fc31b4e9f5)

Ensuite on lance la génération de la wordlist. Prévoir assez d'espace disque car 777 600 000 mots de passe, ça prend de la place !  

![Capture d'écran 2025-04-13 223430](https://github.com/user-attachments/assets/d2cb684c-c25d-429a-b972-0fbca404a449)  

### ``crunch 8 8 -f /usr/share/crunch/charset.lst TSSRchallenge -t Az@@@@@7 -o wordlist.txt``
* `crunch` : lancement de la commande
* ``8`` : Le premier "8" signifie taille minimale du password  
* ``8`` : Le deuxième "8" signifie taille maximale du password
* ``-f /usr/share/crunch/charset.lst`` : indique que l'on indique le chemin d'un fichier de "charset".
* `TSSRchallenge` : Indique le charset que j'ai créé spécialement pour ce challenge.
* `-t Az@@@@@7` : On indique un template (-t) puis le template, commençant par "Az" puis les @ pour mentionner n'importe quel caractère présent dans mon charset, puis finissant par "7".
* ``-o wordlist.txt`` : Permet de rediriger la sortie vers un fichier texte.  

On peut vérifier le nombre de lignes dans notre fichier  
![Capture d'écran 2025-04-13 224428](https://github.com/user-attachments/assets/861731d9-8cee-4502-b6a3-63106945f5e7)  
C'est bon tout le monde est là :smile:.  

Si on veut vérifier si un mot est présent parmi les millions... :  
![Capture d'écran 2025-04-14 100542](https://github.com/user-attachments/assets/dd8433c6-efda-4708-bbed-2139ee84851f)


A partir de là notre wordlist est prête, il nous reste plus qu'à utiliser John the ripper pour lancer une attaque par dictionnaire avec ce que l'on vient de créer.  

### :arrow_forward: Envoi du dossier .zip de la machine physique (sous Windows) vers la VM Kali en SSH  
![Capture d'écran 2025-04-14 093623](https://github.com/user-attachments/assets/3ff6d197-18ba-4094-9a42-1c08e62355f8)

#### On vérifie sur la VM Kali que le dossier est bien copié  

![Capture d'écran 2025-04-14 094152](https://github.com/user-attachments/assets/61a87938-b7d8-42cf-8ce9-111813795786)



### :arrow_forward:"John The ripper"  

Nous allons utiliser l'outil "John the Ripper" pour lancer l'attaque.  
Je vérifie qu'il est bien installé  :  

![Capture d'écran 2025-04-12 202319](https://github.com/user-attachments/assets/a9d95f68-16c6-4e19-ad52-8c6f9889e83b)  
Il est bien installé.  

#### Je convertis le fichier `TSSRchallenge.zip` en un hash que j'appelle `hashzip` :  
![Capture d'écran 2025-04-14 101641](https://github.com/user-attachments/assets/a0529d13-a05b-416e-8f01-1a81623d741b)  

### :arrow_forward: Lancement de l'attaque (enfin !)  

ça ne prend que quelques secondes et le mot de passe est craqué !  
![Capture d'écran 2025-04-14 101750](https://github.com/user-attachments/assets/63a57259-a29e-4473-92b9-c806b9ce1d99)

### :white_check_mark: **`Azh792j7`** :white_check_mark:  

</details>

---

<details>
<summary><h1>:tada: 3. FILES ? <h1></summary>
 

La machine est protégées par un mot de passe et non n'avons aucune information...  
Il existe une possibilité pour palier à ça :  

### :arrow_forward: Modifier le Grub de Debian pour se connecter au Shell :shell: en Root :seedling: et ensuite modifier le mot de passe.  

#### Accéder au Grub au démarrage nous avons cette fenêtre :  
![Capture d'écran 2025-04-16 115037](https://github.com/user-attachments/assets/61eeb07a-c866-443b-9cb1-d2fee9a245c6)  

#### Cliquer sur **`e`** et on peut éditer le contenu du Grub, nous arrivons à cette fenêtre :  
![Capture d'écran 2025-04-16 115152](https://github.com/user-attachments/assets/665dfe41-d5da-4cbd-bda2-f610df3240f7)  

#### A la fin de la ligne commençant par "Linux" (vers la fin), il faut ajouter `rw init=/bin/bash` (Attention ici le clavier est en qwerty).  
![Capture d'écran 2025-04-16 115803](https://github.com/user-attachments/assets/6b66699b-6bd3-40ec-befc-479ab0967b63)  

:bulb: **Explications :**  
Quand un système Linux démarre, il suit un processus bien défini : le bootloader (comme GRUB) charge le noyau Linux, qui ensuite exécute le programme d'initialisation :  
(``init``, souvent ``/sbin/init`` ou ``systemd``). Ce programme est responsable de démarrer tous les services du système.  
Pour court-circuiter tout ça, et avoir un accès root sans passer par l’authentification on peut le faire via le grub en modifiant les options au démarrage.  
**`rw init=/bin/bash`** :  
* `rw` : Par défaut, au tout début du démarrage, la partition racine (/) est montée en **lecture seule** **(read-only, ro)** pour des raisons de sécurité.  
L’option **rw** force le montage du système de fichiers en **lecture-écriture**, ce qui est nécessaire si tu veux modifier des fichiers système (comme changer un mot de passe, éditer fstab, etc.).  
* `init=/bin/bash` : Cette option remplace le programme d’initialisation par défaut (comme systemd) par le shell /bin/bash.  
Résultat : au lieu de démarrer tout le système normalement, le noyau lance simplement un terminal bash avec les droits root, et c’est tout. Pas de login, pas de services, juste toi et ton terminal.  

#### Ensuite "Ctrl+X" pour rebooter.  
On accède au shell en mode root.  
![Capture d'écran 2025-04-16 123749](https://github.com/user-attachments/assets/77306d70-14de-4c06-b175-49658d8b2472)  

#### Vérifier si nous avons accès en lecture et écriture au système de fichiers où il y a l'OS :  
![Capture d'écran 2025-04-16 125846](https://github.com/user-attachments/assets/2d1ab647-ccb5-43f6-941f-59e71d89b94c)  
Si la commande retourne une ligne avec la valeur « (rw,realtime) » à la fin, cela signifie que vous avez un accès en lecture et en écriture au système de fichiers. Ainsi, il sera possible de changer le mot de passe, car on a les droits d’écriture.  

### :arrow_forward: Changer le mot de passe de Root avec `passwd` et redémarrer :  
![Capture d'écran 2025-04-16 130511](https://github.com/user-attachments/assets/59305538-cdab-4bb5-ba69-2a2d8e3c8cfb)  
⚠️ Au redémarrage, le clavier rebascule en "azerty", donc attention au mot de passe tapé en "qwerty" juste avant...  
![Capture d'écran 2025-04-16 130703](https://github.com/user-attachments/assets/7952da76-2f9c-4b03-a6ab-41f400bd67aa)  

### :white_check_mark: ☠️ Nous sommes connectés en Root avec notre propre mot de passe de façon définitive ! ☠️ :white_check_mark:  
#### On liste les utilisateurs existants dans la machine  
![Capture d'écran 2025-04-16 131122](https://github.com/user-attachments/assets/d2cce75f-731b-4bed-a8a2-5fc714bc92c4)  

On a 2 utilisateurs **ftponly** et **wildssh**.  
Je liste le contenu de leurs dossiers :  
* **ftponly**  
![Capture d'écran 2025-04-16 132225](https://github.com/user-attachments/assets/570291b1-9c87-4f31-8a90-7e1d8f130ab6)  

* **wildssh**  
![Capture d'écran 2025-04-16 132336](https://github.com/user-attachments/assets/a26f6c5b-e43e-4b87-9246-b198571daf67)  

A priori, seul les fichiers .zip de "ftponly" semblent intéressants pour le moment. Il me faut les dézipper, pour celà je dois télécharger un outil pour dézipper et donc paramétrer une connexion internet...  

### :white_check_mark: Paramétrage réseau :white_check_mark:  


</details>  

---
 
 ## :tada: 4. MAIN  

### :arrow_forward: Challenge 1 : trouver url et mot de passe  
Mot de passe du fichier :
Mot de passe classique de la formation concaténé avec la somme des 2 ports utilisée dans la première partie. Si tu as utilisé une méthode sans utilisation de port spécifique, demande à ton formateur le mot de passe...  

### :arrow_forward: Challenge 2 : trouver le nombre  
Mot de passe du fichier :
11 premiers caractères du nom du site (après le https://) trouvé au challenge 1 Et les 6 derniers caractères du mot de passe trouvé au challenge 1  

### :arrow_forward: Challenge 3 : trouver l'id  
Mot de passe du fichier :  
20 premiers caractères du sha512sum du numéro de coffre trouvé au challenge 2  

### :arrow_forward: Challenge 4 : trouver le mot de passe  
Mot de passe du fichier :  
10 premiers chiffres du code du bouton (trouvé au challenge 3) mis au cube  

### :arrow_forward: Challenge 5 : trouver le mot de passe  
Mot de passe du fichier :  
Date de naissance (en français) sur 6 chiffres concatenée avec le nom de famille  



