# Esteira de Aprendizado de Máquina: Previsão de Inadimplência de Cartão de Crédito

[![Abrir no Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/SEU_USUARIO/NOME_DO_REPOSITORIO/blob/main/default_of_credit_cards_clients.ipynb)

**Vídeo de apresentação (YouTube):** LINK_DO_VIDEO

## Descrição do projeto

Trabalho semestral da disciplina de Inteligência Artificial (CST em Análise e Desenvolvimento de Sistemas, Fatec Praia Grande).

O projeto é um notebook Python que implementa uma **esteira (pipeline) de aprendizado de máquina completa**, da obtenção dos dados até a predição de um exemplar inédito. O problema é de **classificação binária**: prever se um cliente de cartão de crédito ficará **inadimplente** no mês seguinte (classe 1) ou **pagará em dia** (classe 0).

- **Base de dados:** [Default of Credit Card Clients](https://archive.ics.uci.edu/dataset/350/default+of+credit+card+clients), do UCI Machine Learning Repository (ID 350). São 30.000 clientes, 23 atributos explicativos e 1 atributo alvo. A base é baixada automaticamente pelo código, sem upload manual.
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

## Como reproduzir a execução apresentada no vídeo

### Opção A: Google Colab (mais simples, sem instalar nada)

1. Clique no botão **Abrir no Colab** no topo deste README. Se preferir, acesse [colab.research.google.com](https://colab.research.google.com), vá em **Arquivo > Abrir notebook > GitHub**, cole a URL deste repositório e escolha `default_of_credit_cards_clients.ipynb`.
2. No menu, clique em **Ambiente de execução > Executar tudo**.
3. Aguarde a execução terminar (poucos minutos). A primeira célula instala o pacote `ucimlrepo` e baixa a base, então é preciso ter internet.
4. Confira as saídas na ordem das células (valores esperados na seção abaixo).

### Opção B: Execução local

Requisitos: Python 3.10 ou superior, Git e conexão com a internet.

```bash
# 1. Clonar o repositório
git clone https://github.com/SEU_USUARIO/NOME_DO_REPOSITORIO.git
cd NOME_DO_REPOSITORIO

# 2. Criar e ativar um ambiente virtual
python -m venv .venv
source .venv/bin/activate        # Linux/macOS
# .venv\Scripts\activate         # Windows

# 3. Instalar as dependências
pip install -r requirements.txt

# 4. Abrir o notebook
jupyter notebook default_of_credit_cards_clients.ipynb
```

5. No Jupyter, clique em **Kernel > Restart & Run All**.

## Resultados esperados

Como a divisão dos dados e o modelo usam `random_state=42`, a execução deve reproduzir os valores do vídeo. Pequenas diferenças podem aparecer se as versões das bibliotecas forem diferentes.

| Etapa | Saída esperada |
|---|---|
| Base carregada | 30.000 linhas x 24 colunas |
| Classes | Pagou (0): 77,88% / Inadimplente (1): 22,12% |
| Linhas removidas | 35 duplicadas, 345 com `EDUCATION` inválido, 54 com `MARRIAGE` inválido (restam 29.566) |
| Colunas após as transformações | 28 |
| Divisão | Treino 23.652 / Validação 2.957 / Teste 2.957 |
| Teste após separar o exemplar inédito | 2.956 amostras |
| Acurácia no teste | 0,8072 (cerca de 80,7%) |
| AUC-ROC no teste | 0,7694 |
| Predição do exemplar inédito (índice 20807) | 14,84% de probabilidade de inadimplência, classificado como "Pagou (0)". O conceito real era "Inadimplente (1)" |

> **Sobre o exemplar inédito:** o modelo erra esse caso porque, na classe inadimplente, o recall é de 37%. Isso mostra que o modelo deixa passar parte dos clientes que não pagam, e que a acurácia sozinha esconde essa limitação, já que cerca de 78% dos clientes são adimplentes. Para testar outra linha, altere a variável `posicao` na célula 6.

O notebook gera o arquivo `modelo_inadimplencia_xgb.joblib` durante a execução. Ele não é versionado no Git.

## Estrutura do repositório

```
.
├── README.md
├── requirements.txt
├── .gitignore
└── default_of_credit_cards_clients.ipynb
```

## Autor

NOME COMPLETO DO ALUNO, Fatec Praia Grande.
