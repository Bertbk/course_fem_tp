+++
title = "Lecture du maillage"

date = 2018-09-09T00:00:00
# lastmod = 2018-11-21T:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.
math = true

weight = 140
diagram = false

edit_page = {repo_url = "https://github.com/Bertbk/fem_tp", repo_branch = "master", submodule_dir="content/course/fem_tp/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_fem_tp"
  submodule_dir = "content/course/fem_tp/"

# Add menu entry to sidebar.
[menu.fem_tp]
  parent = "2. Solveur FEM"
  name = "Lecture du Maillage"
  weight = 50

+++


## Une classe par élément

Nous proposons de construire 3 classes : `Point`, `Segment` et `Triangle` représentant un élément de type point, segment et triangle. La première étape consiste à transcrire les informations du maillage dans notre structure de données. Nous pouvons aussi ne pas passer par cette étape et n'utiliser que GMSH, cependant, cela nous donnera un meilleur contrôle par la suite.

MERMAID GRAPH

La classe `Point` contient les coordonnées du point et un identifiant (0,1,2,3, ...). Les classes `Segment` et `Triangle` ont pour paramètres :

- Son identifiant (0,1,2,3...)
- Ses 2 ou 3 (ou plus ?) `Point` qui le définissent, dans le bon ordre
- Son tag `Physical`

L'identifiant n'a pas besoin d'être le même que dans GMSH, mais le tag `Physical` doit l'être. Vous êtes évidemment libre d'ajouter des paramètres et des méthodes à ces classes !

## Une classe pour les gouverner toutes...

Plutôt que d'utiliser des listes de `Point`, `Segment` et `Triangle`, il est sans doute plus malin de les regrouper dans une classe `Mesh` qui aurait comme paramètres trois listes, une pour chaque élément.

{{% alert exercise %}}
Supposons qu'un maillage est déjà chargé en mémoire, c'est à dire qu'il vient d'être construit par GMSH (`gmsh.model.mesh`) ou alors il vient d'être lu sur disque (`gmsh.merge("fichier.msh")`). Après avoir implémenté les classes décrites ci-dessus :

1. Construisez une instance de la classe `Mesh`
2. Remplissez sa liste des `Point`
3. Remplissez ses listes des `Segment` et `Triangle`. Pour cela, vous pouvez parcourir les entités `Physical` puis les entités élémentaires lui appartenant et enfin les éléments de celles-ci.

{{% /alert %}}

## ...Et dans les ténébres les lier

Nous aurons enfin besoin de pouvoir extraire tous les éléments d'une partie `Physical` du maillage. Pour cela, construisez une méthode `Mesh.getElements(dim, physical_tag)` (de la classe `Mesh`) retournant une liste de tous les éléments ayant la dimension `dim` (=1 pour segment, 2 pour triangle) et le tag `Physical` `physical_tag`.