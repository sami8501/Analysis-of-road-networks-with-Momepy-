Voici un projet de fichier README pour le package Momepy, basé sur le notebook Python fourni et axé sur l'analyse des réseaux :

# Momepy - Analyse des réseaux routiers

Momepy est une bibliothèque Python puissante pour l'analyse quantitative de la forme urbaine, avec un accent particulier sur l'analyse des réseaux routiers. Ce README se concentre sur l'utilisation de Momepy pour l'analyse des réseaux, en s'appuyant sur un exemple d'étude du réseau pédestre de La Rochelle, France.

## Installation

```bash
pip install momepy
```

## Dépendances

- geopandas
- osmnx
- networkx
- matplotlib

## Fonctionnalités principales pour l'analyse des réseaux

### 1. Importation et préparation des données

Momepy s'intègre parfaitement avec OSMnx pour récupérer les données de réseau routier :

```python
import osmnx as ox
import momepy

streets_graph = ox.graph_from_place('La Rochelle, France', network_type='walk')
streets_graph = ox.projection.project_graph(streets_graph)
streets = ox.graph_to_gdfs(ox.get_undirected(streets_graph), nodes=False, edges=True,
                           node_geometry=False, fill_edge_geometry=True)
```

### 2. Conversion en format compatible Momepy

Momepy utilise un format NetworkX.MultiGraph pour l'analyse de réseau :

```python
graph = momepy.gdf_to_nx(streets)
```

### 3. Analyse basée sur les nœuds

Momepy offre plusieurs fonctions pour l'analyse des nœuds du réseau :

#### Clustering

```python
graph = momepy.clustering(graph, name='clustering')
```

#### Maillage (Meshedness)

Analyse topologique :
```python
graph = momepy.meshedness(graph, radius=5, name='meshedness')
```

Analyse métrique :
```python
graph = momepy.meshedness(graph, radius=400, name='meshedness400', distance='mm_len')
```

### 4. Visualisation des résultats

Après l'analyse, les résultats peuvent être convertis en GeoDataFrame pour la visualisation :

```python
nodes = momepy.nx_to_gdf(graph, points=True, lines=False, spatial_weights=False)

# Exemple de visualisation
import matplotlib.pyplot as plt

f, ax = plt.subplots(figsize=(30, 30))
nodes.plot(ax=ax, column='clustering', markersize=100, legend=True, cmap='viridis',
           scheme='quantiles', alpha=0.5, zorder=2)
streets.plot(ax=ax, color='lightgrey', alpha=0.5, zorder=1)
ax.set_axis_off()
plt.show()
```

## Exemples d'utilisation

Le notebook fourni illustre l'analyse du réseau pédestre de La Rochelle, démontrant :
- L'importation des données depuis OpenStreetMap
- L'analyse du clustering des nœuds
- L'analyse du maillage basée sur la distance topologique et métrique
- La visualisation des résultats sur des cartes

## Licence

Momepy est distribué sous licence BSD 3-Clause.

## Contact

Pour plus d'informations, visitez la [documentation officielle de Momepy](https://docs.momepy.org/).

Citations:
[1] https://docs.momepy.org/en/stable/user_guide/graph/network.html
[2] http://docs.momepy.org
[3] http://docs.momepy.org/en/stable/user_guide/graph/graph.html
[4] https://github.com/pysal/momepy/blob/main/docs/user_guide/getting_started.ipynb
[5] https://docs.momepy.org/en/latest/user_guide/preprocessing/simple_preprocessing.html
