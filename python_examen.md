+++
title = "Examen"

date = 2018-09-09T00:00:00
# lastmod = 2018-11-21T:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math = true


edit_page = {repo_url = "https://github.com/Bertbk/fem_tp", repo_branch = "master", submodule_dir="content/course/fem_tp/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_fem_tp"
  submodule_dir = "content/course/fem_tp/"

# Add menu entry to sidebar.
[menu.fem_tp]
  parent = "python"
  identifier = "python_examen"
  name = "Examen"
  weight = 30

+++

$\newcommand{\diff}{\mathrm{d}}$
$\newcommand{\xx}{\mathbf{x}}$
$\newcommand{\vec}[1]{\mathbf{#1}}$
$\newcommand{\Pb}{\mathbb{P}}$
$\newcommand{\dn}{\partial\_{\mathbf{n}}}$
$\newcommand{\Lo}{L^2(\Omega)}$
$\newcommand{\Ho}{H^1(\Omega)}$
$\newcommand{\dsp}{\displaystyle}$
$\newcommand{\uh}{u\_h}$
$\newcommand{\eh}{e\_h}$
$\newcommand{\norm}[1]{\left\\|#1\right\\|}$
$\newcommand{\normL}[1]{\norm{#1}\_{\Lo}}$
$\newcommand{\normH}[1]{\norm{#1}\_{\Ho}}$


Une fois votre programme fonctionnel, ne vous arrêtez pas à "ça marche youpi". Non. Le temps de l'analyse est arrivée ! Préparez vos machines, ça va mouliner sévère, voici quelques idées de résultats que vous pouvez regarder, mais soyez imaginatif que diable !

## Simulations numériques

1. Augmentez le nombre d'onde $k$. Attention, n'oubliez pas que le maillage doit lui aussi devenir de plus en plus fin. Quel est le nombre d'onde maximal que vous avez pu faire tourner ?
2. Ondes de Herglotz : plutôt qu'une seule onde plane, prenez comme onde incidente une somme d'ondes planes avec un poids particuliers pour chacune d'elles. Ce type d'onde est appelée Onde de Herglotz


## Analyse du code

1. Profiling de code : quelle opération prend le plus de temps CPU ? De mémoire ?
2. Vitesse du programme en fonction du nombre d'inconnues
3. Analyse de l'erreur en norme $L^2$
4. Pourquoi avez-vous fait tel ou tel choix ?

## Améliorations

1. Que pourrions nous modifier pour accélérer le code ?
2. Que faudrait il modifier pour faire du P2 ? P3 ? ...


## Astuces

1. Apportez des résultats de simulation **complets** avec vous : image du résultat, paramètres utilisés, ...
2. Soyez active/actif : "j'ai fait ci en utilisant ça, regardez comme je suis un boss"
3. Préparez un (voire des) exemple(s) *ready-to-use* : je n'aurais qu'à faire (par exemple) :

        gmsh example.geo -2    # Génération du maillage
        python example.py      # Résolution
        paraview example.vtu   # Affichage
Et boum, ça me génére le maillage, résout le problème, et me l'affiche. Bien entendu, dans ce cas, il faut que le problème soit rapide à résoudre. Si ma machine tombe en rade, ça va probablement me déplaire...
4. N'oubliez pas le fichier README ! Il devrait par exemple m'expliquer comment lancer un exemple simple !
5. Préparez plusieurs exemples tout prêt, qui peuvent être (pas obligé !) rangés dans des dossiers. Le jour J, vous ne disposez que de 10 minutes pour me présenter votre travail. Rappelez-vous que **je ne peux juger votre travail que sur ce que vous me montrez**...
6. Last but not least: **versionnez vos codes avec git**!