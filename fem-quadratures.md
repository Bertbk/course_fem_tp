+++
title = "Quadratures"

date = 2018-09-09T00:00:00
# lastmod = 2018-11-21T:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.
math = true

weight = 160
diagram = false

edit_page = {repo_url = "https://github.com/Bertbk/fem_tp", repo_branch = "master", submodule_dir="content/course/fem_tp/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_fem_tp"
  submodule_dir = "content/course/fem_tp/"

# Add menu entry to sidebar.
[menu.fem_tp]
  parent = "2. Solveur FEM"
  name = "Quadratures"
  weight = 70

+++

## Rappel

Certaines intégrales ne peuvent être calculées analytiquement et devront être approchées numériquement via [des règles de quadrature](http://bthierry.pages.math.cnrs.fr/course/fem/implementation_matrices_elementaires/#quadratures). Prenons pour exemple d'une fonction $f$ quelconque mais connue, le second membre $B$ (un vecteur) sera alors de la forme suivante 
\begin{equation}
\label{eq:B}
B[I] = \int\_{\Omega} f(\mathbf{x}) \varphi\_I(\mathbf{x})\\;\mathrm{d}\mathbf{x}
= \sum\_{T\_p}\sum\_{i} \int\_{T\_p} f(\mathbf{x}) \varphi\_i^p(\mathbf{x})\\;\mathrm{d}\mathbf{x}
\approx \sum\_{T\_p} \sum\_{i}\sum\_m \omega\_m f(\mathbf{x}(\xi\_m, \eta\_m)) \hat{\varphi}\_i(\xi\_m, \eta\_m)
\end{equation}
Les poids $\omega\_m$ et les points de quadrature $(\xi\_m, \eta\_m)$ dépendent de la précision recherchée. Rappelons aussi que $\mathbf{x}(\xi\_m, \eta\_m)$ s'obtient par les fonctions d'interpolation géométrique, qui dans le cas d'éléments finis isoparamétriques, sont les mêmes que les fonctions éléments finis $\mathbb{P}^1$, c'est à dire que pour $\mathbf{x}$ appartenant à un élément de sommets $(\mathbf{s}\_i)_i$ :
\begin{equation}
\label{eq:param}
\mathbf{x}(\xi\_m, \eta\_m) = \sum\_{i=1} \hat{\psi}\_i(\xi\_m, \eta\_m)\mathbf{s}\_i,
\end{equation}

Les fonctions $\hat{\psi}$ sont égales au fonctions de forme $P1$-Lagrange. Cependant, les fonction $\hat{\psi}\_i$ sont des fonctions d'interpolation *géométriques* tandis que les fonctions de forme $\hat{\varphi}\_i$ sont des fonctions d'interpolation de la solution.

## Méthodes et fonctions utiles

Pour chaque type d'élément, `Triangle` ou `Segment`, nous avons besoin de méthodes permettant d'obtenir les quantités suivantes: :

1. Les poids des points de quadrature
2. Les coordonnées paramétriques des points de quadrature
3. Les coordonnées physiques des points de quadrature
4. Les valeurs des fonctions de forme de référence $\hat{\varphi}$ sur des coordonnées paramétriques

Remarquez que les points 1, 2 ne dépendent que du type de l'élément considéré.

{{% callout exercise %}}
Afin de bien compartimenter chaque fonctionnalité, nous proposons :

- Ajouter aux classes `Triangle` et `Segment` la méthode `def gaussPoint(self,order=2):` qui retourne, dans le format de votre choix, les poids, les coordonnées paramétriques et les coordonnées physiques des points de Gauss de l'élement considéré et pour une précision `order`. Vous aurez sans doute besoin de méthodes intermédiaires pour calculer, par exemple les $\hat{\psi}\_i(\xi,\eta)$.
- Ajouter une fonction `def phiRef(element, i:int, param:[float]):` qui calcule $\hat{\varphi}\_i(\xi,\eta)$ sur un élément `Segment` ou `Triangle`. L'argument `param` est une liste des coordonnées paramétriques ($\xi,\eta$ pour un triangle, $s$ pour un segment))
{{% /callout %}}


## Intégrale

Construisez maintenant une fonction de prototype suivant
```python
def Integrale(msh:Mesh, dim:int, physical_tag:int, f, B:np.array, order=2):
```
Cette fonction calcul l'intégrale de $f \varphi\_I$ sur le domaine de tag physique `physical_tag` et de dimension `dim`. Le résultat est alors **ajouté** dans le `B[I]` (voir \eqref{eq:B}). L'argument `f` sera une fonction décrite par l'utilisatrice/utilisateur, elle prendra 2 arguments, `x` et `y` et retournera un scalaire correspondant à $f(x,y)$.