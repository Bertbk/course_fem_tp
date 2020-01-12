+++
title = "Résolution"

date = 2018-09-09T00:00:00
# lastmod = 2018-11-21T:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.
math = true

weight = 200
diagram = false

edit_page = {repo_url = "https://github.com/Bertbk/fem_tp", repo_branch = "master", submodule_dir="content/course/fem_tp/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_fem_tp"
  submodule_dir = "content/course/fem_tp/"

# Add menu entry to sidebar.
[menu.fem_tp]
  parent = "2. Solveur FEM"
  name = "Résolution"
  weight = 100

+++

## Problème de référence

Résumons ici l'utilisation de notre programme éléments finis sur le problème suivant :

$$
\left\\{
\begin{array}{r c l l}
-\Delta u  + u & = & f & (\Omega)\\\\\\
u & = & 0 & (\partial \Omega)
\end{array}
\right.
$$
La formulation variationnelle est donnée par
$$
\left\\{
\begin{array}{l}
\text{Trouver }u\in H^1\_0(\Omega) \text{ tel que }\\\\\\
\displaystyle \forall v \in H^1\_0(\Omega), \qquad \int\_{\Omega} \nabla u\cdot\nabla v + \int\_{\Omega} uv = \int\_{\Omega}fv
\end{array}
\right.
$$
Pour simplifier nous prenons $\Omega = ]0,1[\times]0,1[$ le carré unitaire et $f(x,y) = (1+2\pi^2)\sin(\pi x)\sin(\pi y)$
de sorte que la solution exacte est connue et vaut
$$
u(x, y) = f(x, y).
$$

{{< figure src="../img/uref.png" title="Solution" numbered="true">}}

## Résolution

Dans notre programme, cela reviendra à écrire quelque chose comme

```python
#import ...

#Données
def g(x,y):
  return np.sin(np.pi*x)*np.sin(np.pi*y)
def f(x,y):
  return g(x,y)*(2*np.pi*np.pi +1 )
def diri(x,y):
  return 0.
#Maillage
msh = geo.mesher("mesh.msh")
# Triplets
t = common.Triplets()
fem_p1.Mass(msh, 2,10, t)
fem_p1.Stiffness(msh, 2,10, t)
b = np.zeros((msh.Npts,))
fem_p1.Integral(msh, 2, 10, f, b, 2)
fem_p1.Dirichlet(msh, t, b, 1, 1, diri)
# Résolution
A = (sparse.coo_matrix(t.data)).tocsr()
U = sparse.linalg.spsolve(A, b)

# Visualisation
x= [pt.x for pt in msh.points]
y= [pt.y for pt in msh.points]
connectivity=[]
for tri in msh.triangles:
  connectivity.append([ p.id for p in tri.p]) 

plt.tricontourf(x, y, connectivity, U, 12)
plt.colorbar()
plt.show()

### U de référence
Uref = np.zeros((msh.Npts,))
for pt in msh.points:
  I = int(pt.id)
  Uref[I] = g(pt.x, pt.y)
plt.tricontourf(x, y, connectivity, Uref, 12)
plt.colorbar()
plt.show()
```
