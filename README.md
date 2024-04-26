# hackathon-hiper-069

import networkx as nx
import matplotlib.pyplot as plt

def generate_diagram(data):
  """
  Generates a hierarchical diagram from the provided data.

  Args:
    data: A list of dictionaries, each representing an application with the following structure:
      {
        "Aplicacao": str,
        "PastaOrigem": str,
        "PastaDestino": str (optional),
        "PastaBackup": str (optional)
      }
  """

  
  G = nx.DiGraph()

  
  for app in data:
    app_name = app["Aplicacao"]
    G.add_node(app_name, shape="box")

    
    G.add_edge(app["PastaOrigem"].lower(), app_name)

    # Add edge to destination folder if exists
    if "PastaDestino" in app:
      G.add_edge(app_name, app["PastaDestino"].lower())

    
    if "PastaBackup" in app:
      for other_app in data:
        if other_app["PastaDestino"] and other_app["PastaDestino"].lower() == app["PastaBackup"].lower():
          G.add_edge(other_app["Aplicacao"], app_name, style="dashed")

  
  pos = nx.nx_agraph.graphviz_layout(G, prog="dot", args="-Grankdir=LR")

  
  nx.draw(G, pos, with_labels=True, node_size=2000, node_color="lightblue", font_size=8)
  edge_labels = nx.get_edge_attributes(G, "style")
  nx.draw_networkx_edge_labels(G, pos, edge_labels=edge_labels, font_size=6)
  plt.show()


data = [
  {"Aplicacao": "App1", "PastaOrigem": "FolderA", "PastaDestino": "FolderB", "PastaBackup": "FolderC"},
  {"Aplicacao": "App2", "PastaOrigem": "FolderB"},
  {"Aplicacao": "App3", "PastaOrigem": "FolderC", "PastaDestino": "FolderD"},
  {"Aplicacao": "App4", "PastaOrigem": "FolderD"}
]

generate_diagram(data)
