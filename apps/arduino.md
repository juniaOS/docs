---
description : Présentation de l'application Arduino
---

# Arduino


## Introduction
Arduino est une plateforme de prototypage à bas coût sous licence Creative Commons. Basée sur les microcontrôleurs ATMEL, elle permet de réaliser rapidement des projets électroniques sur les trois plateformes Linux, Mac et Windows. 

Arduino fournit un utilitaire graphique, ainsi qu'une bibliothèque de librairies permettant de programmer les cartes et des périphériques simples.

## Installation

### À partir des dépôts

Le paquet Arduino est présent dans les dépôts Universe d'Ubuntu.\\
Pour l'installer, il suffit d'[[:tutoriel:comment_installer_un_paquet|installer le paquet]] **[[apt>arduino|arduino]]**.\\
La version présente dans le dépôt n'est pas toujours la plus récente, il peut être intéressant d'installer la dernière version depuis l'archive du site officiel en suivant la méthode décrite ci-dessous.

Un utilitaire est présent dans les dépôts. Il permet de programmer Arduino en ligne de commande, Pour l'installer, il suffit d'[[:tutoriel:comment_installer_un_paquet|installer le paquet]] **[[apt>arduino-mk|arduino-mk]]**.

### Depuis l'archive du site officiel

Pour installer Arduino à partir de l'archive officielle :

  * télécharger l'archive .tgz (choisir "32 bit" ou "64 bits" selon votre [[architecture_materielle|architecture]]) sur [[https://www.arduino.cc/en/Main/Software|le site officiel]]
  * [[archivage|extraire l'archive]]
  * lancer le script "install.sh" de l'archive, vous n'avez qu'à glisser-déposer le fichier dans un terminal et appuyer sur "Entrée" (ctrl-alt-t pour ouvrir un terminal)
  * Un raccourcis "Arduino IDE" sera créé sur votre bureau, vous devez le rendre exécutable en faisant clic droit → Propriété → Permission → cochez "autorisez l'exécution du fichier comme un programme"
  * lancez le logiciel du raccourcis

**(en)** [[https://www.arduino.cc/en/Guide/Linux|Installation d'Arduino IDE sous linux sur le site officiel]]
**(en)** [[http://www.arduino.cc/playground/Linux/Ubuntu|Pour les plus anciennes versions d'Ubuntu]].

### Utilisation des ports série du menu
Les nouvelles versions du logiciel vous demanderont lors de la première exécution de vous authentifier pour vous donner l'autorisation d'accéder aux ports série en vous ajoutant aux groupes "tty" et "dialout". Notez que vous devez vous déconnecter et vous reconnecter pour que les changements soient effectifs.

Si pour une raison ou pour une autre l'utilitaire échoue à l'ajout du groupe, vous pouvez le faire manuellement:
<code> sudo usermod -a -G tty Nom_Utilisateur </code>
<code> sudo usermod -a -G dialout Nom_Utilisateur </code>
   
modifier les droits de /ttyACM0 avec la commande (la carte doit être connectée !) : 
<code> sudo chmod a+rw /dev/ttyACM0</code>

ou depuis la mise a jour

<code> sudo chmod a+rw /dev/ttyUSB0 </code>

il faut ensuite se déconnecter et se reconnecter pour que les modifications soient effectives.

Si le paquet a été installé à partir de la logithèque GNOME (logiciels) alors il faut en plus autoriser l'accès aux fichiers personnels et aux périphériques USB à partir du bouton "Autorisations" pour obtenir les droits sur les ports séries.

### Problèmes connus 
<note important>Il semble que ce problème soit résolu avec les nouvelles versions d'Arduino IDE (à confirmer) le problème n'a pas été rencontré avec une carte arduino (R3) sous Ubuntu 12.10 ni sous Ubuntu 14.04 ou sous 16.04</note>

Problème de micro-logiciel utilisé pour la communication par le port USB : Sur les Duemilanove, le CI utilisé pour la communication Série USB entre Arduino et le port USB était un CI FTDI qui fonctionne très bien.\\ Avec la version Arduino UNO, ce CI a été remplacé par un nouveau CI dédié utilisant un micro-logiciel dédié. Ce changement entraîne des problèmes de communication entre le PC et la carte Arduino Uno sous Ubuntu. Ceci est vrai pour les versions avant mai 2011 apparemment.\\
Sur ces versions de carte Arduino UNO antérieures à mai 2011, il est nécessaire de mettre à jour le micrologiciel de communication USB de la carte UNO, ce qui se fait par le port USB. Voir [[http://arduino.cc/en/Hacking/DFUProgramming8U2|cette page du projet]] décrivant la manipulation ainsi que [[http://www.mon-club-elec.fr/pmwiki_reference_arduino/pmwiki.php?n=Main.MaterielUnoMAJFirmwareUSB|ce site en français]] qui explique très bien les opérations à faire.

Si le port de sortie //ttyUSBx// ou //ttyACMx// n'apparaît pas dans la liste des ports série du logiciel Arduino, une autre page à consulter - en anglais - pour installer le module cdc_acm et lier le matériel au module (fonctionne avec Arduino UNO R3 firmware Rev.001 et Ubuntu studio 14.04 64 bits)[[http://playground.arduino.cc/Linux/All|Installation of arduino on all Linux version]]). En simplifiant :
  * Récupérer les identifiants du vendeur et du produit en saisissant dans un [[:terminal]]: <code>lsusb -v</code> qui répondra par exemple <code>Bus 003 Device 002: ID XXXX:YYYY</code> 
  * Créer le lien avec le port : avec [[:sudo|les droits superutilisateur]], on [[:tutoriel:comment_modifier_un_fichier|crée]] le fichier **/etc/udev/rules.d/99-arduino.rules**
     * On y place : <file>SUBSYSTEMS=="usb", ATTRS{idProduct}=="YYYY", ATTRS{idVendor}=="XXXX", SYMLINK+="ttyACM%n" </file> en remplaçant XXXX ET YYYY par vos valeurs précédemment récupérées
     * On fait charger le module ´´cdc_acm´´ au démarrage. Avec les [[:sudo|droits du superutilisateur]] , [[:tutoriel:comment_modifier_un_fichier|modifier le fichier]] **/etc/modules** pour ajouter la ligne  <file>cdc_acm</file>
  * On rend le port accessible à l'utilisateur : ce port est dans le groupe dialout.
      * On rattache l'utilisateur au groupe dialout : tableau de bord/Système/Utilisateurs et groupes - Gérer les groupes - sélectionner dialout et cliquer sur Propriétés - cocher l'utilisateur
  * On reboot, on branche l'Arduino et on vérifie avec ´´dmesg´´ dans une console que l'on a quelque chose comme : <code>cdc_acm 3-1:1.0: ttyACM0: USB ACM device</code>
  * On lance le logiciel Arduino et dans Menu Outils/Ports série, on sélectionne /dev/ttyACM0
<note tip>
Pour les cartes Arduino autres que UNO, on a /dev/ttyUSBX  au lieu de  /dev/ttyACM0 et le groupe est uucp au lieu de dialout (renseignements dars la page citée plus haut).
</note>


## Utilisation

### Compilation et programmation sous console 
<note> Cette méthode ne permet peut-être pas d'utiliser les librairies fournies par Arduino </note>
Il s'agit dans cette partie de montrer comment compiler avec avr-gcc et comment charger votre programme dans votre carte Arduino.

#### Installation 
[[apt://avr-gcc | avr-libc | avrdude | binutils-avr]]

#### Compilation
**avr-gcc** s'utilise presque comme gcc, en spécifiant le microcontrolleur de la carte.\\
Par exemple pour une carte Arduino UNO, avec un avr atmega328p, on a :
<code> avr-gcc -mmcu=atmega328p -o main.elf main.c </code>
<note tip> La liste des micro-controlleurs supportés est spécifiée dans le [[:man|manuel]] d'avr-gcc :
<code> man avr-gcc </code> </note>

Puis on extrait les données utilisables par le micro-controlleur :
<code> objcopy -O ihex -R .eeprom main.elf main.hex </code>


#### Programmeur 
La programmation de la carte se fait avec avrdude. Il faut lui spécifier le programmeur, le micro-contrôleur et le port sur lequel la carte est branchée.\\
Dans notre exemple le programmeur est un "stk500v2", mais un type de programmeur plus spécifique a été créé : "arduino".\\
Le micro-controleur sera cette fois-ci : m328p.
<note tip> Là encore, la liste des micro-controlleurs et programmeur supportés se trouve dans le manuel: <code> man avrdude </code> </note>
Vous aurez par exemple :
<code> sudo avrdude -c arduino -p m328p  -P /dev/cuaU0 -U flash:w:main.hex </code>
<note important>  Le port peut varier. On peut le retrouver en comparant les fichiers dans /dev/ avant et après le branchement de la carte. Le fichier qui apparaîtra sera sûrement le port à choisir </note>



### Simulation de l'Arduino

Il existe un petit logiciel qui permet de simuler son utilisation simulide sur le [[http://simulide.blogspot.com/|site officiel]].

Il existe un petit logiciel qui permet de simuler son utilisation, mais il ne semble pas fonctionner sous [[:Wine]], une machine [[:virtualisation|virtuelle]] Windows est ici nécessaire : il s'appelle [[http://www.virtualbreadboard.com|VBB]].

Il existe un logiciel pour dessiner les plans des cartes électroniques pour l'Arduino : [[:fritzing]].

Il est possible de contrôler l'Arduino par le biais de [[:scratch#etendre_les_capacites_du_logiciel|Scratch]].

## Voir aussi
  * [[http://www.mon-club-elec.fr/pmwiki_reference_arduino/pmwiki.php?n=Main.HomePage|Une version française du site Arduino]]
  * Le tutoriel Arduino pour les débutants sur [[https://openclassrooms.com/courses/programmez-vos-premiers-montages-avec-arduino|OpenClassrooms]] (ex Site du Zéro).
  * [[http://fr.flossmanuals.net/arduino/|Manuel français sur Arduino]] et l'électronique libre, réalisé par Flossmanuals Francophone
  * [[http://blog.ardublock.com/]] Ardublock programmation graphique de l’Arduino.
  * [[https://eskimon.fr/]] blog très détaillé de la programmation avec Arduino.

