import networkx as nx
import matplotlib.pyplot as plt

graph = {
    'A': {'B': 2, 'C': 4, 'D': 1},
    'B': {'A': 2, 'C': 3, 'E': 2},
    'C': {'A': 4, 'B': 3, 'D': 2, 'E': 5},
    'D': {'A': 1, 'C': 2, 'E': 3, 'F': 6},
    'E': {'B': 2, 'C': 5, 'D': 3, 'F': 2, 'G': 1},
    'F': {'D': 6, 'E': 2, 'G': 3, 'H': 4},
    'G': {'E': 1, 'F': 3, 'I': 2},
    'H': {'F': 4, 'I': 3, 'J': 5},
    'I': {'G': 2, 'H': 3, 'J': 1},
    'J': {'H': 5, 'I': 1}
}

def plot_graph_with_path(graph, path):
    G = nx.Graph()
    
    for node, neighbors in graph.items():
        for neighbor, weight in neighbors.items():
            G.add_edge(node, neighbor, weight=weight)
    
    pos = nx.spring_layout(G)
    
    nx.draw_networkx_nodes(G, pos)

    nx.draw_networkx_edges(G, pos)
    
    labels = {node: node for node in G.nodes()}
    nx.draw_networkx_labels(G, pos, labels)
    
    path_edges = [(path[i], path[i + 1]) for i in range(len(path) - 1)]
    nx.draw_networkx_edges(G, pos, edgelist=path_edges, edge_color='r', width=2)
    
    plt.axis('off')
    plt.show()

start_node = 'A'
goal_node = 'J'
beam_width = 2

shortest_path = oracle(graph, start_node, goal_node, beam_width)
if shortest_path:
    print(f"Shortest path from {start_node} to {goal_node}: {shortest_path}")
    plot_graph_with_path(graph, shortest_path)
else:
    print(f"No path found from {start_node} to {goal_node}")
