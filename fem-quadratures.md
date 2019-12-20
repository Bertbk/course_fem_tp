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
\mathbf{x}(\xi\_m, \eta\_m) = \sum\_{i=1} \hat{\varphi}\_i(\xi\_m, \eta\_m)\mathbf{s}\_i,
\end{equation}

## Élémentaire

Pour chaque type d'élément, triangle et segment, nous aurons besoin des méthodes permettant d'obtenir les quantités suivantes: :

1. Les poids des points de quadrature
2. Les coordonnées paramétriques des points de quadrature
3. Les valeurs des fonctions de forme $\hat{\varphi}$ sur chaque point de quadrature
4. Calculer les coordonnées physiques d'un point à partir de ses coordonnées paramétriques $(\xi\_m, \eta\_m)$, autrement dit, calculer la formule \eqref{eq:param}.

Remarquez que les 3 premiers points sont indépendants de l'élément considéré et ne dépendent que du type de l'élément considéré.

{{% alert exercise %}}
Pour chacun type d'élémént, triangle et segment, implémentez une méthode prenant en argument une fonction `f` et qui ajoute les contributions dans un `np.array` B:
```bash
Triangle.Quadrature(f, int degre, np.array B)
{
  For i = 1, 2...
    I = Loc2Glob(i)
    For m= 1,2...
      B(I) += ...
    End
  End
}
```
{{% /alert %}}

## Globale

Dans la classe `Mesh`, construisez une méthode de prototype suivant
```
Mesh.Quadrature(int dim, int physical_tag, f, np.array B)
```
Cette méthode ajoute dans le vecteur `np.array B`, à l'indice `I`, la quantité décrite par \eqref{eq:B}. L'argument `f` sera une fonction décrite par l'utilisatrice/utilisateur, elle prendra 2 arguments, `x` et `y` et retournera un scalaire correspondant à $f(x,y)$.