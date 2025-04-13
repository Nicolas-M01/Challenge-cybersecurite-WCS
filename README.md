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

## :tada: 2. INIT  
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

Je nomme mon nouveau "charset" `TSSRchallenge` comme définie au début, c'est à dire contenant les caractères : `abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLNOPQRSTUVWXYZ012346789` (sans "M" ni "5").  
![Capture d'écran 2025-04-13 204617](https://github.com/user-attachments/assets/dcd74bd3-5c0a-4202-9716-e26637eabdf5)











* Je vais utiliser "John The ripper". Je vérifie qu'il est bien installé  :

![Capture d'écran 2025-04-12 202319](https://github.com/user-attachments/assets/a9d95f68-16c6-4e19-ad52-8c6f9889e83b)  
Il est bien installé.  

