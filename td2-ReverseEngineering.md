# Binwalk

Le but du jeu est de changer l'image du petit pingoin de la démo de vmlinuz-qemu-arm-2.6.20.
--> On commence par télécharger vmlinuz-qemu-arm-2.6.20
--> On télécharge également binwalk

On utilise Binwalk car il permet de rechercher des fichiers et du code dans une image binaire.

### Commandes utilisées

```console
binwalk vmlinuz-qemu-arm-2.6.20
```

Résultat :

```console
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
12720         0x31B0          gzip compressed data, maximum compression, from Unix, last modified: 2007-05-09 06:03:48
```

Pour décompresser ce fichier, on utilise l'option -e :

```console
binwalk -e vmlinuz-qemu-arm-2.6.20
```

On obtient un dossier _vmlinuz-qemu-arm-2.6.20.extracted/

En se plaçant dedans, on effectue la même opération sur le fichier qu'il contient :

```console
ls
31B0
binwalk -e 31B0
cd _31B0_extracted/
ls
23F2F5.crt  E7E0
binwalk -e E7E0 
```

On obtient toute une liste de fichiers, avec notemment le nom des fichiers d'images. En particulier, le fichier contenant les images de pingouin s'appelle "tux.png" :
`2984412       0x2D89DC        ASCII cpio archive (SVR4 with no CRC), file name: "/usr/local/share/directfb-examples/tux.png", file name length: "0x0000002B", file size: "0x00006050"`

L'étape maintenant est de changer le fichier tux (on la remplace par une autre image qui s'appelle tux.png) et on recompresse tout.

