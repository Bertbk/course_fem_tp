+++
title = "Gestion du maillage"

date = 2018-09-09T00:00:00
# lastmod = 2018-11-21T:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.
math = true

weight = 140
diagram = true

edit_page = {repo_url = "https://github.com/Bertbk/fem_tp", repo_branch = "master", submodule_dir="content/course/fem_tp/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_fem_tp"
  submodule_dir = "content/course/fem_tp/"

# Add menu entry to sidebar.
[menu.fem_tp]
  parent = "2. Solveur FEM"
  name = "Gestion du Maillage"
  weight = 50

+++


## Une classe par élément

Nous proposons de construire 3 classes : `Point`, `Segment` et `Triangle` représentant un élément de type point, segment et triangle. La première étape consiste à transcrire les informations du maillage dans notre structure de données. Nous pouvons aussi ne pas passer par cette étape et n'utiliser que GMSH, cependant, cela nous donnera un meilleur contrôle par la suite.


La classe `Point` contient les coordonnées du point et un identifiant (0,1,2,3, ...), son indice globale, qui correspondra en P1 à la ligne dans la matrice du degré de liberté (DOF). Les classes `Segment` et `Triangle` ont pour paramètres :

- Un identifiant (0,1,2,3...)
- Une liste orientée de `Point` qui le définissent
- Un tag `Physical` (ou -1 sinon)

L'identifiant n'a pas besoin d'être le même que dans GMSH, il est même plus logique que les identifiants soient consécutifs et incrémenté à chaque nouvelle instance (0, 1, 2, ...). Le tag `Physical` doit en revanche être le même que GMSH pour éviter toute confusion. Vous êtes évidemment libre d'ajouter des paramètres et des méthodes à ces classes ! 

Par ailleurs, les 2 classes d'élément `Segment` et `Triangle`, disposeront au moins des méthodes suivantes :

- `area()` : calcule l'aire de l'élément (pour un segment, sa taille ; peut être stockée dans un paramètre et être calculé une fois pour toute)
- `jac()` : calcule son jacobien (son aire pour un segment, 2 fois l'aire pour un triangle)



## Une classe pour les gouverner toutes...

Il peut être assez malin de construire une classe `Mesh` qui représente le maillage et permettra d'effectuer des recherches d'éléments. Cette classe aura pour paramètre trois listes : une liste de `Point`, une de `Segment` et une de `Triangle`. De plus, nous lui ajoutons deux méthodes :

- `Mesh.getElements(dim, physical_tag)` : retourne une liste de tous les éléments ayant la dimension `dim` (=1 pour segment, 2 pour triangle) et le tag physique `physical_tag`
- `Mesh.getPoints(dim, physical_tag)` : retourne uniquement les points du domaine de dimension `dim` et de label physique `physical_tag`

## ...Et dans les ténébres les lier (à GMSH)

Le maillage étant construit avec GMSH, il s'agit maintenant de convertir les données issues de GMSH dans notre structure. Nous supposons ici que le maillage est déjà chargé en mémoire, soit parce qu'il vient d'être construit par GMSH (via `gmsh.model.mesh`) soit parce qu'il a été lu sur disque (via `gmsh.merge("fichier.msh")`). Nous proposons que le constructeur de `Mesh` soit vide (ne construit rien) et que l'instance de `Mesh` soit construite à l'aide d'une méthode `GmshToMesh(string filename)`, qui lit les données GMSH et construit les `Point`, `Segment` et `Triangle` et les ajoute dans les listes correspondent à `Mesh`. L'argument `filename` peut être optionnel si le maillage est déjà construit.

{{< diagram >}}
classDiagram
    class Mesh{
          +Point[ ] points
          +Segment[ ] segments
          +Triangles[ ] triangles
          +int Npts, Nseg, Ntri
          __init__(self)
          __str__(self)
          GmshToMesh(self, filename)
          getElements(self, dim, physical_tag)
          getPoints(self, dim, physical_tag)
      }
      class Point{
          +int id
          +float x, y
          -static int N
          -static name="Point"
          __init__(self, x, y, id)
      }
      class Segment{
          int id
          int physical_tag
          Point[ ] p
          -static int N
          -static name="Segment"
          __init__(self, Point[], id)
          area(self)
          jac(self)
      }
      class Triangle{
          int id
          int physical_tag
          Point[ ] p
          -static int N
          -static name="Triangle"
          __init__(self, Point[], id)
          area(self)
          jac(self)
      }
      Point <.. Mesh
      Segment >.. Mesh
      Triangle <.. Mesh
{{< /diagram>}}

{{< alert tips >}}
La méthode la plus délicate à construire est `GmshToMesh`. Pour vous aider un petit peu, n'hésitez pas à fouiller dans l'API de GMSH :

- `gmsh.model.mesh.getNodes()` : retourne tous les noeuds
- `gmsh.model.getPhysicalGroups()` : retourne tous les groupes physiques avec leurs dimension et tag
- `gmsh.model.getEntitiesForPhysicalGroup(dim, physical_tag)` : retourne toutes les entités d'un groupe physique
- `gmsh.model.mesh.getElements(dim, tag)` : retourne un élément particulier
L'algorithme ressembl surement à ceci
```
// Création des points
For every Nodes
  Point(...)
End
//Création des éléments
For every Physical Entity
  For every Entity
    For every Element

```

{{< /alert >}}