+++
title = "Matrices de Masse et de Rigidité"

date = 2018-09-09T00:00:00
# lastmod = 2018-11-21T:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.
math = true

weight = 150
diagram = false

edit_page = {repo_url = "https://github.com/Bertbk/fem_tp", repo_branch = "master", submodule_dir="content/course/fem_tp/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_fem_tp"
  submodule_dir = "content/course/fem_tp/"

# Add menu entry to sidebar.
[menu.fem_tp]
  parent = "2. Solveur FEM"
  name = "Matrices de Masse et de Rigidité"
  weight = 60

+++

## [Rappel] Algorithme d'assemblage

Plutôt que de calculer les coefficients un à un de la matrice $A$, [l'algorithme d'assemblage](http://bthierry.pages.math.cnrs.fr/course/fem/implementation_assemblage/#algorithme-dassemblage) propose de parcours chaque élément et d'ajouter leur contribution élémentaire à la grande matrice. Avec notre notation en `Triplets` cela donne :

```bash
Triplets t;
For p = 1, ... Nt  // Parcours des Triangles
  Mp = MatElem(p); // Matrice Elementaire du triangle p
  For i = 1,2,3
    I = Loc2Glob(p, i);
    For j = 1,2,3
      J = Loc2Glob(p,j);
      t.append(I, J, Mp(i,j)); // contribution élémentaire
    End
End
```

Si les élements parcourus sont ne sont pas des triangles à 3 points (segments, tétrahèdres, ...), il suffit d'adapter le pseudo-code ci-desssus.

Nous devons donc implémenter les calculs des matrices de masse et de rigidité élémentaire pour chaque élément.

## Matrices de masse élémentaires

Construisez une fonction `mass_elem` prenant en argument un `Segment` ou un `Triangle`, un `Triplets` et un scalaire optionnel :
```python
# element = Segment ou Triangle ; triplets = Triplets ; alpha un scalaire optionnel
def mass_elem(element, triplets, alpha =1.):
    # ...
    # return triplets
```

Cette fonction calcule [les coefficients de la matrice élémentaire](http://bthierry.pages.math.cnrs.fr/course/fem/implementation_matrices_elementaires/) de l'élément (selon son type) et les ajoute à `triplets`.

{{% callout note%}}
Pour un élément donné, son type (`Segment` ou `Triangle`) est donné par son paramètre `name`. 
{{% /callout %}}

## Matrice de masse globale

Nous proposons de construire une fonction qui calcule toutes les contributions élémentaires de la matrice de masse d'un domaine de tag `Physical` et de dimension `dim` issue d'un maillage `msh`. Les coefficients partiels seront ajoutés sous forme de `Triplet` dans une liste envoyée en argument. Nous séparons pour le moment les calculs de la matrice de masse de ceux de la matrice de rigidité :

```python
# msh = Mesh, dim = int, physical_tag = int, triplets = Triplets
def Mass(msh, dim, physical_tag, triplets):
    #...
```

{{% callout exercise %}}
Au boulot ! Assurez vous que la matrice de masse globale $M$ associée au domaine $\Omega$ vérifie la relation suivante
$$
U^T M.U = |\Omega|, \qquad U = [1, 1, 1, \ldots, 1]^T.
$$
{{% /callout%}}

## Matrice de Rigidité

Pour les matrices de rigidité, il faut calculer des quantités supplémentaires, comme la matrice $B\_p$ ou les gradients des fonctions de forme, par exemple :

```python
def gradPhi(element, i:int):
    # ...
```

{{% callout exercise %}}
Ajoutez les fonctionnalités dans votre code permettant de calculer les contributions élémentaires des matrices de rigidité puis la matrice globale. 

Vérifiez que votre matrice de rigidité $K$ satisfait bien la relation suivante :
$$
K U = 0, \qquad U = [1, 1, 1, \ldots, 1]^T.
$$
{{% /callout %}}
