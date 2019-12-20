+++
title = "GMSH : API"

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
  parent = "1. Maillage avec GMSH"
  identifier = "api"
  name = "API GMSH"
  weight = 30

+++

[Dans le tutoriel GMSH](http://bthierry.pages.math.cnrs.fr/tutorial/gmsh), intéressez vous à l'utilisation de l'API Python.

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
