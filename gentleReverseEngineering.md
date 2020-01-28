# Reponses

### Quels sont les chemins d'attaque possibles sur la signature d'un système embarqué?



### A quoi sert la chaine de confiance? Pourquoi est-elle nécessaire?

### Décrire la méthode pour aborder la sécurité sur un produit embarqué. Pourquoi établir un modèle d'attaquant est-il important?

### Trouver un moyen rapide de faire du debug embarqué (par exemple sur cible ARM)? Expliquer les avantages

### Lister les catégories de bug possibles et comment les exploiter et les défendre

### Quelles idées pour améliorer la sécurité en embarqué? (IA, Anti-debug, Obfuscation, Crypto ...) Choisissez une idée, chercher si elle existe et développer en quelques phrases quel avantage elle apporte et ses limites


# TD

## finir crack emily

### Commande file

```console
file ex1
```

Résultat obtenu :

`ex1: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=7512127f0f16a533d82d7d0dc42888b12d45fd2b, not stripped`

Cette commande permet de déterminer le type d'un fichier. Ici, on remarque que le fichier est de type "shared object", car c'est ce que créé GCC par défaut. Pour obtenir un "executable" comme dans le tuto, il faut ajouter l'option -no-pie :

`gcc -no-pie gentleReverseEngineering-ex1.c -o ex1`
`file ex1`

On obtient :

`ex1: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=98911243a16a67da490959d90ae18db85d2cbc16, not stripped`

En effet, un shared object est un "Positional Independant Executable (PIE)", et a une adresse mémoire donnée aléatoirement, tandis qu'un executable aura une adresse mémoire définie (mais les deux s'utilisent de la même façon).

### Commande strings

`strings ex1`

On obtient une longue liste avec beaucoup de blabla :

```console
/lib64/ld-linux-x86-64.so.2
libc.so.6
__isoc99_scanf
puts
printf
malloc
[...]

poop
Please input a word: 
That's correct!
That's not correct!

[...]
.init_array
.fini_array
.jcr
.dynamic
.got
.got.plt
.data
.bss
.comment
```

On peut voir entre autres toutes les chaines de caractères contenues dans notre programme. 

En testant cette commande sur un simple fichier .txt, la commande `strings monFichier.txt`affiche son contenu (sans tout le reste trouvé précédemment), car le type de fichier est plus simple (la commande `file monFichier.txt` donne "ASCII text")

On peut donc remarquer que selon le type de fichier (texte, executable, ...), la commande strings va être plus ou moins verbeuse, et donner beaucoup d'informations, comme les données, fonctions utilisées, etc.

En pratique, cette commande peut être utile dans le cas d'une attaque : l'attaquant peut ainsi repérer des mots clés tels que "password", "passphrase" ou autres, et ainsi repérer des informations sensibles dans le document, sans avoir forcément accès au code source.

### Commande objdump

`objdump -S -l -C -F -t -w ex1 | less`

Cette commande permet de voir le code assembleur de notre exécutable ex1. On peut voir entre autre le code de notre fonction is-valid :

```console
0000000000400686 <is_valid> (Offset dans le fichier : 0x686):
is_valid():
  400686:       55                      push   %rbp
  400687:       48 89 e5                mov    %rsp,%rbp
  40068a:       48 83 ec 10             sub    $0x10,%rsp
  40068e:       48 89 7d f8             mov    %rdi,-0x8(%rbp)
  400692:       48 8b 45 f8             mov    -0x8(%rbp),%rax
  400696:       48 8d 35 27 01 00 00    lea    0x127(%rip),%rsi        # 4007c4 <_IO_stdin_used+0x4> (Offset dans le fichier : 0x7c4)
  40069d:       48 89 c7                mov    %rax,%rdi
  4006a0:       e8 bb fe ff ff          callq  400560 <strcmp@plt> (Offset dans le fichier : 0x560)
  4006a5:       85 c0                   test   %eax,%eax
  4006a7:       75 07                   jne    4006b0 <is_valid+0x2a> (Offset dans le fichier : 0x6b0)
  4006a9:       b8 01 00 00 00          mov    $0x1,%eax
  4006ae:       eb 05                   jmp    4006b5 <is_valid+0x2f> (Offset dans le fichier : 0x6b5)
  4006b0:       b8 00 00 00 00          mov    $0x0,%eax
  4006b5:       c9                      leaveq 
  4006b6:       c3                      retq
```

Les lignes 40068e et 400692 indiquent une comparaison (fonction `strcmp` de notre programme). Le registre rbp pointe sur les données du programme. Les instructions suivantes (lea et callq) appellent la fonction strcmp, le résultat est stocké dans le registre eax. On compare les contenus de eax et si ils ne sont pas égaux (jne = jump if not equal), on passe à la ligne 4006b0, qui dit de mettre la valeur 0 dans eax. S'ils sont égaux, alors on passe à la ligne 4006a9, qui dit de mettre la valeur 1 dans eax. Dans les deux cas, on passe ensuite à la ligne 4006b5, qui fait quitter le programme.

Donc en d'autres termes, en regardant le code assembleur de la fonction is-valid, on comprend que le programme compare deux données, et renvoie 1 si elles sont égales, et 0 sinon.

Un attaquant peut donc, grâce à la commande objdump, comprendre dans les grandes lignes ce que fait notre programme. En addition avec la commande `strings`, il peut donc éventuellement mettre la main sur les données, et ainsi de "reconstituer" le programme (ou en tout cas avoir une idée générale).

### Commande dd

Le but ici, c'est de modifier l'exécutable sans avoir à toucher au code source, afin de toujours renvoyer 1 à la fonction is-valid. Ainsi, un attaquant pourrait se passer de mot de passe, et mettre n'importe quelle entrée à la fonction is-valid. On utilise la commande suivante :

`printf '\x01' | dd of=ex1 bs=1 seek=1713 count=1 conv=notrunc`

On a trouvé la valeur 1713 grâce à la méthode suivante :
- On récupère les bytes correspondant au retour égal à 0 : 4006b0:       b8 00 00 00 00
- `hexdump -C ex1 | grep "b8 00 00 00 00"` : 000006b0  b8 00 00 00 00 c9 c3 55  48 89 e5 48 83 ec 10 48  |.......UH..H...H|
- On veut modifier le 2eme bit pour écrire 1. On traduit donc "6b1" (2eme bit de cette ligne) en décimal pour utiliser la commande dd. 6b1 = 1713
- On utilise enfin la commande ci-dessus.

Résultat :
```console
./ex1 
Please input a word: coucou
That's correct!
```

On a donc trouvé le moyen de corrompre le fichier, en faisant en sorte de toujours valider la fonction is-valid.

## binwalk

## heap buffer overflow

