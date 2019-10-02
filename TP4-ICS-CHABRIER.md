Loïs CHABRIER

# _TP 4 - Utilisateurs, groupes et permissions_

## Exercice 1. Gestion des utilisateurs et des groupes

<br>
<span style='color:red'>1.</span> Commencez par créer deux groupes groupe1 et groupe2
</span>

<br>Pour créer des groupes, il faut être en root, donc entrer `sudo su` puis ensuite `addgroup groupe1 && addgroup groupe2`

<span style='color:red'>2.</span> Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de
leur dossier personnel et avec bash pour shell

<br>Entrer : `useradd u1 -m -s /bin/bash` puis `useradd u2 -m -s /bin/bash` puis `useradd u3 -m -s /bin/bash` puis `useradd u4 -m -s /bin/bash`

<span style='color:red'>3.</span> Placez les utilisateurs dans les groupes :
- u1, u2, u4 dans groupe1 : `adduser u1 groupe1` puis `adduser u2 groupe1` puis `adduser u4 groupe1`
- u2, u3, u4 dans groupe2 : `adduser u2 groupe2` puis `adduser u3 groupe2` puis `adduser u4 groupe2`

<span style='color:red'>4.</span> Donnez deux moyens d’afficher les membres de groupe2

1ère façon : `grep "groupe2" /etc/group`
2ème façon : `members groupe2` (si le package "members" n'est pas installé : `apt install members`)

<span style='color:red'>5.</span> Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire
de /home/u3 et /home/u4

 - `chown u1:groupe1 /home/u1`
 - `chown u1:groupe1 /home/u2` 
 - `chown u1:groupe2 /home/u3` 
 - `chown u1:groupe2 /home/u4` 

<span style='color:red'>6.</span> Remplacez le groupe primaire des utilisateurs :
- groupe1 pour u1 et u2 : `usermod -g groupe1 u1 ` puis `usermod -g groupe1 u2`
- groupe2 pour u3 et u4 : `usermod -g groupe2 u3` puis `usermod -g groupe2 u4`

<span style='color:red'>7.</span> Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et
mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier
associé.

Il faut faire `mkdir /home/groupe1 && mkdir /home/groupe2`
Puis `chown u1:groupe1 groupe1` et `chown u2:groupe2 groupe2` pour mettre groupe1 et groupe2 propriétaires de leurs fichiers
Et enfin `chmod 720 groupe1 groupe2`

<span style='color:red'>8.</span> Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer
ou supprimer ce fichier ?

Il suffit d'entrer la commande `chmod +t /home/groupe1/ && chmod +t /home/groupe2/`
Le `+t` correspond au "Sticky bit", c'est-à-dire qu'il permet seulement au propriétaire du fichier, au propriétaire du dossier ou root de renommer ou supprimer des fichiers dans ce dossier.

<span style='color:red'>9.</span> Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?

Entrer la commande `su u1` : il n'est pas possible de s'y connecter car l'utilisateur u1 ne possède pas encore de mot de passe.

<span style='color:red'>10.</span> Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son
compte.

Pour activer le compte utilisateur, il faut lui affecter un mot de passe comme ceci : `sudo passwd u1` puis entrer le nouveau mdp.
Ensuite, `su u1` pour vérifier si la connexion est opérationnelle.

<span style='color:red'>11.</span> Quels sont l’uid et le gid de u1 ?

Pour le voir : `id u1` ce qui retourne : `uid=1001(u1) gid=1001(groupe1) groups=1001(groupe1)`

<span style='color:red'>12.</span> Quel utilisateur a pour uid 1003 ?

Entrer `id 1003` et on remarque que c'est u3 qui a pour uid 1003.

<span style='color:red'>13.</span> Quel est l’id du groupe groupe1 ?

Entrer `cat /etc/group | grep groupe1` et cela retourne 1001.

<span style='color:red'>14.</span> Quel groupe a pour guid 1002 ? (Rien n’empêche d’avoir un groupe dont le nom serait 1002...)

Entrer `cat /etc/group | grep 1002` et on voit que c'est le groupe 2 qui a pour guid 1002.

<span style='color:red'>15.</span> Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.

Entrer `sudo gpasswd -d u3 groupe2`. Comme il n'est plus dans le groupe2, il ne peut plus accéder à /home/groupe2.

<span style='color:red'>16.</span> Modifiez le compte de u4 de sorte que :
 - il expire au 1er juin 2019 : `usermod --expiredate 06/01/2019 u4`
 - il faut changer de mot de passe avant 90 jours : `sudo chage -M 90 u4`
 - il faut attendre 5 jours pour modifier un mot de passe : `sudo chage -m 5 u4`
 - l’utilisateur est averti 14 jours avant l’expiration de son mot de passe : `sudo chage -W 14 u4`
 - le compte sera bloqué 30 jours après expiration du mot de passe : `sudo chage -I 30 u4`

<span style='color:red'>17.</span> Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?

Se connecter en root puis effectuer `echo $SHELL`. L'interpréteur de commande est `/bin/bash`.

<span style='color:red'>18.</span> A quoi correspond l’utilisateur nobody ?

nobody est un compte utilisateur ne possédant aucun fichier, qui n'est dans aucun groupe qui a des privilèges et dont les seules possibilités sont celles que tous les "autres utilisateurs" ont.

<span style='color:red'>19.</span> Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ?
Quelle commande permet de forcer sudo à oublier votre mot de passe ?

Par défaut, il garde le mot de passe en mémoire pendant 15 minutes.
Pour forcer l'oubli, il est possible d'exécuter la commande `sudo -k`.

## Exercice 2. Gestion des permissions

<br>
<span style='color:red'>1.</span> Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier contenant quelques lignes de texte. Quels sont les droits sur test et fichier?
</span>

<br>`cd /home &&  sudo mkdir test && cd test &&  sudo touch fichier &&  sudo echo 'salut' > fichier`
<br>Les droits du dossier test sont 755 (ce sont les droits de base pour un dossier sous linux) et les droits du fichier sont 644 (droits de base pour un fichier sous linux).

<span style='color:red'>2.</span> Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?

`sudo chmod 000 test` : il est toujours possible d'y accéder et de le modifier sous root.

<span style='color:red'>3.</span> Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?

`chmod 300 fichier && echo "hello world" > fichier` :

<span style='color:red'>4.</span> Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.



<span style='color:red'>5.</span> Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ? Rétablissez le droit en lecture sur test



<span style='color:red'>6.</span> Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvez- vous déduire de toutes ces manipulations ?



<span style='color:red'>7.</span> Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc...Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?



<span style='color:red'>8.</span> Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd ..” ? Pouvez-vous donner une explication ?



<span style='color:red'>9.</span> Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.



<span style='color:red'>10.</span> Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.



<span style='color:red'>11.</span> Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos réper- toires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.



<span style='color:red'>12.</span> Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.



<span style='color:red'>13.</span> Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.



<span style='color:red'>14.</span> Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier
/etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?


