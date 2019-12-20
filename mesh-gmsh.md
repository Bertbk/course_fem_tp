+++
title = "Prise en main de GMSH"

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
  identifier = "maillage"
  name = "Prise en main"
  weight = 20

+++

[Tutoriel GMSH]({{<ref "tutorial/gmsh/_index.md">}}) :

- Génération de géométries simples (*e.g.* stade)
- Génération de géométries plus compliquées avec OpenCascade (*e.g.* tasse)
- Gestion des labels `Physical`
- Débuter avec l'API Python