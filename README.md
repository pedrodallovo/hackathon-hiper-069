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


#(Importar Bibliotecas: Importamos o networkx para criação e manipulação de graficos, e o matplotlib.pyplot para visualização.

Função generate_diagram(data): Esta função recebe uma lista de dicionários, cada um representando uma aplicação com suas informações de pasta.

Criação do Grafico: Criamos um grafico direcionado G usando o networkx.

Nós e Arestas: Para cada aplicação: -Um nó é criado com o nome da aplicação e uma forma de caixa. -Uma aresta é adicionada da pasta de origem para o nó da aplicação. -Se uma pasta de destino existir, uma aresta é adicionada da aplicação para a pasta de destino. -Se uma pasta de backup existir, iteramos por outras aplicações e adicionamos arestas tracejadas das aplicações monitorando a pasta de backup para esta aplicação.

Layout do Grafico: Utilizamos graphviz_layout para gerar automaticamente um layout hierárquico para o grafico.

Desenho: -Desenhamos o grafo com rótulos de nós, tamanhos e cores. -Rótulos de aresta são adicionados para diferenciar arestas de monitoramento de backup (linhas tracejadas).

Exemplo de Uso: Substitua os dados de exemplo pelos seus dados reais e execute a função generate_diagram.

Formato dos Dados de Entrada: -Os dados de entrada devem ser uma lista de dicionários, cada um representando uma aplicação com as seguintes chaves: "Aplicacao": Nome da aplicação (string). "PastaOrigem": Caminho da pasta de origem (string). "PastaDestino": Caminho da pasta de destino (opcional, string). "PastaBackup": Caminho da pasta de backup (opcional, string).

Saída: O código irá gerar um diagrama hierárquico mostrando o fluxo de informações entre as aplicações com base em suas relações de pasta.)
