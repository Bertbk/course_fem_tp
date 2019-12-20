+++
title = "Solveur et Dirichlet"

date = 2018-09-09T00:00:00
# lastmod = 2018-11-21T:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.
math = true

weight = 190
diagram = false

edit_page = {repo_url = "https://github.com/Bertbk/fem_tp", repo_branch = "master", submodule_dir="content/course/fem_tp/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_fem_tp"
  submodule_dir = "content/course/fem_tp/"

# Add menu entry to sidebar.
[menu.fem_tp]
  parent = "2. Solveur FEM"
  name = "Solveur et Dirichlet"
  weight = 90

+++

## Problème de référence

Résumons ici l'utilisation de notre programme éléments finis sur le problème suivant :

$$
\left\\{
\begin{array}{r c l l}
-\Delta u & = & f & (\Omega)\\\\\\
u & = & 0 & (\partial \Omega)
\end{array}
\right.
$$
La formulation variationnelle est donnée par
$$
\left\\{
\begin{array}{l}
\text{Trouver }u\in H^1\_0(\Omega) \text{ tel que }\\\\\\
\displaystyle \forall v \in H^1\_0(\Omega), \qquad \int\_{\Omega} \nabla u\cdot\nabla v = \int\_{\Omega}fv
\end{array}
\right.
$$
Pour simplifier nous prenons $\Omega = ]0,1[\times]0,1[$ le carré unitaire et $f(x,y) = \sin(\pi x)\sin(\pi y)$
de sorte que la solution exacte est connue et vaut
$$
u(x, y) = \frac{1}{2\pi^2}\sin(\pi x)\sin(\pi y).
$$

## Résolution

Dans notre programme, cela reviendra à écrire quelque chose comme

```python
# definition du maillage
# ...
def f(x,y):
  return np.sin(np.pi*x)*np.sin(np.pi*y)
# Construction Matrice de mass
Triplet t
Mesh.Stiffness(2, 10, t) # idem
# Vecteur
Ns = #... Nombre de sommets
B = np.array((Ns,))
Mesh.Quadrature(2, 10, f, B)
# Matrice CSR à partir des triplets
A = coo_matrix(t.data).tocsr()
# Prise en compte des conditions de Dirichlet
# ... Cf en dessous
# Résolution
U = ...
# Visualisation
# ... matplotlib ou paraview
```

## Conditions de Dirichlet

Pour prendre en compte les éventuelles condition de Dirichlet, nous avons besoin d'une fonction de prototype suivant
```python
def Dirichlet(dim, physical_tag, g, A, B):
```
Cette fonction prend comme argument la matrice CSR `A` du système linéaire, le vecteur `B` et une fonction `g` et force la condition de Dirichlet $u=g$ sur le domaine de dimension `dim` et de tag `Physical` `physical_tag`. La méthode utilisée pour forcer cette condition est [celle vue en cours](http://bthierry.pages.math.cnrs.fr/course/fem/condition_dirichlet/#condition-de-dirichlet-non-homogne).

{{% alert note %}}
A cause de cette méthode, certains coefficients du stockage CSR seront nuls mais conservés en mémoire malgré tout : tant pis !
{{% /alert %}}
