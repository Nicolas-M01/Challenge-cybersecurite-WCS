# Challenge Cybersecurite WCS  
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

A priori, seuls les fichiers .zip de "ftponly" semblent intéressants pour le moment. Il me faut les dézipper, pour celà je dois télécharger un outil pour dézipper et donc paramétrer une connexion internet...  

### :arrow_forward: Paramétrage réseau  
Avec ça on ve pas aller loin il faut tout paramétrer  
![Capture d'écran 2025-04-16 143600](https://github.com/user-attachments/assets/46b47be4-8b8d-4a0e-b32a-1837f6199808)  
#### Paramétrage dans le fichier de conf. avec le nom de la première carte fournie par `ip a`. En mode DHCP :  
![Capture d'écran 2025-04-16 143638](https://github.com/user-attachments/assets/002245f8-021f-44bd-b405-23817ec1e73a)  
#### Redémarrage du service et vérif de la config  
![Capture d'écran 2025-04-16 143722](https://github.com/user-attachments/assets/5696f056-6398-49f9-9e83-91e416cac390)
#### Ping Google  
![Capture d'écran 2025-04-16 143749](https://github.com/user-attachments/assets/d5d65540-46b4-4587-bdf5-1650dedcf16b)  

A partir de là nous avons internet, je peux télécharger les outils pour dézipper.  

### :arrow_forward: Téléchargement `Zip` & `Unzip`  
![Capture d'écran 2025-04-16 145203](https://github.com/user-attachments/assets/a277bc24-3cb0-4dbd-81a2-4fcc7f6e022e)  
( Finalement uniquement `unzip` était utile)  

### :arrow_forward: décompresser les zip  
ça fonctionne  
![Capture d'écran 2025-04-16 145604](https://github.com/user-attachments/assets/c414a626-0150-44df-8a79-32189b20079b)  

:white_check_mark: **La suite au numéro 4...**  


</details>  

---


<details>
<summary><h1>:tada: 4. MAIN <h1></summary>

#### :disappointed_relieved: Je n'arrive pas  à trouver le premier mot de passe avec les indications... donc j'envoie tous les fichiers zip sur Kali pour faire une attaque par dictionnaire (au moins pour la première)  
#### Paramétrage de la carte réseau sur Kali pour être en DHCP et surtout sur le même réseau que la Debian.  
![Capture d'écran 2025-04-16 151932](https://github.com/user-attachments/assets/cf41b8eb-76ea-4a6e-bb60-8c9bdf7bb3e9)  
![Capture d'écran 2025-04-16 152022](https://github.com/user-attachments/assets/6bec3a50-b7d0-49b1-a6b4-c3b2a61d4c0e)  
#### Elles communiquent entre elles :  
![Capture d'écran 2025-04-16 152114](https://github.com/user-attachments/assets/8c217498-c3bc-4fc8-a731-3233e75b2de6)  
#### Le service SSH client est installé et activé sur la Debian (le serveur est également fonctionnel sur Kali :  
![Capture d'écran 2025-04-16 152411](https://github.com/user-attachments/assets/6da55a58-dde4-4fee-82f7-a6c5b9d3e49c)  
#### Envoi des fichiers en SSH avec SCP :  
![Capture d'écran 2025-04-16 152713](https://github.com/user-attachments/assets/6399f75c-3c8a-47f2-8834-667f62a26708)  
#### Vérification de la bonne réception des fichiers  
![Capture d'écran 2025-04-16 152743](https://github.com/user-attachments/assets/011cafed-a6ac-47ce-ae25-2006163b755f)  



### :arrow_forward: Challenge 1 : trouver url et mot de passe  
#### Mot de passe du fichier : Mot de passe classique de la formation concaténé avec la somme des 2 ports utilisée dans la première partie. Si tu as utilisé une méthode sans utilisation de port spécifique, demande à ton formateur le mot de passe...  

#### Génération d'une wordlist commençant par `Azerty1*` et suivi de 2 chiffres avec redirection vers un fichier (explications de Crunch sont expliquées en haut). PAr défaut, lorsque l'on ne charge pas de "charset", le `%` signifie n'importe quel chiffre  
![Capture d'écran 2025-04-16 154630](https://github.com/user-attachments/assets/b95da82a-bc11-4100-8a22-68abeceb59a3)  
#### Création du hash  
![Capture d'écran 2025-04-16 154650](https://github.com/user-attachments/assets/f629e74a-0a0b-40fa-894e-a7c4b3f64c47)  
#### Attaque avec John  
![Capture d'écran 2025-04-16 154844](https://github.com/user-attachments/assets/29ada736-4ea3-42ac-be30-3207591d5344)  

:white_check_mark: ☠️ **`Azerty1*43`** Bam craqué ! :white_check_mark: ☠️  
Je suis sûr de l'avoir tapé au début, je devais avoir un problème d'inversion du verr. num. !  
#### Mot de passe fonctionne  
![Capture d'écran 2025-04-16 155505](https://github.com/user-attachments/assets/6ebf681a-5e9f-4c12-a520-64c93acff9d1)  
#### Ouverture du PDF  
![Capture d'écran 2025-04-16 155630](https://github.com/user-attachments/assets/8fa81190-e89c-43ce-bd0e-1be45873ca77)  
![Capture d'écran 2025-04-16 164701](https://github.com/user-attachments/assets/95ad9b14-493f-4a32-bb28-8f22b78e885a)  

#### Visiblement il y a un fichier Wireshark à DL  
![Capture d'écran 2025-04-16 160821](https://github.com/user-attachments/assets/7e774416-66e9-4564-ac41-c3ee3638dcc3)  
#### Ouverture du fichier avec les trames ethernet. Mon instinct me dit que je dois regarder dans le protocole HTTP pour avoir une adresse URL, je choisis la première, Bingo  
![Capture d'écran 2025-04-16 161241](https://github.com/user-attachments/assets/33c5688a-c842-43f6-b498-f68ddec50bd4)  
#### L'entrée de la grotte !  
![Capture d'écran 2025-04-16 161633](https://github.com/user-attachments/assets/674e55d4-eee3-4d35-aa50-143fb813d2f7)  
#### Code d'entrée du site  
En cherchant dans les trames on tombe sur  
![Capture d'écran 2025-04-16 163230](https://github.com/user-attachments/assets/1bded15d-e233-4804-94a1-6d7f202ba884)  
Effectivement il fonctionne  
![Capture d'écran 2025-04-16 164446](https://github.com/user-attachments/assets/b11a1436-7762-414b-bfb5-ae81328e5f1a)  


### :arrow_forward: Challenge 2 : trouver le nombre  
Mot de passe du fichier :
11 premiers caractères du nom du site (après le https://) trouvé au challenge 1 Et les 6 derniers caractères du mot de passe trouvé au challenge 1  
Voici le récap :  
![Capture d'écran 2025-04-16 215838](https://github.com/user-attachments/assets/40dab55f-d004-4d20-9425-1d930801321b)  
Trame 19 on a les 2 réunis. Après avoir mis un peu de temps à trouver comme je ne prenais pas les bons codes (pfff), voici donc la combinaison :  
cyber-cours (pour les 11 premiers caractères du site trouvé) + les 6 derniers du mot de passe : S3cr3T  
![Capture d'écran 2025-04-16 215524](https://github.com/user-attachments/assets/038840fd-a48b-4318-a413-84158409aa2f)  

:white_check_mark: ☠️ `cyber-coursS3cr3T` :white_check_mark: ☠️  

On ouvre le fichier pdf, et le plus intéressant est ça...    
![Capture d'écran 2025-04-16 223315](https://github.com/user-attachments/assets/d71396ca-2f4c-464d-bd0d-c4edfed58c5e)  

#### On rentre dans la grotte, il y a 100 pages webs 1 page contient le bon mot clé `toison`, (toison d'or)  
![Capture d'écran 2025-04-16 174714](https://github.com/user-attachments/assets/473aaad4-3247-4e3b-b708-cb4fdc2e0888)  
![Capture d'écran 2025-04-16 174725](https://github.com/user-attachments/assets/e7533e3c-d1b0-4e5a-9122-2767ea4e5d9c)  

Ok il faut faire un script pour parcourir les pages web, et rechercher avec "grep", le mot "toison". Bon là je me tourne vers chatGPT et j'ajuste le script :  
#### création du script  
![Capture d'écran 2025-04-16 223232](https://github.com/user-attachments/assets/3dd6e2af-9994-478f-ba1d-63ee722c73d3)  

```bash
#!/bin/bash

# Configuration
BASE_URL="http://cyber-course.wildcodeschool.com/coffre.php?n=" # URL de base, il reste que le nombre à la fin pour parcourir
MOT_RECHERCHE="toison"   # mot à chercher
NB_PAGES=100 # nombre de pages total

# Vérifie que html2text est installé
command -v html2text >/dev/null 2>&1 || { echo >&2 "html2text n'est pas installé. Lance : sudo apt install html2text"; exit 1; }

# Boucle pour parcourir toutes les pages en commençant par 1 et jusqu'à 100
for i in $(seq 1 $NB_PAGES); do
    URL="${BASE_URL}${i}"
    echo "📄 Page $i : $URL"

    wget -q -O temp_page.html "$URL"

    # Extraction texte propre
    TEXTE=$(html2text temp_page.html)

    # Affiche si le mot est trouvé
    if echo "$TEXTE" | grep -qi "$MOT_RECHERCHE"; then
        echo "✅ Mot trouvé sur la page $i : $URL"
    else
        echo "❌ Mot non trouvé"
    fi
done

rm -f temp_page.html

```
#### ça marche !  
![Capture d'écran 2025-04-16 223032](https://github.com/user-attachments/assets/51ea8bc0-d008-4337-8dcf-e72928d55943)  
![Capture d'écran 2025-04-16 223145](https://github.com/user-attachments/assets/4053357d-4deb-44de-bb0b-821660a6ea0e)  

#### Modif du code HTML de la page contenant le coffre  
![Capture d'écran 2025-04-16 225714](https://github.com/user-attachments/assets/64c56d77-8db4-4300-b17d-ad2c2e9263e1)  
Je le change en "enabled"  
![Capture d'écran 2025-04-16 225624](https://github.com/user-attachments/assets/d0bd49e8-b69f-4183-9ae0-4400e6d3b05b)  
ça fonctionne !  
![Capture d'écran 2025-04-16 225641](https://github.com/user-attachments/assets/37af866b-ac00-4a98-ac20-cf193a9d5099)  





### :arrow_forward: Challenge 3 : trouver l'id  
Mot de passe du fichier :  
20 premiers caractères du sha512sum du numéro de coffre trouvé au challenge 2  
#### Génération du hash en sha512  
![Capture d'écran 2025-04-20 114006](https://github.com/user-attachments/assets/a9d9c22f-e25c-42c4-b67c-d9acca42d9a9)  
💡 Explications :  
* Numéro de coffre : `51`, on le met entre guillements  
* On affiche notre numéro de coffre avec `echo`, mais on ajoute l'option `-n` pour éviter le saut de ligne  
* Ensuite on met un pipe "|" pour envoyer la commande à `sha512sum` qui va générer un hash en sha512.  
Et ça marche.
#### Si on veut générer la sha512 et récupérer que les 20 premiers caractères, on ajoute la commande cut par un "pip" avec les bons paramètres  
![Capture d'écran 2025-04-20 114843](https://github.com/user-attachments/assets/9cfbdb51-664f-473d-9327-4e389e968814)  
![Capture d'écran 2025-04-20 115100](https://github.com/user-attachments/assets/ede8f26d-e198-41cc-9efd-c6c582150b6e)  

:white_check_mark: ☠️ `861522120d559ea5f946` :white_check_mark: ☠️  

![Capture d'écran 2025-04-20 115431](https://github.com/user-attachments/assets/9d0bde1f-b3c5-4fe9-9bba-6a605569ed99)  
![Capture d'écran 2025-04-20 115442](https://github.com/user-attachments/assets/0c066df4-e469-479d-83ac-ea9ed7f515fb)  






### :arrow_forward: Challenge 4 : trouver le mot de passe  
Mot de passe du fichier :  
10 premiers chiffres du code du bouton (trouvé au challenge 3) mis au cube  

#### 15700416^3 = 287 019 840 914 387 344 997. Donc  les 10 premiers chiffres :  
:white_check_mark: ☠️ **``3 870 198 409``** ☠️ :white_check_mark:  
![Capture d'écran 2025-04-19 191706](https://github.com/user-attachments/assets/2e4e6014-d892-4c82-bdc0-135233524876)  
ça marche !  
![Capture d'écran 2025-04-19 192311](https://github.com/user-attachments/assets/b8071784-a423-4af9-ab3e-324547036f65)  
![Capture d'écran 2025-04-19 192324](https://github.com/user-attachments/assets/cf71412b-05d5-4b33-af61-8e3108285f4a)  

#### Je me connecte donc à `https://www.db-fiddle.com/`, j'ai aussi téléchargé la BDD et je l'envoie sur db-fiddle.  
J'obtiens ceci :  
![Capture d'écran 2025-04-19 194326](https://github.com/user-attachments/assets/3aaf05f1-e82d-4b56-b04b-8c8d0aec79b6)  

Comme je ne connais rien à SQL, je demande à une IA de m'expliquer en gros. Je lance ensuite une requête SQL pour connaître les rôles dans la base de données, car je ne sais pas si la personne que je recherche est enregistré sous "Admin", Mécanicien" ou une variante.  
![Capture d'écran 2025-04-19 194332](https://github.com/user-attachments/assets/6011699f-d42f-4ddd-a36f-8245838c8a96)  
Pour lance la requête il faut cliquer sur "Run"  
![Capture d'écran 2025-04-19 194617](https://github.com/user-attachments/assets/62a42873-a496-46fe-9085-65be4f367acf)  

#### Parfait, il y a un role `admin`   
![Capture d'écran 2025-04-19 194355](https://github.com/user-attachments/assets/7c694b2b-925d-45cb-8afa-4180660bc885)  


#### Il ne reste plus qu'à le localiser, avec une nouvelle requête :  
![Capture d'écran 2025-04-19 194449](https://github.com/user-attachments/assets/327e6195-cddf-4e43-8ae4-b87710e5fb61)  

#### :white_check_mark: ☠️ Nous l'avons trouvé avec son mot de passe !! ☠️ :white_check_mark:  
![Capture d'écran 2025-04-19 194510](https://github.com/user-attachments/assets/dee66d58-8b9a-4dcb-9dd3-2f93ef69f9fc)  

#### On a presque fini !



### :arrow_forward: Challenge 5 : trouver le mot de passe  
Mot de passe du fichier :  
Date de naissance (en français) sur 6 chiffres concatenée avec le nom de famille  
#### :white_check_mark: ☠️ **`040780mecanicus`** ☠️ :white_check_mark:  
![Capture d'écran 2025-04-20 111811](https://github.com/user-attachments/assets/6ff9fa98-afb8-4c69-a576-888b39389cec)  
Clic droit, choisir l'endroit, rentrer le bon mot de passe  
![Capture d'écran 2025-04-20 112224](https://github.com/user-attachments/assets/208d5c19-8d9a-4022-b531-10648bce0b55)  
![Capture d'écran 2025-04-20 112341](https://github.com/user-attachments/assets/d80d0fbe-8d37-4b7d-940a-ce59cea552e2)  
![Capture d'écran 2025-04-20 112430](https://github.com/user-attachments/assets/9a80c339-3a64-4330-a467-e2c333c69916)  
![Capture d'écran 2025-04-20 112444](https://github.com/user-attachments/assets/4deccbc8-345c-4206-9e03-583dd3eaaef8)  


OUVERTURE DU COFFRE

Le code de l'admin est `796a80b899e3e787173eff40a3778dd6`. Nous apprenons que c'est en fait un hash en `MD5`.  
Je ne vais pas suivre les conseils fournis au challenge 5, je vais utiliser John The ripper et générer une wordlist avec `crunch`.  
le PDF nous dit qu'il y a au moins 8 caractères et que le code est uniquement en alpha numérique, ce qui limite les possibilités.  
après quelques recherches, je me rend compte qu'un hash en md5, ne fait pas la différence entre majuscules et minuscules et que les lettres sont en hexadécimal.  
Donc on peut se limiter à `abcdef0123456789`.  



</details>

---
