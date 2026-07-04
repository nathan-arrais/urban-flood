# Nowcasting de inundações urbanas com modelos U-RNN simplificados

Este repositório contém uma solução demonstrativa em Python para a atividade de inteligência artificial, séries espaço-temporais e nowcasting de inundações urbanas, inspirada no modelo U-RNN.

O dataset `urbanflood24.zip` é muito grande para ser descompactado integralmente em muitos computadores. Por isso, a entrega inclui em `data_demo/processed/` um subconjunto leve já processado, extraído de alguns eventos de `location1` no padrão aproximado do UrbanFlood24 Lite. O notebook também consegue regenerar esse subconjunto a partir do ZIP original, caso ele esteja disponível localmente.

O experimento compara quatro abordagens:

- persistência;
- regressão residual simples;
- Mini U-RNN;
- U-RNN Lite U-like, uma versão maior com encoder-decoder em formato de U e ConvGRU no gargalo.

A versão atual usa 8 eventos de treino e 4 eventos de teste de `location1`, todos com séries de chuva de 360 passos. Os modelos neurais são treinados por 20 épocas em CPU. As métricas são salvas de duas formas:

- `outputs/metricas.csv`: métricas agregadas nos 4 eventos de teste;
- `outputs/metricas_por_evento.csv`: métricas separadas por evento.

## Arquivos principais

- `urban_flood_urnn_demo.ipynb`: notebook principal, com explicações, código, gráficos, métricas e evidências.
- `data_demo/processed/`: subconjunto leve usado pelo notebook.
- `outputs/`: gráficos e métricas já gerados.
- `urbanflood24.zip`: arquivo original opcional para regenerar os dados processados; ele é grande e fica ignorado pelo Git.

## Como executar

1. Clone o repositório.
2. Abra `urban_flood_urnn_demo.ipynb`.
3. Execute as células em ordem.

O ZIP original não é necessário para validar a entrega, pois os dados processados leves já estão em `data_demo/processed/`. Se quiser recriar esses arquivos a partir do dataset bruto, coloque `urbanflood24.zip` na raiz do projeto e remova ou substitua os arquivos processados.

Dependências usadas:

```bash
pip install numpy matplotlib pandas scikit-learn torch nbformat nbclient
```

O notebook usa e/ou cria:

- `data_demo/processed/`: subconjunto reduzido dos dados;
- `outputs/`: gráficos e métricas gerados pelo experimento.

## Referências

- Artigo: U-RNN: High-resolution Spatiotemporal Nowcasting of Urban Flooding, Journal of Hydrology, 2025.
- Dataset: Supplementary data of U-RNN high-resolution spatiotemporal nowcasting of urban flooding.
- GitHub oficial: https://github.com/holmescao/U-RNN

## Observação técnica

Esta entrega não tenta reproduzir o treinamento completo do artigo, que usa dados em alta resolução, pesos treinados e ambiente com GPU. O objetivo aqui é demonstrar, de forma funcional e verificável em CPU, o fluxo computacional do problema: leitura dos dados, redução espaço-temporal, uso de variáveis hidrológicas e estáticas, modelo recorrente convolucional simplificado, inferência, métricas e visualizações.

## Aderência aos requisitos

A solução entregue contempla:

- estudo e adaptação da proposta do U-RNN;
- uso de um subconjunto processado do material suplementar `urbanflood24.zip`;
- execução em notebook Python;
- inferência e experimento simplificado de nowcasting;
- geração de gráficos, mapas, métricas e evidências em `outputs/`;
- explicação das decisões técnicas e das limitações da abordagem;
- organização mínima para publicação em GitHub público.

## Interpretação dos resultados

O baseline de persistência é muito forte porque a previsão é feita entre passos próximos de 10 minutos. Assim, o mapa de inundação do próximo instante costuma ser parecido com o mapa anterior.

Os modelos neurais implementados são versões reduzidas, treinadas em CPU e com dados reduzidos, portanto não devem ser comparados diretamente ao resultado completo do artigo. Eles servem para demonstrar a integração entre chuva, mapas estáticos urbanos, estado anterior de inundação e redes recorrentes convolucionais.

Na configuração atual, a regressão residual simples tende a apresentar melhor RMSE/R² agregado que os modelos neurais, enquanto a persistência permanece muito competitiva em MAE. Esse resultado é coerente com um experimento curto de nowcasting, no qual a continuidade temporal é extremamente forte.
