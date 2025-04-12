# Challenge cybersecurite WCS  
---
## 1. Intro  
Bienvenu dans ce challenge cybersécurité de fin de session TSSR.
Tu peux utiliser tous les outils, techniques, sites web, logiciels, que tu souhaites pour arriver
à tes fins.
Les fichiers ou documents fournis peuvent t'aider, ou pas...
Si tu es bloqué, ton formateur pourra - peut-être - t'aider à avancer !
Soit inventif et perspicace !
BONNE CHANCE !

## 2. INIT  
### Instructions :  
* 8 caractères  
* Début : Az  
* Fin : 7  
* Sans : M et 5  

Nous avons donc un mot de passe dans ce genre :  
#### **A z _ _ _ _ _ 7**  
Il y a 5 caractères à trouver : nombres (sauf "5", et lettres minuscules et majuscules à priori sans caractère spécial).  

Nombre de caractères possibles (sans M ni 5) :  
Majuscules (A-Z sauf M) → 25  
Minuscules (a-z) → 26  
Chiffres (0-9 sauf 5) → 9  
→ Total = 25 + 26 + 9 = 60  
**60^5 = 777 600 000 combinaisons possibles en tout**  


### Je vais utiliser "John The ripper". Je vérifie qu'il est bien installé  :  
![Capture d'écran 2025-04-12 202319](https://github.com/user-attachments/assets/a9d95f68-16c6-4e19-ad52-8c6f9889e83b)  
Il est bien installé.  

Je prépare un script pour générer la liste de 777 600 000 passwords que je vais rediriger dans un fichier texte pour ensuite générer une attaque par dictionnaire   

