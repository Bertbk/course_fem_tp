+++
title = "2019 - 2020 : Projet / Rendu"

date = 2018-09-09T00:00:00
# lastmod = 2018-11-21T:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.
math = true

weight = 1000
diagram = false

edit_page = {repo_url = "https://github.com/Bertbk/fem_tp", repo_branch = "master", submodule_dir="content/course/fem_tp/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_fem_tp"
  submodule_dir = "content/course/fem_tp/"

# Add menu entry to sidebar.
[menu.fem_tp]
  parent = "3. Projet / Rendu" 
  name = "Année Univ. 2019 - 2020 "
  weight = 1000

+++

\begin{equation}
\label{eq:problErr}
\left\\{\begin{array}{r c l l}
-\Delta u + u &=& f & (\Omega)\\\\\\
u &=&0&(\partial\Omega)
\end{array}\right.
\end{equation}
$\Omega$ est le carré unité $]0,1[\times]0,1[$ et $f(x,y) = (1+2\pi^2)\sin(\pi x)\sin(\pi y)$.


Les TPs/Projet peuvent être réalisés en binôme.

1. **Programmez** un code éléments finis P1
2. **Preparez** un script qui résout le problème \eqref{eq:problErr} et affiche la solution
3. **Preparez** un second script qui, **pour différents pas de maillage**, calcule l'erreur en norme $L^2$ entre la solution exacte et la solution approchée pour le problème \eqref{eq:problErr}. Affichez ensuite la courbe de l'erreur en fonction de $h$ en échelle log-log. Calculez ensuite la pente de la courbe et déduisez-en la vitesse de convergence par rapport au pas de maillage ($h$). Sauvegardez par ailleurs une copie de la courbe en format données (JSON ou autre) ou image (`PNG` par exemple, pas de `JPG` nous ne sommes pas des sauvages !). 
4. **Ajoutez** à votre code un court fichier `README.md` facilitant sa compréhension, répondant notamment aux questions : "comment lance-t-on vos programmes ?" et "que doit-on obtenir ?" (exemple : "*Exécutez 'main.py' et vous devez obtenir la même image que 'solution.png' qui résout le problème méga compliqué numéro 2*")

