+++
title = "TP3 : Fonctions de forme et matrice de masse"

date = 2018-09-09T00:00:00
# lastmod = 2018-11-21T:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math = true


weight = 120
diagram = false
#markup = "mmark"

edit_page = {repo_url = "https://github.com/Bertbk/fem_tp", repo_branch = "master", submodule_dir="content/course/fem_tp/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_fem_tp"
  submodule_dir = "content/course/fem_tp/"

# Add menu entry to sidebar.
[menu.fem_tp]
  parent = "TPs"
  identifier = "tp3"
  name = "TP 3"
  weight = 30

+++


## Fonction de Forme

Le but est d'afficher une fonction de forme $\mathbb{P}^1-$Lagrange, c'est à dire une fonction $\varphi\_{IJ}$ de $\mathbb{P}^1-$ qui vaut 0 sur tous les sommets $J\neq I$ du maillage sauf un, $I$, pour lequelle la fonction prend la valeur 1 :
$$
\forall I,J = 0,\ldots, N\_s-1,\qquad \varphi\_I(\mathbf{s}\_J) = \delta\_{IJ}
$$

{{% alert exercise %}}

À l'aide de l'API Python de GMSH :

- Générez un carré unitaire avec un pas de maillage de 0.25
- Appliquez des labels `Physical`: un pour la surface et un pour son bord
- Générez le maillage 2D
- Construisez 3 [`numpy array`](https://numpy.org/) `Phi`, `X` et `Y` unidimensionnel et de taille le nombre de sommets $N\_s$ du maillage tels que:
  - `Phi` est le vecteur nul sauf en un coefficient où `Phi[I] = 1` (choisissez `I`) 
  - `X` et `Y` sont respectivement les coordonnées x et y des points du maillage (voir ci-dessous)
- Affichez le tout à l'aide de [Matplotlib et de la projection 3D](https://matplotlib.org/3.1.1/gallery/mplot3d/trisurf3d.html)

{{% /alert %}}

Pour obtenir les coordonnées des points d'un groupe `Physical` donné, vous pouvez utilisez `gmsh.model.mesh.getNodesForPhysicalGroup(dim, tag)` (voir [gmsh.py](https://gitlab.onelab.info/gmsh/gmsh/blob/master/api/gmsh.py)):

```python
def getNodesForPhysicalGroup(dim, tag):
  """
  Get the nodes from all the elements belonging to the physical group of
              dimension `dim' and tag `tag'. `nodeTags' contains the node tags; `coord'
              is a vector of length 3 times the length of `nodeTags' that contains the x,
              y, z coordinates of the nodes, concatenated: [n1x, n1y, n1z, n2x, ...].

              Return `nodeTags', `coord'.
  """
```

{{% alert warning %}}
 
- GMSH commence la numérotation des sommets à 1
- La liste retournée par `getNodesForPhysicalGroup` n'est pas triée
{{% /alert %}}

## Interpolation P1

Prenons une fonction f définie sur $\Omega$. Une interpolation possible de f sur l'espace $\mathbb{P}^1$ est la fonction $\Pi\_hf$ telle que $\Pi\_hf(\mathbf{s}) = f(\mathbf{s})$ pour chaque sommet $\mathbf{s}$ du maillage.

{{% alert exercise %}}

Construisez l'interpollée $\Pi\_hf$ de la fonction $f(x,y) = \sin(\pi x)\sin(\pi y)$
{{% /alert %}}

## Aire d'un triangle

{{% alert exercise %}}
Construisez une fonction prenant un triangle en argument (*e.g.* les coordonnées des sommets) et qui retourne son aire.
{{% /alert %}}


## Matrice de Masse

La matrice de masse $M$ représente l'identité dans la base des fonctions de forme $\mathbb{P}^1-$Lagrange :
$$
M(I,J) = \int\_{\Omega}\varphi\_J(x)\varphi\_I(x) dx
$$
L'algorithme efficace permettant de construire cette matrice est l'algorithme d'assemblage suivant :

```
M = 0; // Matrice nulle
For p = 1:N_triangles
  For i=1:3 //3 = N_s
    I = Loc2Glob(p, i)$ // Indice globale du i-ème sommet
    For j=1:3 //3 = N_s
      J = Loc2Glob(p, j)$ // Indice globale de j-ème sommet
      M(I,J) += M^e_{p}(i,j) // Matrice de masse élémentaire du triangle T_p
    EndFor
  EndFor
EndFor
```
La fonction `Loc2Glob(p,i)` retourne l'indice global $I$ du `i`$^{ème}$ sommet du triangle $T\_p$. La matrice de masse élémentaire $M^e\_p$(=`M^e_{p}`) du triangle $T\_p$ est donnée par ($|T\_p|$ = aire du triangle $T\_p$) :
$$
M^e\_p = \frac{|T\_p|}{12}\begin{pmatrix}
2 & 1 & 1 \\\\\\
1 & 2 & 1 \\\\\\
1 & 1 & 2
\end{pmatrix}
$$

{{% alert exercise %}}
Construisez une fonction permettant de construire la matrice de masse du maillage, en prenant en compte les informations suivantes :

- Utilisez [`numpy.ndarray`](https://docs.scipy.org/doc/scipy/reference/tutorial/linalg.html#numpy-matrix-vs-2d-numpy-ndarray) plutôt que  `numpy.matrix`.
- La matrice $M$ doit vérifier la relation suivante, pour $U = [1,1,\ldots,1]^T$ : $U^T M U = \left|\Omega\right|$
{{% /alert %}}

## Matrice creuse

La matrice de masse est creuse, une (très) bonne idée est d'exploiter cette propriété ! Cela permet un gain en mémoire et en CPU (pour les produits matrice-vecteur).

{{% alert exercise %}}

Modifiez l'algorithme d'assemblage pour :

- Utiliser le format COO ([`coo_matrix` dans SciPy](https://docs.scipy.org/doc/scipy/reference/generated/scipy.sparse.coo_matrix.html)) plutôt que le stockage dense (`array`)
- Une fois la matrice COO construite, transformez la en matrice CSR ([`csr_matrix`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.sparse.csr_matrix.html))

{{% /alert %}}
