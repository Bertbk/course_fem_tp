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

Plutôt que de calculer les coefficients un à un de la matrice $A$, [l'algorithme d'assemblage](http://bthierry.pages.math.cnrs.fr/course/fem/implementation_assemblage/#algorithme-dassemblage) propose de parcours chaque élément et d'ajouter leur contribution élémentaire à la grande matrice. Avec notre notation en `Triplet` cela donne :

```bash
Triplet t;
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

Les 2 classes d'élément `Segment` et `Triangle`, disposeront au moins des méthodes suivantes :

- `area()` : calcule l'aire de l'élément (pour un segment, sa taille ; peut être stockée dans un paramètre et être calculé une fois pour toute)
- `mass(Triplet t)` :  ajoute à `t` (avec `t.append` défini avant) les triplets de coefficients de la matrice de [masse élémentaire associée à cet élément](http://bthierry.pages.math.cnrs.fr/course/fem/implementation_matrices_elementaires/)
- `stiffness(Triplet t)` :  idem pour la rigidité
- 
{{% alert exercise %}}
Pour le moment, implémentez uniquement les méthode `area()` et `mass(Triplet t)`. Bien entendu, validez le code, par exemple sur un triangle tout simple comme celui de référence.
{{% /alert%}}

{{% alert note %}}
Si nous n'avons toujours pas vu en cours comment calculer la matrice de masse (et de rigidité) d'un segment, contentez vous alors des triangles pour moment.
{{% /alert %}}

## Matrice de masse globale

Nous proposons de construire une fonction qui calcule toutes les contributions élémentaires de la matrice de masse d'un domaine de tag `Physical` et de dimension `dim`. Les coefficients partiels seront ajoutés sous forme de `Triplet` dans une liste envoyée en argument. Nous séparons pour le moment les calculs de la matrice de masse de ceux de la matrice de rigidité. 

Nous pouvons construire cette fonction hors de toute classe, ou comme méthode de la classe `Mesh`, ce que nous proposons de faire. Cette méthode aura le prototype et de pseudo code suivant :

```bash
Mesh.Mass(int dim, int physical_tag, Triplet t)
{
  L = getElements(dim, physical_tag); // Retourne la liste des éléments adéquats
  For element in L
    element.mass(t) // contributions élémentaires ajoutées dans t
  End
}
```

{{% alert exercise %}}
Au boulot. Assurez vous que la matrice de masse globale $M$ associée au domaine $\Omega$ vérifie la relation suivante
$$
U^T M.U = |\Omega|, \qquad U = [1, 1, 1, \ldots, 1]^T.
$$
{{% /alert%}}

{{% alert note %}}
Si cette forme d'implémentation ne vous plait pas, libre à vous de la construire d'une autre manière. En revanche, faites en sorte de pouvoir calculer la matrice de masse d'un domaine `Physical` donné.
{{% /alert %}}

## Matrice de Rigidité

Pour les matrices de rigidité, il faut calculer des quantités supplémentaires, comme la matrice $B\_p$ ou les gradients des fonctions de forme. Outre ces petits désagréments, la méthode reste similaire.

{{% alert exercise %}}
Ajoutez les fonctionnalités dans votre code permettant de calculer les contributions élémentaires des matrices de rigidité puis la matrice globale. Vous aurez sans doute besoin d'ajouter une méthode calculant le gradient d'une fonction de forme.

Vérifiez que votre matrice de rigidité $K$ satisfait bien la relation suivante :
$$
K U = 0, \qquad U = [1, 1, 1, \ldots, 1]^T.
$$
{{% /alert %}}