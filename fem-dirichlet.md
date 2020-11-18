+++
title = "Conditions de Dirichlet"

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
  name = "Conditions de Dirichlet"
  weight = 90

+++

Pour prendre en compte les éventuelles condition de Dirichlet, nous avons besoin d'une fonction de prototype suivant
```python
def Dirichlet(msh, dim, physical_tag, g, triplets, B):
```
Cette fonction prend comme argument le `Triplets triplets` et le vecteur `B` du système linéaire et les modifie pour prendre en compte la condition de dirichlet $u=$`g` sur le domaine de dimension `dim` et de tag physique `physical_tag`. La technique utilisée pour forcer cette condition est [celle vue en cours](http://bthierry.pages.math.cnrs.fr/course/fem/condition_dirichlet/#condition-de-dirichlet-non-homogne).

Pour cela, nous parcourons les noeuds `I` du domaine de Dirichlet. Puis, dans la liste des indices ligne de `triplets`, dès qu'un occurence à `I` est obtenu, la valeur de ce triplet est mise à $0$.
Il ne faut pas oublier, à la fin, d'ajouter un triplet `(I,I,1)` correspondant au terme diagonal et de modifier le coefficient `B[I] = g(x,y)`.

{{% callout note %}}
Cette technique n'est peut être pas la plus optimale ! Mais elle a le mérite de fonctionner...
{{% /callout %}}
