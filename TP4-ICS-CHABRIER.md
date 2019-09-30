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
- groupe1 pour u1 et u2<br>
- groupe2 pour u3 et u4


<span style='color:red'>7.</span> Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et
mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier
associé.



<span style='color:red'>8.</span> Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer
ou supprimer ce fichier ?

<span style='color:red'>9.</span> Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?

<span style='color:red'>10.</span> Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son
compte.

<span style='color:red'>11.</span> Quels sont l’uid et le gid de u1 ?

<span style='color:red'>12.</span> Quel utilisateur a pour uid 1003 ?

<span style='color:red'>13.</span> Quel est l’id du groupe groupe1 ?

<span style='color:red'>14.</span> Quel groupe a pour guid 1002 ? (Rien n’empêche d’avoir un groupe dont le nom serait 1002...)

<span style='color:red'>15.</span> Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.

<span style='color:red'>16.</span> Modifiez le compte de u4 de sorte que :
 - il expire au 1er juin 2019
 - il faut changer de mot de passe avant 90 jours
 - il faut attendre 5 jours pour modifier un mot de passe
 - l’utilisateur est averti 14 jours avant l’expiration de son mot de passe
 - le compte sera bloqué 30 jours après expiration du mot de passe

<span style='color:red'>17.</span> Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?

<span style='color:red'>18.</span> à quoi correspond l’utilisateur nobody ?

<span style='color:red'>19.</span> Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ?
Quelle commande permet de forcer sudo à oublier votre mot de passe ?

