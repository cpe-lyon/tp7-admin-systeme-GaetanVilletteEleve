# Exercice 1. Personnalisation de GRUB

## 1. Commencez par changer l’extension du fichier /etc/default/grub.d/50-curtin-settings.cfg s’il est présent dans votre environnement (vous pouvez aussi commenter son contenu).

## 2. Modifiez le fichier /etc/default/grub pour que le menu de GRUB s’affiche pendant 10 secondes ; passé ce délai, le premier OS du menu doit être lancé automatiquement.
<code>
  cd /etc/default/
  sudo nano grub
</code></br>
Dans grub, il faut:
</br>modifier la variable GRUB_TIMEOUT=10
</br>ajouter la variable GRUB_TIMEOUT_STYLE=menu

### 3. Lancez la commande update-grub

### 4. Redémarrez votre VM pour valider que les changements ont bien été pris en compte

### 5. On va augmenter la résolution de GRUB et de notre VM. Cherchez sur Internet le ou les paramètres à rajouter au fichier grub.
Pour changer la resolution, il faut ajouter au fichier GRUB:
<code>
  GRUB_GFXMODE=1280x1024,1024x768x32 
  GRUB_GFXPAYLOAD_LINUX=keep
</code></br>

### 6. On va à présent ajouter un fond d’écran. Il existe un paquet en proposant quelques uns : grub2-splash-images (après installation, celles-ci sont disponibles dans /usr/share/images/grub).
<code>
  sudo apt install grub2-splashimages
  sudo apt update
  sudo cp -r /usr/share/images/grub /boot/grub
</code></br>
Il faut ajouter ensuite au fichier GRUB:
<code>
  GRUB_BACKGROUND="boot/grub/BonsaiTridentMaple.tga"
  sudo update-grub
</code></br>

### 7. Il est également possible de configurer des thèmes. On en trouve quelques uns dans les dépôts (grub2-themes-*). Installez-en un.
<code>
  sudo apt install grub2-themes-ubuntustudio
  sudo apt update
</code></br>
Il faut ajouter ensuite au fichier GRUB:
<code>
  GRUB_THEMES="boot/grub/ubuntustudio"
  sudo update-grub
</code></br>

### 8. Ajoutez une entrée permettant d’arrêter la machine, et une autre permettant de la redémarrer.
Dans /etc/grub.d/40_custom : 
<code>
  menuentry ’Arrêt du système’ { halt }
  menuentry ’Redémarrage du système’ { reboot }
</code></br>

### 9. Configurer GRUB pour que le clavier soit en français
sudo mkdir /boot/grub/layouts 
sudo grub-kbdcomp -o /boot/grub/layouts/fr.gkb fr 
Dans /etc/default/grub rajouter:
GRUB_TERMINAL_INPUT=at_keyboard 
Dans /etc/grub.d/40_custom rajouter:
#Clavier fr insmod keylayouts keymap fr


## Exercice 2. Noyau

### 1. Commencez par installer le paquet build-essential, qui contient tous les outils nécessaires (compilateurs, bibliothèques) à la compilation de programmes en C (entre autres).
<code>
  sudo apt install build-essential
</code></br>

### 2.Créez un fichier hello.c contenant le code sur le tp
<code>
  sudo touch hello.c
  sudo nano hello.c
</code></br>

### 3. Créez également un fichier Makefile :
<code>
  sudo touch Makefile
  sudo nano Makefile
</code></br>

### 4. Compilez le module à l’aide de la commande make, puis installez-le à l’aide de la commande make install.
<code>
  make
  make install
</code></br>


### 5. Chargez le module ; vérifiez dans le journal du noyau que le message ”La fonction init_module() est appelée” a bien été inscrit, synonyme que le module a été chargé ; confirmez avec la commande lsmod.
<code>
  sudo insmod /lib/.../hello.ko
  lsmod | grep $hel
</code></br>

### 6. Utilisez la commande modinfo pour obtenir des informations sur le module hello.ko ; vous devriez notamment voir les informations figurant dans le fichier C.
<code>
  modinfo hello.ko
</code></br>
Donne toutes les informations dont le nom commence par MODULE_, plus des informations complémentaires.</br>

### 7. Déchargez le module ; vérifiez dans le journal du noyau que le message ”La fonction cleanup_module() est appelée” a bien été inscrit, synonyme que le module a été déchargé ; confirmez avec la commande lsmod.
<code>
  sudo rmmod hello.ko
  lsmod | grep $hel
</code></br>

### 8. Pour que le module soit chargé automatiquement au démarrage du système, il faut l’inscrire dans le fichier /etc/modules. Essayez, et vérifiez avec la commande lsmod après redémarrage de la machine.
<code>
  lsmod | grep $hel
</code></br>

## Exercice 3. Interception de signaux










