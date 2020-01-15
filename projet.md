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

## To Do :Éléments finis P1 (15/20)

Les TPs/Projet peuvent être réalisés en binôme.

1. **Programmez** un code éléments finis P1 qui **fonctionne**, en suivant par exemple toute la partie précédente.
2. **Préparez** 2 scripts :
   1.  Le premier calcule l'erreur en norme $L^2$ entre la solution exacte et la solution approché pour le problème \eqref{eq:problErr}. La courbe doit être affichée en échelle log-log. Sauvegardez par ailleurs une copie de la courbe en format image (`PNG` par exemple, pas de `JPG` nous ne sommes pas des sauvages !)
   2. Le deuxième résout le problème \eqref{eq:problem2} et affiche la solution.
3. **Ajoutez** à votre code un court fichier `README.md` facilitant sa compréhension, répondant notamment aux questions : "comment lance-t-on vos programmes ?" et "que doit-on obtenir ?" (exemple : "*Exécutez 'main.py' et vous devez obtenir la même image que 'solution.png' qui résout le problème méga compliqué numéro 2*")


## Problèmes

### Problème de référence

\begin{equation}
\label{eq:problErr}
\left\\{\begin{array}{r c l l}
-\Delta u + u &=& f & (\Omega)\\\\\\
u &=&0&(\partial\Omega)
\end{array}\right.
\end{equation}
$\Omega$ est le carré unité $]0,1[\times]0,1[$ et $f(x,y) = (1+2\pi^2)\sin(\pi x)\sin(\pi y)$.

### Propagation d'onde

{{% alert warning %}}
Matrice et solution sont complexes !
{{% /alert %}}

En dimension 2, dessiner un cercle de rayon 1, noté $\Gamma$, entouré d'un cercle de même centre et de rayon 3, noté $\Gamma^{\infty}$. Le domaine de calcul, $\Omega$ est le domaine contenu entre $\Gamma$ et $\Gamma^{\infty}$. Nous cherchons à calculer l'onde diffractée $u$ (complexe !) par une onde incidente $u^{inc}$ sur $\Gamma$, solution de l'équation de Helmholtz :

\begin{equation}
\label{eq:problem2}
\left\\{\begin{array}{r c l l}
(\Delta + k^2) u &=& 0 & (\Omega)\\\\\\
u &=& -u^{inc}&(\Gamma)\\\\\\
\partial\_{n} u - i k u &=& 0&(\Gamma^{\infty})
\end{array}\right.
\end{equation}

Le domaine $\Omega$ contient un (joli) sous-marin (ou autre) entouré d'une ellipse. Faites en sorte que la longueur du sous-marin soit égale à 1. Le domaine de calcul, $\Omega$ est situé entre le sous-marin et l'ellipse. Le bord du sous-marin est noté $\Gamma$ et l'ellipse $\Gamma^{\infty}$. Prenez $R1$ et $R2$ suffisamment grand (mais pas trop !) et évitez de mailler le sous-marin !

Le nombre d'onde $k$ sera fixé à $2\pi$ dans un premier temps. Le pas de maillage $h$ dépend du nombre d'onde et nous choisirons $h = \frac{2\pi}{kn\_{\lambda}}$ avec $n\_{\lambda} = 15$. L'onde incidente sera plane et de direction $\alpha \in [0,2\pi]$ : $u^{inc}(x,y) =e^{ik(x\cos(\alpha) + y\sin(\alpha))}$. La quantité à afficher est le champ total physique $u\_t = u + u^{inc}$. 

En sortie, affichez la partie réelle de $u + u^{inc}$.

{{< figure src="../submarine.svg" title="Sous-marin 2D" numbered="true" width="75%">}}