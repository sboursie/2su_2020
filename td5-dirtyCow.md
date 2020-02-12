# Dirty Cow, c'est quoi ?

D'après Wikipedia :

markdown
> Dirty COW (copy-on-write) est une vulnérabilité de sécurité du noyau Linux qui affecte tous les systèmes d'exploitation Linux, y compris Android (jusqu'à la version Android.7). C'est un défaut d'élévation de privilège qui exploite une condition de concurrence dans la mise en œuvre de la copie sur écriture dans le noyau de gestion de la mémoire. Cette vulnérabilité a été découverte par Phil Oester. Un attaquant local peut exploiter cette vulnérabilité pour rendre un fichier accédé en lecture seule en un accès en écriture. 

La vulnérabilité donne des accès en écriture à des zones mémoire normalement accessibles en lecture seule. D'après Phil Oester, 

markdown
> l’exploitation de ce bug ne laisse aucune trace d’événement anormal dans les journaux d’activité.



# Comment exploiter la faille ? Que peut-on faire avec ?

Cette faille peut être utilisée pour obtenir un accès root sur la machine. En obtenant les privilèges du root, les programmes malveillants ont un accès illimité au système, ce qui peut être très dangereux : il peut effacer ou modifier les fichiers, voler des données personnelles, etc.

# Comment s'en protéger ?

Au niveau des appareils fonctionnant sur Android, Google a mis au point un patch correctif en 2016, fonctionnant sur Android 4.4 ou supérieur. Il est donc bon de faire régulièrement des mises à jour sur ses appareils.
Afin de vérifier, allez sur votre téléphone Android. Dans Paramètres/A propos du téléphone/Mise à jour du correctif de sécurité, vérifiez que la date est plus récente que décembre 2016.

Au niveau des ordinateurs, la plupart des systèmes basés sur Linux comme Ubuntu ou Debian ont été corrigés. En revanche, les mises à jour sont bien moins fréquentes sur les systèmes embarqués avec un noyau Linux : ce sont eux les plus vulnérables.
