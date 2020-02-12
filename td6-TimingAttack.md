# Qu'est ce qu'une timing attack ?

Une timing attack est un type d'attaque dans laquelle l'attaquant analyse le temps d'exécution d'un algorithme, afin d'obtenir des informations cachées, comme un code ou un mot de passe.

En effet, chaque opération logique de l'ordinateur prend du temps à exécuter. De plus, le temps de certaines opérations comme une comparaison peut prendre plus ou moins de temps selon le degré de similitude des deux entrées.

Un attaquant peut donc effectuer des mesures précises du temps d'execution d'un algorithme en testant différentes entrées, jusqu'à remonter vers l'information qu'il recherchait.

# Exemple

Pour la recherche d'un mot de passe, nous reprendrons l'exemple simpe is_valid, utilisant la comparaison entre le vrai mot de passe, et le mot que la fonction prend en paramètre.

def is_valid(mdp):
   if(mdp == "monMotDePasse"):
      return true
   else
      return false

(1) is_valid(monMotDepasse)
(2) is_valid(joapdcnghrue)

Les appels (1) et (2) ne vont pas s'exécuter dans le même temps. le (2) va échouer plus rapidement, on peut donc changer caractère par caractère en mesurant les temps d'exécution (en d'autres termes la rapidité d'échec), jusqu'à trouver le bon string.
		

# En quoi cela peut-il poser problème en embarqué ?

En embarqué, les systèmes sont souvent soumis à des ralentissements et autres aléas (à cause du mouvement, de la température, de la météo, ...
Par exemple, si on considère un téléphone portable et une application bancaire, l'attaquant peut se servir des différences d'inclinaison lors de l'appui sur les numéros d'authentification. Il peut par exemple repérer des doublons.

# Comment se prémunir contre cette attaque ?

On peut par exemple ajouter un sleep(nombre random) dans notre méthode is_valid afin d'induire en erreur l'attaquant.

Une autre méthode consiste à faire en sorte que le temps d'exécution de la fonction soit constant. Par exemple, on remplace (mdp == "monMotDePasse") par (sha256(mdp) == sha256("monMotDePasse"), la fonction sha256 appliquant les mêmes calculs quelle que soit la chaine de caractères entrée en paramètres.

On peut ainsi contrer l'attaquant.
