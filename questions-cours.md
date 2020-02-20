# Reponses

## - Quels sont les chemins d'attaque possibles sur la signature d'un système embarqué?

Un système de signature met en jeu deux clés : une clé publique et une clé privée.Un attaquant peut donc avoir plusieurs chemins d'attaque possibles.

Par exemple, lors de l'échange de la clé publique, un attaquant peut envoyer sa propre clé (attaque man in the middle). 
Un autre chemin d'attaque est le fait que l'attaquant peut tenter de trouver la clé privée du système embarqué, s'il la trouve, il peut tenter de la modifier pour se faire passer pour le signataire.
Enfin, l'attaquant peut altérer le système de génération des clés, afin de le rendre bien moins sécurisé.

## - A quoi sert la chaine de confiance? Pourquoi est-elle nécessaire?

La chaine de confiance sert de moteur de calcul séparé contrôlant le processeur cryptographique sur le PC ou l’appareil mobile dans lequel il est intégré. Autrement dit, grâce à un système de signature, il permet de vérifier que les données sont bien conformes (la signature correspond à celle attendue).

La chaine de confiance permet par exemple de détecter des modifications non autorisées sur un binaire, de détecter des rootkit, ou d'empêcher que des programmes ne puissent lire ou écrire n'importe ou sur la mémoire d'un autre programme.

## - Décrire la méthode pour aborder la sécurité sur un produit embarqué. Pourquoi établir un modèle d'attaquant est-il important?

1. Analyse du produit : on doit être bien au fait du système que l'on souhaite sécuriser

2. Faire un inventaire de toutes les menaces possibles sur le système

3. Essayer de pirater son propre système : trouver des failles, des possibilités d'attaque, en faisant du reverse engineering par exemple

4. Evaluer les côuts et voir ce qui est le plus rentable / intéressant

5. Mettre à jour régulièrement le système

Il est intéressant de faire un modèle d'attaquant, un peu comme établir un Persona dans les études de finance/marketing etc. Cela permet d'essayer de se mettre dans la peau d'un attaquant et d'imaginer quels sont ses intérêts, ses méthodes d'attaque etc. Ainsi, si on comprend son "ennemi", on peut mieux se prémunir contre ses attaques.

## - Trouver un moyen rapide de faire du debug embarqué (par exemple sur cible ARM)? Expliquer les avantages

Le moyen le plus simple est de passer par un émulateur, car cela permet de tester facilement (et à moindre coût, comme ça si on "casse" la cible, ce n'est pas grave, on recommence simplement l'émulation)

## - Quelles idées pour améliorer la sécurité en embarqué? (IA, Anti-debug, Obfuscation, Crypto ...) Choisissez une idée, chercher si elle existe et développer en quelques phrases quel avantage elle apporte et ses limites

On peut protéger le code source contre le reverse engineering grâce par exemple à l'obfuscation. Cette technique consiste à rendre le code illisible pour l'attaquant. L'obfuscation transforme le code source avant la compilation (tout en restant lisible uniquement pour le compilateur bien sûr). Cette technique a des avantages et des limites.

### Fonctionnalités

Grâce à l'obfuscation, on peut :

- transformer des identifiants
- supprimer les commentaires
- modifier des noms de variables (pour ne pas avoir une variable qui s'appelle directement "password"), etc.

### Avantages

- facile à mettre en place (en terme d'architecture)
- transparente pour l'utilisateur et les développeurs
- peu coûteuse financièrement

### Limites

- Il peut exister des "désobfuscateurs" : des logiciels qui essayent de réaliser l'opération inverse et "annuler" l'obfuscation. 
- De ce fait, cela retarde le reverse engineering, mais ca n'est pas une vraie barrière sur le long terme
- Ce traitement augmente le temps d'exécution de l'application : dans les systèmes temps réel, cela peut avoir des conséquences fâcheuses
- Certains appels à des API externes ne peuvent pas être obfusquées : cela donne donc quelques indices aux attaquants


