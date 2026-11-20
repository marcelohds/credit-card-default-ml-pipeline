# Esteira de Aprendizado de Máquina: Previsão de Inadimplência de Cartão de Crédito

## Descrição do projeto

O projeto é um notebook Python que implementa uma **esteira (pipeline) de aprendizado de máquina completa**, da obtenção dos dados até a predição de um exemplar inédito. O problema é de **classificação binária**: prever se um cliente de cartão de crédito ficará **inadimplente** no mês seguinte (classe 1) ou **pagará em dia** (classe 0).

- **Base de dados:** *Default of Credit Card Clients*, do UCI Machine Learning Repository (ID 350). São 30.000 clientes, 23 atributos explicativos e 1 atributo alvo. A base é baixada automaticamente pelo código, sem upload manual.
- **Modelo:** XGBoost (`XGBClassifier`).

## Etapas da esteira

| Etapa | O que o notebook faz |
|---|---|
| 1. Obtenção dos dados | Baixa a base direto do UCI com o pacote `ucimlrepo` |
| 2. Estatísticas descritivas | `info`, `describe`, valores nulos, frequência das categóricas e distribuição das classes |
| 3. Transformação de linhas | Remove duplicatas e registros com `EDUCATION` ou `MARRIAGE` fora do dicionário da base |
| 4. Transformação de colunas | Cria `UTILIZACAO_LIMITE_M1`, aplica one-hot em `SEX`/`EDUCATION`/`MARRIAGE` e normaliza as colunas monetárias |
| 5. Divisão dos dados | Treino (80%), validação (10%) e teste (10%), estratificados |
| 6. Treinamento | XGBoost com 100 árvores, `max_depth=5`, `learning_rate=0.2`, `subsample=0.8`, `colsample_bytree=0.8`, `random_state=42` |
| 7. Avaliação | Acurácia, AUC-ROC, matriz de confusão, curva ROC e variáveis mais importantes |
| 8. Predição | Salva o modelo com `joblib`, carrega do disco e prevê **1 exemplar inédito**, separado do conjunto de teste |

## Como reproduzir a execução (Google Colab)

Não é preciso instalar nada no computador. Você só precisa de uma conta Google e de conexão com a internet.

1. Neste repositório, abra o arquivo `default_of_credit_cards_clients.ipynb` e baixe-o para o seu computador (botão de download no canto superior direito da visualização do arquivo).
2. Abra o Google Colab no navegador e entre com a sua conta Google.
3. No menu, clique em **Arquivo > Fazer upload de notebook** e selecione o arquivo `default_of_credit_cards_clients.ipynb` que você baixou.
4. Com o notebook aberto, clique em **Ambiente de execução > Executar tudo**.
5. Aguarde a execução terminar (poucos minutos). A primeira célula instala o pacote `ucimlrepo` e baixa a base automaticamente.
6. Confira as saídas de cada célula, na ordem de execução.

O notebook gera o arquivo `modelo_inadimplencia_xgb.joblib` durante a execução, dentro do ambiente temporário do Colab.

## Estrutura do repositório

```
.
├── README.md
├── .gitignore
└── default_of_credit_cards_clients.ipynb
```
