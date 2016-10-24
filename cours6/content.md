# CR340

###Systèmes d'exploitation

### Cours 6 - Le noyau (suite)
---
# Rappels
--
## Le noyau
![kernel](images/kernel.png)
(source: wikipedia)
---
# Les appels système (suite)
--
## Rappel
Un appel système est une méthode pour un processus *userspace* de faire une demande au noyau pour une ressource gérée par celui-ci.  Les appels système sont donc la principale méthode pour un processus d'interagir avec le système d'exploitation.
--
## Appels systèmes - Système de fichier
```
creat()
open()
close()
read()
write()
lseek()
dup()
link()
unlink()
stat()
fstat()
access()
chmod()
chown()
umask()
ioctl()
```
--
## Appels système - Communication interprocessus
```
pipe()
msgget()
msgsnd()
msgrcv()
msgctl()
semget()
semop()
shmget()
shmat()
shmdt()
```
---
# Les systèmes de fichier
--
## Qu'est-ce qu'un système de fichier?
--

## Permissions Posix
--
## Système de fichier virtuel
Le système de fichier virtuel est une extension au systèmes de fichiers Linux qui permettent d'accéder à de l'information sur le système de la même manière que
--
## `/dev`
Le répertoire `/dev` contient des fichiers représentant le matériel (et du pseudo-matérie) du système.  Ces fichiers représentent la manière principale d'interragir avec le matériel du système.
--
## Exemple: `/dev/urandom`
--
## Exemple: `/dev/sd*`
--
## `/proc`
Le répertoire /proc contient de l'information sur l'état actuel du système.  Beaucoup d'information retourné par les commandes de base de Linux proviennent de ce répertoire.
--
## Exemple
---
# Les signaux
--
## Qu'est-ce qu'un signal
Un signal est un mécanisme simple de communication inter-processus.  Linux définie 30 signaux, utiisé pour des raisons variables, pouvant être envoyé par le noyau ou par d'autre processus.  Les processus ne continennent aucun arguments et sont utilisé de manière similaire aux interruptions matérielles.
--
## Gestionnaire de signal


--
## Signaux Linux

|Signal|No|Utilisation|
|---------|------------|
|SIGHUP|1|Ligne raccrochée (terminal)|
|SIGINT|2|Interruption (terminal)|
|SIGQUIT|3|Cordump (terminal)|
|SIGILL|4|Instruction illégale|
|SIGTRAP|5|Trappe|
|SIGABRT|6|Annulation de l'exécution du processus|
|SIGBUS|7|Erreur du bus|
|SIGFPE|8|Exception point flottant|
--
## Signaux Linux

|Signal|No|Utilisation|
|---------|------------|
|SIGKILL|9|Tuer le processus (ne peux être reconfiguré)|
|SIGUSR1|10|Défini par l'utilisateur|
|SIGSEGV|11|Accès invalide a la mémoire|
|SIGUSR2|12|Défini par l'utilisateur|
|SIGPIPE|13|Écriture sur un tuyaux invalide|
|SIGALRM|14|Minuterie|
|SIGTERM|15|Termination du processus|
--
## Signaux Linux

|Signal|No|Utilisation|
|---------|------------|
|SIGSTKFLT|16|Erreur de pile|
|SIGCHLD|17|Changement d'un processus engant|
|SIGCONT|18|Continuer l'exécution (si arrêtée|
|SIGSTOP|19|Arrêter l'exécution (ne peux être reonfiguré)|
|SIGTSTP|20|Arrêter l'exécution (terminal)|
|SIGTTIN|21|Processus d'arrière plan tente de lire du terminal|
--
## Signaux Linux

|Signal|No|Utilisation|
|---------|------------|
|SIGTTOU|22|Processus d'arrière plan tente d'écrire au terminal|
|SIGURG|23|Condition urgente sur un *socket*|
|SIGXCPU|24|Limite d'utilisation du processeur dépassée|
|SIGXFSZ|25|Limite de taille de fichier dépassée|
|SIGVTALRM|26|Minuterie virtuelle|
|SIGPROF|27|Minuterie de profilage|
--
## Signaux Linux

|Signal|No|Utilisation|
|---------|------------|
|SIGWINCH|28|Obsolete|
|SIGIO|29|Entrée/sortie activée|
|SIGPWR|30|Redémarrage du à une panne de courant|
--
# Exemple
```C
#include<stdio.h>
#include<signal.h>
#include<unistd.h>

void sig_handler(int signo)
{
  if (signo == SIGINT)
    printf("received SIGINT\n");

  if (signo == SIGUSR1)
    printf("received SIGINT\n");
}

int main(void)
{
  if (signal(SIGINT, sig_handler) == SIG_ERR)
    printf("\ncan't catch SIGINT\n");

    if (signal(SIGUSR1, sig_handler) == SIG_ERR)
      printf("\ncan't catch SIGUSR1\n");
  while(1)
    sleep(1);
  return 0;
}
```
---
# Muli-tâche
--
## Processus
Un processus est une image binaire contenant du code en cours d'exécution.  Chaque processus possède une table de descripteurs et une table de pages de mémoire virtuelle.
--
## Planificateur de tâches
Le planificateur de tâche à la responsabilité de partager les ressources systèmes entre les différents processus.  Le planificateur de tâche analyse la demande en temps CPU et en resources d'entrée/sortie des diff/rents processus et décide l'ordre d'exécution de ceux-ci.  Le `load average`permet de savoir combien de processus était en attente d'éxécution dans les derniers 1, 5 et 15 minutes.  
--
## Priorités
Les processus peuvent avoir différents niveau de priorité.  Le niveau de priorité est fixé à l'aide de la commande `nice dans Linux.  Le niveau de priorité affecte le comportement du planificateur de tâches lorsque les processus demande plus de ressource que disponible.
---
# Laboratoire
```
fakeroot make menuconfig
fakeroot debian/rules binary-headers binary-generic
sudo dpkg -i linux*
```
