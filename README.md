# Guide de style Git

Ceci est un guide pour Git inspiré par [*How to Get Your Change Into the Linux
Kernel*](https://www.kernel.org/doc/Documentation/SubmittingPatches),
les [pages de man](http://git-scm.com/doc) et plusieurs pratiques populaires
au sein de la communauté.

Des traductions de ce guide sont disponibles dans les langues suivantes:

* [Chinois (Simplifié)](https://github.com/aseaday/git-style-guide)
* [Chinois (Traditionnel)](https://github.com/JuanitoFatas/git-style-guide)
* [Japonais](https://github.com/objectx/git-style-guide)
* [Coréen](https://github.com/ikaruce/git-style-guide)
* [Portugais](https://github.com/guylhermetabosa/git-style-guide)
* [Ukrainien](https://github.com/denysdovhan/git-style-guide)
* [Français](https://github.com/pierreroth64/git-style-guide)

Si vous souhaitez contribuer, merci de vous lancer! Forkez le projet et
ouvrez une pull request.

# Table des matières

1. [Branches](#branches)
2. [Commits](#commits)
  1. [Messages](#messages)
3. [Fusionner](#merging)
4. [Divers](#misc)

## Branches

* Choisissez des noms *courts* et *signifiants*:

  ```shell
  # bon
  $ git checkout -b oauth-migration

  # mauvais - trop vague
  $ git checkout -b login_fix
  ```

* Les identifiants de tickets d'un service externe (ex: un ticket GitHub)
  sont aussi de bons candidats à utiliser dans le nommage des branches. Par exemple:

  ```shell
  # GitHub issue #15
  $ git checkout -b issue-15
  ```

* Utilisez des *tirets* pour séparer les mots.

* Quand plusieurs personnes travaillent sur la *même* fonctionnalité, il pourrait
  être utile de créer des branches *personnelles* et une branche *d'équipe*.
  Suivez la convention de nommage suivante:

  ```shell
  $ git checkout -b feature-a/master # branche d'équipe
  $ git checkout -b feature-a/maria  # branche personnelle de Maria
  $ git checkout -b feature-a/nick   # branche personnelle de Nick
  ```

  Fusionnez les branches personnelles dans la branche d'équipe (cf. ["Fusion"](#merging)).
  Eventuellement, la branche d'équipe sera fusionnée dans "master".

* Supprimez votre branche du dépôt maître après l'avoir fusionnée (à moins
  qu'il n'y ait une raison particulière de la conserver).

  Astuce: Utilisez la commande suivante tout en étant sur "master", pour lister
  les branches fusionnées:

  ```shell
  $ git branch --merged | grep -v "\*"
  ```

## Commits

* Chaque commit devrait être un seul *changement logique*. Ne faites pas de
  multiples *changements logiques* dans un commit. Par exemple, si un patch
  corrige un bug et optimise la performance d'une fonctionnalité, divisez-le
  en deux commits séparés.

* Ne divisez pas un seul *changement logique* en plusieurs commits. Par exemple,
  l'implémentation d'une fonctionnalité et les tests correspondants devraient
  être dans le même commit.

* Commitez *tôt* et *souvent*. De petits commits autonomes sont plus facilement
  compréhensibles et réversibles lorsqu'ils sont incorrects.

* Les commits devraient être ordonnés *logiquement*. Par exemple, si le *commit X*
  dépend des changements apportés par le *commit Y*, alors le *commit Y* devrait
  apparaître avant le *commit X*.

### Messages

* Utilisez l'éditeur et pas le terminal lorsque vous écrivez un message de commit:

  ```shell
  # bon
  $ git commit

  # mauvais
  $ git commit -m "Quick fix"
  ```

  Commiter depuis un terminal encourage l'idée de devoir tout inclure dans une
  seule ligne, ce qui conduit en général à des messages de commit ambigüs avec
  peu d'information.

* La ligne de résumé (ç-à-d. la première ligne du message) devrait être *descriptive*
  et *succincte*. Idéalement, elle ne devrait pas dépasser *50 caractères*.
  Elle devrait commencer par une majuscule et être écrite au présent de l'impératif.
  Elle ne devrait pas être terminée par un point puisque c'est effectivement
  le *titre* du commit:

  ```shell
  # bon - impératif présent, démarre par une majuscule, inférieure à 50 caractères
  Mark huge records as obsolete when clearing hinting faults

  # mauvais
  fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails.
  ```

* Après cette ligne devrait être inséré une ligne vide suivie par une description
  plus détaillée. Celle-ci devrait être limitée en largeur à *72 caractères* et
  expliquer *pourquoi* le changement est nécessaire, *comment* il adresse le
  problème et quels *effets de bord* il pourrait générer.

  Cette description devrait aussi fournir les pointeurs vers des ressources éventuelles
  (ex: lien vers le ticket correspondant dans le tracker de bug):

  ```shell
  Résumé court des changements (50 caractères ou moins)

  Test explicatif plus détaillé si nécessaire limité en largeur à
  72 caractères. Dans certains cas, la première ligne est considérée
  comme le sujet d'un email et le reste du texte comme le corps du
  message. La ligne vide séparant le résumé du corps du message est
  critique (à moins d'omettre le corps complètement); des outils comme
  rebase peuvent être perdus si vous utilisez les deux ensemble.

  Des paragraphes supplémentaires suivent après des lignes vides.

  - Les puces sont aussi acceptées

  - Utilisez un tiret ou un astérisque pour la puce,
    suivi par un espace simple, avec une ligne vide entre chaque puce

  Source http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
  ```

  Et finalement, lorsque vous écrivez un message de commit, pensez à ce dont
  vous auriez besoin si vous aviez à vous y pencher dessus dans un an.

* Si un *commit A* dépend d'un autre *commit B*, la dépendance devrait être
  mentionnée dans le message du *commit A*. Utilisez le hash du commit lorsque
  vous faites référence à des commits.

  De façon similaire, si le *commit A* résout un bug introduit par le *commit B*,
  cela devrait être mentionné dans le message du *commit A*.

* Si un commit est destiné à être fusionné dans un autre commit, utilisez les
  options `--squash` et `--fixup` afin de clairement exprimer votre intention:

  ```shell
  $ git commit --squash f387cab2
  ```

  *(Astuce: Utilisez l'option `--autosquash` lorsque d'un rebase. Les commits
   marqués seront automaitquement fusionnés.)*

## Fusionner

* **Ne réécrivez pas l'historique publié.** L'historique du dépôt a une réelle
   valeur et il est très important de pouvoir savoir *ce qui s'est vraiment passé*.
   Altérer l'historique publié est une cause connue de problèmes pour quiconque
   travaille sur le projet.

* Cependant, il y a des cas où réécrire l'historique est légitime. Ces cas sont:

  * Vous êtes la seule personne à travailler sur la branche et elle n'a pas été revue.

  * Vous voulez nettoyez votre branche (ex: fusionner des commits) et/ou la rebaser sur
    la branche "master" dans le but de la fusionner plus tard.

  Ceci dit, *ne réécrivez jamais l'historique de la branche "master"* ou de quelconque
  branche spéciale (ex: celles utilisées en production on par serveurs d'intégration
  continue).

* Gardez l'historique *propre* et *simple*. *Juste avant de fusionner* votre branche:

    1. Assurez-vous qu'elle est conforme au guide et corrigez si vous constatez
       des écarts (fusionner/réordonner des commits, retravailler les messages etc.)

    2. Rebasez la sur la branche dans laquelle elle sera fusionnée:

       ```shell
       [ma-branche] $ git fetch
       [ma-branche] $ git rebase origin/master
       # puis fusionnez (merge)
       ```

       Ces opérations conduisent à obtenir une branche qui peut être appliquée directement
       à la fin de la branche "master" et produit ainsi un historique simple.

       *(Remarque: Cette stratégie est plus adaptée aux projets avec des branches
       à courte durée de vie. Sinon, il est préférable de fusionner la branche "master"
       de temps en temps plutôt que de rebaser sur celle-ci.)*

* Si votre branche inclut plus d'un commit, ne la fusionnez pas avec un fast-forward:

  ```shell
  # bon - s'assure que commit de fusion est créé
  $ git merge --no-ff ma-branche

  # mauvais
  $ git merge ma-branche
  ```

## Divers

* Il y a de multiples workflows et chacun a ses propres forces et faiblesses.
  Qu'un workflow vous convienne dépendra de l'équipe, du projet et de vos
  procéddures de développement.

  Ceci dit, il est important de réllement *choisir* un workflow et de s'y tenir.

* *Soyez cohérent.* Ceci est valable pour le workflow mais s'entend aussi pour
  les messages decommit, les noms de branches et les tags. Être cohérent sur
  l'ensemble du dépôt rend plus facile la compréhension de ce qui ce passe en
  parcourant les logs, les messages de commit, etc...

* *Testez avant de pusher.* Ne pas pusher du travail non terminé.

* Utilisez des [tags annotés](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Annotated-Tags)
  pour marquer vos releases ou autres points importants de l'historique. Préférez
  les [tags légers](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Lightweight-Tags)
  pour un usage personnel, pour par exemple marquer des commits afin de les référencer
  ultérieurement.

* Maintenez vos dépôts en exétutant des tâches de maintenance de temps en temps dans
 vos dépôts locaux *et* distants:

  * [`git-gc(1)`](http://git-scm.com/docs/git-gc)
  * [`git-prune(1)`](http://git-scm.com/docs/git-prune)
  * [`git-fsck(1)`](http://git-scm.com/docs/git-fsck)

# License

![cc license](http://i.creativecommons.org/l/by/4.0/88x31.png)

Ce travail est sous licence internationale Creative Commons Attribution 4.0.

# Credits

Agis Anastasopoulos / [@agisanast](https://twitter.com/agisanast) / http://agis.io


# Traduccion

Peio Roth / [@pierreroth64](https://twitter.com/pierreroth64)
