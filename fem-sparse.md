+++
title = "Matrices Creuses"

date = 2018-09-09T00:00:00
# lastmod = 2018-11-21T:00:00

draft = false  # Is this a draft? true/false
toc = true  # Show table of contents? true/false
type = "docs"  # Do not modify.
math = true

weight = 130
diagram = false

edit_page = {repo_url = "https://github.com/Bertbk/fem_tp", repo_branch = "master", submodule_dir="content/course/fem_tp/"}

[git]
  icon = "github"
  repo = "https://github.com/Bertbk/course_fem_tp"
  submodule_dir = "content/course/fem_tp/"

# Add menu entry to sidebar.
[menu.fem_tp]
  parent = "2. Solveur FEM"
  identifier = "tp4"
  name = "Matrices Creuses"
  weight = 40

+++

Le but maintenant est d'implémenter la méthode des éléments finis P1 en 2D, autrement dit, à un maillage donné, de calculer :

- Les matrices de masse 2D (union de triangles) et 1D (union de segments)
- Les matrices de rigité (union de triangles) et 1D (union de segments)
- Appliquer les conditions de Dirichlet éventuelles
- Approcher des intégrales par des quadratures adaptées

La matrice du système finale sera stockée sous le format COOrdinate. L'algorithme d'assemblage se prête particulièrement bien au stockage COO puisque la redondance des coefficients est autorisée par Scipy. Autrement dit, les fonctions calculant les matrices de masse et de rigidité retourneront des triplets de type `[I, J, Valeur]` où `I` est l'indice ligne, `J` l'indice colonne et `Valeur` le coefficient (potentiellement partiel).

Avant de résoudre le système, la matrice sera transformée au format Compressed Storage Raw (CSR).

## Matrice COO

### Rappels

Le format COO (_COOrdinates_) propose de ne stocker que les coefficients non nuls d'une matrice `A` sous la forme de 2 listes d'entiers `row` et `col` et une liste de réels `val`, toutes trois de même taille, telles que

```python
A[row[i], col[i]] = val[i]
```

Les trois listes peuvent aussi être combinées en une seule liste `data` de triplets de type `[int, int, double]` et telle que :

```python
data[i] = [row[i], col[i], val[i]]
```

Le format COO est assez permissif :

- Redondance : si deux triplets possèdent les mêmes indices ligne et colonne alors le coefficient de la matrice, associé à ces indices ligne et colonne, sera obtenu en sommant les valeurs de ces triplets.
- Pas d'ordre : aucune nécessité d'ordonner les triplets selon les lignes ou les colonnes

Par exemple, les deux jeux de listes ci-dessous décrivent la même maitrce, la seule différence est que le coefficient en (2,2) est scindé en deux et le premier et le dernier triplet ont été permutés :

| `i` | 0   | 1   | 2   | 3   | 4   | 5   | 6   |
| ----- | --- | --- | --- | --- | --- | --- | --- |
| `row[i]`   | 0   | 0   | 1   | 2   | 3   | 3   | 3   |
| `col[i]`   | 0   | 3   | 1   | 2   | 0   | 1   | 3   |
| `val[i]`   | 1.1 | 2   | 1   | 2.3 | 0.5 | 2   | 2   |

| `i` | 0   | 1   | 2   | 3   | 4     | 5   | 6   | 7 |
| ----- | --- | --- | --- | --- | --- | --- | --- | ---| 
| `row[i]`   | 3   | 0   | 1   | **2**     | **2** | 3   | 3   | 0   |
| `col[i]`   | 3   | 3   | 1   | **2**     | **2** | 0   | 1   | 0   |
| `val[i]`   | 2 | 2   | 1   | **1.3** | **1** | 0.5 | 2   | 1.1   |

### Scipy

Pour construire une [matrice COO dans Scipy](https://docs.scipy.org/doc/scipy/reference/generated/scipy.sparse.coo_matrix.html) à partir d'un jeu de données `data`, ce dernier doit être de type `([], ([],[]))` : un `Tuple` contenant une `List` (`val`) ainsi qu'un `Tuple` contenant deux `List` (`row` et `col`) ([list vs. tuple ?](https://stackoverflow.com/questions/626759/whats-the-difference-between-lists-and-tuples)) :

```python
data = (val, (row, col))
```
Une matrice COO se construit alors ainsi
```
A = coo_matrix(data)              # si data dans le format ci-dessus
A = coo_matrix((val, (row, col))) # si row, col et val sont séparées
```
{{% alert tips %}}

Une matrice COO peut être visualisée en se transformant en `array` [avec la méthode `toarray()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.sparse.coo_matrix.html):

```python
print(A.toarray())
```

{{% /alert %}}

## Triplets

Nous proposons de construire notre future matrice par concaténation de triplets de type (I, J, valeur). une classe `Triplets` qui encapsule cette structure de données. Nous lui adjoignons une méthode `append` permettant d'ajouter un triplet au bout des autres :

```python
Triplet t;           print(t.data) # ([], ([], []))
t.append(0, 1 ,2.);  print(t.data) # ([2.], ([0], [1]))
t.append(3, 4 ,5.2); print(t.data) # ([2., 5.2], ([0, 3], [1, 4]))
```

La classe ressemble alors à cela:

```python
def class Triplet:
  def __init__():
    self.data = ([], ([], []))
  def __str__():
    return str(self.data)
  def append(self, I, J, val):
    # Ajoute le triplet [I, J, val] dans self.data
```

{{% alert exercise %}}
Construisez la classe `Triplet` et implémentez la méthode `append`. N'oubliez pas de tester votre classe.
{{% /alert %}}

{{% alert exercise %}}
Testez votre classe `Triplet` en construisant la matrice suivante (au format COO évidemment) :

$$
A = \begin{pmatrix}
1.1 & 0 & 0 & 2 \\\\\\
0 & 1 & 0 & 0  \\\\\\
0 & 0 & 2.3 & 0 \\\\\\
0.5 & 2 & 0 & 2
\end{pmatrix}
$$
{{% /alert %}}

## Format CSR

Une fois la matrice au format COO construite, nous la transformerons [au format CSR par la méthode `tocsr()`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.sparse.coo_matrix.tocsr.html#scipy.sparse.coo_matrix.tocsr):

```python
A = coo_matrix((val, (row, col))).tocsr()
```
