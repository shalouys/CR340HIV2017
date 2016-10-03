# CR340

###Systèmes d'exploitation

### Cours 4 - Le noyau

---
# Qu'est-ce qu'un noyau?


---
## Les interpréteurs de commande
--
## Qu'est-ce qu'un interpréteur de commandes?
Un interpréteur de commande est un logiciel qui permet à un utilisateur d’interagir avec le système d’exploitation (ou autre système) à l’aide d’un langage simple basé sur des commandes textuelles.

--
## Les interpréteurs de commande

L’interpréteur de commande roule en Userspace et permet d’envoyer des appels systèmes au noyau

Bien qu’à la base, tous les interpréteurs de commande offrent des fonctionnalités similaires, chaque interpréteur possède des paramètres, des commandes et une syntaxe différente.

--
## Exemples
* Cmd.exe
* Powershell
* Bash
* Python
* Node

---
# Les variables d'environnements
--
## Variables d'environnement
Une variable d’environnement est un couple de type nom-valeur qui peut être accédé par tous les processus lancés à l’intérieur d’une instance d’un interpréteur de commande.

Ces variables peuvent être créées et modifiées par l’interpréteur de commande et lues par les différents processus.
--
## Utilité des variables d'environnement

Les variables d’environnement sont utiles pour plusieurs choses:
* Définir des paramètres globaux en lien avec la session.
* Dans le cas de script, permettre à l’usager de définir certains paramètres opérationnels
* Remplacer des chaînes de caractères longues et devant être répétés à plusieurs endroits.
* Stocker temporairement le résultat d’une commande ou d’une opération.

---
# Les descripteurs de fichiers
--
## Descripteur
Un descripteur est une référence vers un accès d’entrée-sortie.

De manière concrète, un descripteur est souvent représenté comme un entier représentant l’index de la table des fichiers ouverts du système d’exploitation pour le fichier voulu.

Toute entrée-sortie d’un processus vers tout mécanisme d’entrée-sortie se fera à travers un descripteur.

Les descripteurs peuvent permettre les opérations de lecture (r), d’écriture (w) ou les deux à la fois(rw).
--
## Descripteurs standards

Dans les systèmes compatibles Posix (tous les dérivés d’Unix) il existe 3 descripteurs standards accessible à tous les processus:

|Index|Descripteur|Permissions|
|-----|-----------|-----------|
|0|STDIN|Lecture|
|1|STDOUT|Ecriture|
|2|STDERR|Ecriture|
--
## STDIN

Le descripteur STDIN est utilisé par le processus pour recevoir des données.
Par défaut, ce descripteur est lié au clavier de l’ordinateur.  Toute entrée provenant du clavier pendant qu’un processus est actif sera disponible via le descripteur STDIN.

--
## STDOUT

Le descripteur STDOUT est utilisé pour permettre au processus de sortir de l’information.
Par défaut, il est lié a la console d’où le processus a été démarré.  Ainsi, toute donnée écrite dans STDOUT sera affichée à la console.
--
## STDERR

Le descripteur STDERR est utilisé pour permettre au processus de sortir de l’information en lien avec les erreurs ou les opérations de contrôles du processus.
Tout comme STDOUT, par défaut, il est lié a la console d’où le processus a été démarré.  Ainsi, toute donnée écrite dans STDERR sera affichée à la console.
--
## STDOUT vs STDERR

Par défaut, il ne semble pas y avoir beaucoup de différence entre STDOUT et STDERR.  Les deux sont des descripteurs de sortie et les deux par défaut sont liés à la console.  Pourquoi deux descripteurs alors?

La différentiation entre les deux descripteurs devient utile lorsqu’un processus peut vouloir, en parallèle, présenter des données primaires (par exemple, le résultat d’une opération) et des données sur l’état de l’opération ou des erreurs.
--
## STDOUT vs STDERR

Puisque ces deux types de données seront écrites dans des descripteurs différents, il sera possible de les différencier à la sortie.  Il est donc possible d’utiliser une redirection ou un tuyau pour rediriger l’information primaire (STDOUT) a un endroit, et l’information de contrôle (STDIN) à un autre.


---
# Les redirections
--
## Les redirections

