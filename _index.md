+++
title = "MEF : TPs"

date = 2018-09-09T00:00:00
# lastmod = 2018-09-09T00:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.

math = true
weight = 1
diagram = false
#markup = "mmark"

edit_page = {repo_url = "https://github.com/Bertbk/fem_tp", repo_branch = "master", submodule_dir="content/course/fem_tp/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_fem_tp"
  submodule_dir = "content/course/fem_tp/"


# Add menu entry to sidebar.
[menu.fem_tp]
  name = "MEF : TPs"
  weight = 1

+++

Les TP associés au cours Maillage et Éléments Finis sont divisés en deux parties. Une première partie consiste à utiliser des logiciels open-source afin de résoudre un problème complet à l'aide de la méthode des éléments finis : de la création de la géométrie à la visualisation de la solution. La deuxième partie vous propose d'implémenter vous même un (petit) assembleur éléments finis en Python.

## Outils utilisés (Open Source power)

1. [GMSH](https://gmsh.info), dont un [tutoriel est disponible sur le site]({{<ref "/tutorial/gmsh/_index.md">}})
2. [Python](https://www.python.org/) (que vous maîtrisez) ou [Julia](https://julialang.org/)
3. [Paraview](https://www.paraview.org) que vous êtes en train de maîtriser

## Sur les machines de Polytech...

### GMSH

Installé et exécutable depuis le terminal
```
gmsh
```
et (mieux !) utilisable depuis la SDK et Python
