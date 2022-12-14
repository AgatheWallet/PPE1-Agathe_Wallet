# Journal de bord du projet encadré


---
## 28/09/2022

1. Création du repository sur GitHub et des fichiers Readme.md et journal-de-bord.md
2. Attendre un petit peu après commit avant de faire status, sinon ne détecte pas les modifications

### Les commandes utilisées :  
- `git clone` : clone le repository sur la machine  
- `git status` : affiche statut de repository sur la machine vs repository en ligne  
- `git log` : affiche historique des commits  

Autre.s commande.s :  
- `git push` : ajouter modif faites sur ordi dans le repository en ligne  


---
## 05/10/2022

Exercices sur Shell : 
- Exercice 1 : Créer une arborescence pour classer les documents et y copier les documents
- Exercice 2 : Compter le nombres d'annotations par année, limiter le comptage aux lieux et enregistrer les résultats, établir le classement des lieux les plus cités, trouver les annotations les plus fréquentes pour le mois de décembre

### Les commandes utilisées :  

#### Exercice 1
- `mkdir repository` : création répertoire
- `cp rep1/file.txt rep2/file.txt` : copie du fichier file.txt depuis le rep 1 dans le rep 2
- `ls (-l et/ou -a)`: visualiser le contenu du répertoire dans lequel on se trouve
- `cd rep` : se déplacer dans rep (si se trouve dans le répertoire actuel)

#### Exercice 2
- `ls 2016*.ann |wc -l` : pour compter le nombre d'annotation dans faites en 2016
- `cat 2016*.ann | egrep Location | wc -l` : compte le nombre de ligne contenant Location parmi les annotations faites en 2016 (problème de passage à la ligne entre 2 fichiers)
- `touch file.txt` : création d'un fichier file.txt
- `echo "Bonjour" > file.txt` : remplace le contenu du fichier file.txt par "Bonjour"
- `echo "Je m'appelle Agathe"` >> file.txt : ajoute "Je m'appelle Agathe" au contenu de file.txt
- `sed -e "s/T1\t/\nT1\t/g"` : remplace "T1 suivi d'une tabulation" par "T1 précédé d'un passage à la ligne et suivi d'une tabulation"
- `sed -e "s/^T[0-9]*\t[A-Za-z]*\s[0-9]*\s[0-9]*//g"` : utilisé pour normaliser les fichiers d'annotations → retire tout ce qui précède le nom correspondant au type d'annotation (nom de lieu, nom d'organisation, etc)
- `sort file.txt | uniq -ic | sort -nr` : trie le fichier pour qu'il soit dans l'ordre alphabétique, retire les doublons et compte le nombre d'occurences, en ignorant la casse et trie à nouveau le fichier par la fréquence, du plus grand au plus petit


---
## 25/10/2022
1. Création de scripts pour compter les annotations de lieux (compte_loc.sh), classer les lieux (classement_lieux.sh) et compter le nombre de tel type d'annotations pour telle année (compte.sh).


### Les commandes utilisées : 
- `#!/usr/bin/bash` : appelle bash
- `ANNEE=$1` : permet de renommer l'argument 1 ANNEE (plus explicite dans le programme)
- $ANNEE : appel de la variable ANNEE, correspondant à l'argument 1
```bash
for condition
do
	# what we have to do
done
```
```bash
if [[condition1]]
then
	# what we do
elif [[condition2]]
then
	# what we do
else
	# what we do
fi
```


---
## 09/11/2022

1. Création de pages github
2. Création index.html avec ajout d'un lien
3. Création tableau.html avec différents titres et liens et création d'une liste

### Les commandes utilisées : 
```html
<p>texte<p> : création paragraphe
<ul>  
	<li>élément 1</li>  
	<li>élément 2</li>  
</ul> : création liste
<a href=lien>Texte</a> : création lien
<h1>Titre1</h1> : création gros titre (jusqu'à h6)
```


---
## 15/11/2022
1. Mise à jour du journal de bord

---
## 16/11/2022

### Validation et construction du corpus depuis des URLs

#### Exercice 1

Le script `traitement_url_base.sh` fonctionne sur mon fichier d'URLs.  

---
#### Exercice 2
1. Récupération de l'entête de la réponse du serveur
```bash
curl -ILs <url>
```
L'option -I permet de n'afficher que l'entête et l'option -L permet de suivre les redirections.  

2. Le code de réponse se trouve dans la première ligne de l'entête. Pour le récupérer, nous pouvons utiliser `grep` ou `egrep`, l'option `-o` permettant de ne récupérer que ce qu'on cherche et pas la ligne entière et des expressions régulières (en utilisant l'option `-E`: la ligne contenant le code réponse commence par `HTTP/`. Nous devons donc commencer par chercher cet élément. Nous pouvons ensuite récupérer le code avec l'option `-o`, indiquant que nous cherchons un élément composé de 3 chiffres sur la ligne commençant par `HTTP` (par exemple, `200` indique que l'URL a été trouvée correctement). En cas de redirection, il ne faut récupérer que le dernier code. Nous pouvons donc ajouter que nous voulons la dernière occurence de ce motif en utilisant `tail` avec l'option `-n` grâce à laquelle nous pouvons indiquer le nombre de ligne à partir de la fin que nous voulons récupérer (ici une seule). 

