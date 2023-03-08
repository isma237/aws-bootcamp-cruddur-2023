# Week 0 — Billing and Architecture

Cette première semaine était l'occasion d'apprendre plus sur le projet et sur le bootcamp. 
Durant les 14 prochaines semaines nous travaillerons sur Cruddur, le concurrent de Twitter avec notre idée magnifique des échanges éphémères. 

## A propos du projet
**Margaret** nous a présenté les attentes du client sous forme d'interview. J'ai vraiment apprécié l'approche. C'était une discussion agréable. Nous avons ensuite parlé architecture avec **Chris**.

## Architecture
Après l'intervention de Chris, j'ai pu comprendre les éléments sur lesquels nous pouvons nous appuyer pour définir une **bonne architecture** nous devons nous appuyer les exigences et sur les limitations qui sont imposées par le prohet. Nous parlerons donc:
- Verifiable
- Monitoring: Pour assurer la continuité de service 
- Traçable: Pour assurer l'**accounting** dans la notion de ***AAA***
- *Feasible* Qui doit tenir des exigences. Par exemple, vouloir déployer une application sur un Raspberry ne sera pas faisable

**Exemple**
- Disponibilité de 99,9
- Les différents cas d'utilisations prévus par l'application
- Les contraintes définis dans le cadre ISO 

Un deuxième point sur lequel nous devons nous appuyer pour mettre en place une bonne architecture est le fait de réduire les risques. Excemple, Avoir un seul noeud qui peut rendre notre service indisponible sera un risque. On devra donc trouver le moyen de mitiger l'impact de son indisponible. Redondance ? backup ?

Pour finir, l'architecture qu'on met en place doit tenir compte des **budgets**


Voici l'architecture que nous avons défini pour le projet
![Architecture_Crudder](/_docs/assets/cruddur%20Logical%20Diagram.png)

### Les trois niveaux d'architecture ou design

1. Conceptual Design 
    - Créer par l'équipe projet et les archiectes
    - Le but est définir les concepts et les principaux composants ainsi que les règles
2. Logical Design
    - A ce niveau, le but est de définir les éléments qui permettront de savoir comment le système sera implémentés
    - Pas besoin de définir les caractéristiques techniques de ressources. Penser logique et type de communications
3. Physical Design
    - Ici nous allons plus loin. Nous avons identifié le type d'instance EC2 à utilisé par exemple, on mentionnera son ARN
    - Les adresses IP 
    - C'est réellement l'architecture qui sera construite sur AWS par exemple. 


**NOTE IMPORTANTE:** La documentation est super importante

## Sécurité sur le cloud AWS

Sur AWS, le service qui permet de gérer les autorisations est appelé **IAM**. Sur IAM, nous pouvons gérer trois objets
1. Les utilisateurs - *USER*
2. Les groupes - *GROUP*
3. Les rôles - *IAM ROLE* 

Sur AWS, nous parlerons toujours de **least privilege**. L'idée du principe est de donner uniquement les droits necessaires aux différents objets IAM. 

### Les bonnes pratiques liés à la sécurité:
- Créer un utilisateur IAM sur lequel on attribuera le role **administrateur** et supprimer les crédentials du compte **root*
- Créer des utilisateurs en fonction des besoins. Regrouper ces utilisateurs dans des groupes pour une utilisation centralisée des autorisations quand ceci est possible
- Ne pas stockers les crédentials des utilisateurs dans les instances EC2. Il faut utiliser des rôles qui seront ensuite attachés aux instances en tant que **instance profile**
- Lorsque vous manipulez plusieurs comptes AWS, vous pouvez centraliser la gestion à travers **AWS Organization**


### La facturation
Nous avons appris à définir des budgets afin de suivre la facturation sur notre compte. Ceci s'est fait de deux façon:
1. Depuis la console de management
2. Depuis l'interface de ligne de commande avec la commande AWS qui a été installé sur Gitpod.  