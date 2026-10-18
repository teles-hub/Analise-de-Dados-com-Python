# Análise de notas e votos de animes da Crunchyroll

Análise exploratória em Python da base de avaliações de animes da Crunchyroll. O notebook responde a três perguntas:

1. Como se distribuem as notas e os votos dos animes na plataforma?
2. Quais são os animes mais populares (mais votados) e os mais bem avaliados?
3. Existe relação entre a quantidade de episódios e a nota recebida?

## Base de dados

[Crunchyroll Anime Ratings](https://www.kaggle.com/datasets/filipefilardi/crunchyroll-anime-ratings), do Kaggle. Na execução original, o arquivo `animes.csv` tinha 1.255 linhas e 41 colunas: nome, número de episódios, votos, nota média (`rate`), contagem de votos por nota de 1 a 5 e colunas de gênero. A licença e a data de coleta estão na página do dataset.

O notebook baixa os dados sozinho com o `kagglehub`, então não é preciso colocar o CSV no repositório.

## Como executar

**No Google Colab:** envie o arquivo `analise_animes_crunchyroll.ipynb` e use *Ambiente de execução → Executar tudo*.

**No computador:**

```bash
pip install -r requirements.txt
jupyter notebook analise_animes_crunchyroll.ipynb
```

Se o `kagglehub` pedir autenticação no Kaggle, siga as instruções que ele mostrar.

## O que o notebook faz

1. Identificação do problema e objetivos
2. Importação das bibliotecas e carregamento dos dados
3. Análise exploratória e limpeza
4. Visualizações: histograma de notas, top 10 por votos, top 10 mais bem avaliados, dispersão de episódios × nota e matriz de correlação
5. Relatório final, com os números calculados a partir dos dados

## Decisões sobre os dados

- **Nota 0 = sem votos.** Na primeira execução, o mínimo de votos era 0 e o mínimo de nota também. Animes sem nenhuma avaliação aparecem com nota 0, o que não é uma nota ruim. Eles ficam fora das análises de nota. Na primeira versão, a média de 4,08 incluía esses títulos.
- **Episódios = 0 tratado como "não informado".** Pelo menos 25% das linhas tinham 0 episódios. É uma suposição, e o notebook mostra quantos títulos foram afetados.
- **Ranking de "bem avaliados" com mínimo de 100 votos.** Sem esse corte, um anime com poucos votos e nota 5,0 apareceria no topo. O valor de 100 é uma escolha do projeto e pode ser alterado na constante `MIN_VOTOS`.
- **Correlação de Spearman, sem a coluna `weight`.** Votos e episódios têm poucos valores muito altos, e a correlação de Spearman lida melhor com isso. A coluna `weight` repete a informação de `votes` (nos primeiros registros ela é aproximadamente votos × nota), então foi retirada.

## O que mudou em relação à primeira versão

- Conversão das colunas numéricas antes da remoção de nulos, e contagem de linhas removidas.
- Tratamento de nota 0 e episódios 0 (descrito acima).
- Novo gráfico de mais bem avaliados, que completa a segunda pergunta ("populares **e** bem avaliados").
- Dispersão com escala logarítmica e sem os títulos sem episódios informados.
- Relatório final gerado a partir dos números, sem frases fixas, para que as conclusões acompanhem os dados.
- Remoção de importações não usadas e de uma importação repetida.

## Limitações

- As notas vêm de usuários da plataforma, não de crítica especializada.
- A base é um retrato de um momento, e os dados podem não refletir o catálogo atual.
- Correlação mostra associação, não causa.
- Os resultados dependem das decisões acima (nota 0, episódios 0, corte de votos).

## Possíveis próximos passos

A base tem colunas de gênero e a distribuição de votos por nota (`rate_1` a `rate_5`), que não foram usadas. Elas permitiriam analisar quais gêneros são mais bem avaliados.

## Tecnologias

Python, pandas, matplotlib, seaborn e kagglehub.