Les redirections sont des mécanismes qui permettent de redéfinir le fonctionnement des descripteurs par défaut.

Deux grands types de redirections existent:
* La redirection d’entrée (<)
* La redirection de sortie (>).
--
## La redirection d'entrée

La redirection d’entrée est signalée en ajoutant à la fin d’une commande  le symbole < suivi de la source à utiliser.

Ce type de redirection permet de redéfinir le descripteur STDIN pour le processus à démarrer.

Il est possible de redéfinir STDIN comme étant un fichier préexistant sur le système, ou un tuyau nommé, au lieu d’entrée interactive du clavier.
Exemple d’utilisation:  `wc –l < bigfile.txt`
--
## La redirection de sortie

La redirection d’entrée est signalée en ajoutant a la fin d’une commande symbole > suivi de la nouvelle destination.

Puisque deux descripteurs de sortie existent, il est possible d’ajouter le numéro du descripteur avant le symbole de redirection.  Par exemple, il est possible de faire une redirection de STDERR en utilisant les symboles 2>.  Par défaut, la redirection de sortie s’effectue sur le descripteur STDOUT.

Ce type de redirection permet de prendre les données normalement affichées a la console et de les envoyer vers un fichier ou un tuyau nommé.
Exemple d’utilisation:  `ps –ef 1> stdout.txt 2> stderr.txt`
---
# Les tuyaux (*pipes*)
--
## Qu'est-ce qu'un tuyau

Un tuyau est, à la base, un mécanisme de communication interprocessus. Il permet donc à deux processus distincts de communiquer de l’information.

Un tuyau est un mécanisme à sens unique, les données peuvent donc être communiquées d’un processus source vers un processus de destination.  Les communications dans le sens inverse sont impossibles.
--
## Posix - Files d'attente

Dans le monde Posix, un tuyau est un mécanisme de communication basé sur le processus de file d’attente unique (queue).

Ceci signifie que les données qui entrent dans le tuyau vont sortir dans leur ordre d’entrée.  Le processus cible ne peut demander une donnée en particulier et il n’est pas possible de prioriser certaines données.

Ce type de file d’attente est souvent appelé FIFO pour First In First Out.
--
## Powershell - Objets

Dans le monde Powershell, au lieu de files d’attente classique, les tuyaux communiquent des objets entiers.
Cette façon de fonctionner est plus versatile que les tuyaux Posix car l’information est reçue de manière structurée et il est donc plus facile de l’utiliser et la manipuler.

--
## Types de tuyaux

Il existe deux types de tuyaux principaux:

* **Tuyau direct**:  Ce tuyau est utilisé à l’intérieur même d’une ligne
* **Tuyau nommé**:  Ce type de tuyaux est disponible seulement sur les systèmes compatibles Posix.  Ces tuyaux possèdent un nom et peuvent être utilisés par deux commandes invoquées séparément.
--
## Tuyau direct

Il est important de noter que ce type de tuyaux affecte seulement le descripteur STDOUT et non le descripteur STDERR.

Exemple:  `ps –ef | grep bash`

Dans le monde Powershell, le tuyau direct permet d’utiliser un objet ou une collection d’objets sortant d’une commande comme entrée à une autre commande.

Exemple:  `Get-Process | Select-Object name`
--
## Tuyau direct

Un tuyau direct est indiqué à l’intérieur d’une ligne de commande par l’utilisation du caractère |.
Dans le monde Posix, ce type de tuyaux permet de lier le descripteur STDOUT d’une commande source au descripteur STDIN d’une commande destination.
De cette manière, les données en sortie d’une commande sont utilisées comme entrée pour une autre.
--
## Tuyau nommé

Le standard Posix permet de définir des tuyaux nommés à l’aide de la commande mkfifo
Cette commande crée un fichier spécial sur le disque ayant les propriétés d’une file d’attente FIFIO.
Ainsi, toute donnée envoyée au tuyau restera présente tant qu’un autre processus n’aura pas lu son contenu.  Aussitôt le contenu lu, il n’est plus disponible dans le tuyau.
Un tuyau nommé peut être effacé comme un fichier régulier en utilisant la commande rm.