3. 
```bash
code=$(curl -ILs $URL | grep -e "^HTTP/" | grep -Eo "[0-9]{3}" | tail -n 1)
```

---
#### Exercice 3

→ Voir https://agathewallet.github.io/PPE1-Agathe_Wallet/site/

---
#### Exercice 4

1. L'information sur l'encodage se trouve, lorsqu'elle est présente, au niveau sur la ligne commençant par `Content-Type` et est indiqué après `charset=`.

2. Pour ne récupérer que l'encodage, nous pouvons exécuter la ligne suivante : 
```bash
curl -ILs $URL | grep -Eo "charset=(\w|-)+" | tail -n 1 | cut -d= -f2
```
Nous pouvons la stocker dans une variable pour pouvoir manipuler l'encodage dans notre script.  

---
#### Exercice 5

1. Pour récupérer le contenu textuel et ne pas avoir les liens en fin de page en utilisant lynx, il faut utliser les options `-dump` et `-nolist`. La première option récupère le contenu textuel et la seconde permet de ne pas prendre en comte les listes de lien.

2. Si le site n'est pas en UTF-8, nous pouvons convertir l'encodage avec la commande `iconv` de l'encodage trouvé en UTF-8.



---
## 14/12/2022
1. Installation de wordcloud : `conda install -c conda-forge wordcloud`
2. Utilisation de la commande : `wordcloud_cli --text ./dump-fr.txt --imagefile ./image.png --scale 2 --stopwords motsvides.txt`
- `--text` permet d'indiquer le fichier texte à partir duquel il faut faire le nuage de mots
- `--imagefile` permet d'indiquer le fichier image de sortie
- `--scale n` permet d'agrandir l'image n fois
- `--stopwords` permet d'ajouter un fichier contenant des stopwords à ne pas prendre en compte dans notre nuage de mots


## 27/12/2022

### Deuxième feuille d'exercices sur Git
1. Création d'un tag sur le commit 5a2874eae27726ab9e01ca2d83312ff78c5d8537 (en date du 28092022)
```bash
git tag -a -m "version finie séance 2" 20220928 5a2874eae27726ab9e01ca2d83312ff78c5d8537
git push origin 20220928
```
1. Création d'un tag sur le commit 5a2874eae27726ab9e01ca2d83312ff78c5d8537 (en date du 28092022)
```bash
git tag -a -m "version finie séance 2" 20220928 5a2874eae27726ab9e01ca2d83312ff78c5d8537
git push origin 20220928
```
2. Création d'une erreur et annulation du commit
- Ajout d'une phrase inutile au Jdb + add + commit avec message 'I AM ERROR'
- Ajout d'une section à la date d'aujourd'hui + add + commit + push
- Récupération de l'id SHA du commit erroné : 
```bash
git log
```
- Annulation du commit erroné 7f700438915e70241e9cb3aeb648fde4b8749349
```bash
git reset 7f700438915e70241e9cb3aeb648fde4b8749349
```
Je n'ai pas l'impression d'avoir réussi à annuler le commit en question. En effet, lorsque je fais un git status, on m'informe que je suis "en retard" d'un commit par rapport à origin/main, ce qui doit correspondre au reset. Je me suis dit qu'il fallait peut-être que je push le reset mais j'ai alors une erreur comme quoi il faut que je pull d'abord. Pour pouvoir puller, il faut que je reset mon journal de bord ce qui n'amène aucun changement in fine. J'ai alors essayé avec revert et le numéro du commit mais au lieu de me retirer le commit erreur, cela m'a retiré, j'ai l'impression, le commit correct que j'ai effectué après puisque ma section à la date d'aujourd'hui était supprimée du journal de bord.  

3. Retour vers le passé
- J'ai d'abord écrit plusieurs choses dans oups.md, en commitant entre chaque.
- J'ai ensuite fait un `git revert 20221227-initial` pour revenir à l'état de mon git au moment du tag. 
- Je ne sais pas si la commande à marcher correctement car j'ai en effet à nouveau perdu des informations dans mon journal de bord, mais plus que je ne pensais, et le fichier oups.md était intact. Je ne suis pas sûre d'avoir saisie vraiment ce qu'il fallait faire dans cet exercice et j'avoue ne pas comprendre comment fonctionne les commandes `reset` ou `revert` ni ce que siginifie le 'staging'.  

---
1. Mise à jour du journal de bord
2. Ajout du contexte et du concordancier à mon scirpt

## 28/12/2022

- Amélioration du concordancier → ne récupérait que quelques occurrences car ne prenait pas en compte les occurrences dans les premières phrases des paragraphes

## 01/01/2023

- Ajout du script de normalisation des textes pour iTrameur
