# Agrupamento de Pacientes com Insuficiência Cardíaca (K-Means)

Trabalho de Matemática (TDE 4) que aplica o algoritmo **K-Means** para encontrar grupos de pacientes com insuficiência cardíaca a partir de exames clínicos. Depois, o trabalho verifica se esses grupos têm relação com a mortalidade.

## Base de dados

[Heart Failure Clinical Records](https://archive.ics.uci.edu/dataset/519/heart+failure+clinical+records), do UCI Machine Learning Repository: **299 pacientes** e 13 atributos clínicos, sem valores ausentes.

O agrupamento usa os **6 atributos contínuos**:

| Atributo | Descrição |
|----------|-----------|
| `age` | Idade (anos) |
| `creatinine_phosphokinase` | Enzima CPK no sangue (mcg/L) |
| `ejection_fraction` | % de sangue bombeado a cada contração |
| `platelets` | Plaquetas (kiloplatelets/mL) |
| `serum_creatinine` | Creatinina sérica (mg/dL) |
| `serum_sodium` | Sódio sérico (mEq/L) |

Os atributos binários (anemia, diabetes, hipertensão, sexo e tabagismo) servem apenas para descrever os grupos. O `death_event` (óbito) **não entra no modelo** e só é usado na avaliação final.

## Etapas

1. **Carregamento e exploração:** distribuições, assimetria, valores ausentes e duplicados.
2. **Pré-processamento:** transformação logarítmica em `creatinine_phosphokinase` e `serum_creatinine` (que têm cauda longa) e padronização (z-score).
3. **K-Means** com `k-means++` para k = 1 a 10.
4. **Escolha de k:** curva do cotovelo, silhueta média e gráficos de silhueta por ponto.
5. **Interpretação:** centroides, perfil dos grupos e visualização com PCA.
6. **Análise de sensibilidade:** resultados sem a transformação log e incluindo os binários.
7. **Comparação com o óbito:** mortalidade por grupo e índice ARI.

## Principais resultados

- **k = 2** teve a maior silhueta média (0,18).
- **G0 (206 pacientes):** valores próximos da média em todos os atributos. Mortalidade de **19,4%**.
- **G1 (93 pacientes):** mais idosos (≈ 68 anos), com creatinina alta (≈ 1,86 mg/dL), sódio baixo e menor fração de ejeção. Mortalidade de **60,2%**.
- A mortalidade geral é de 32,1%. Mesmo sem usar o óbito no treinamento, o K-Means separou um grupo de risco bem maior (ARI = 0,22).

## Como executar

Abra o `TDE4_matematica.ipynb` no Google Colab ou no Jupyter e execute as células em ordem. As dependências são:

```bash
pip install ucimlrepo pandas numpy matplotlib scikit-learn
```

O notebook baixa os dados automaticamente pelo `ucimlrepo`. Se isso falhar, ele lê o arquivo `heart_failure_clinical_records_dataset.csv` na mesma pasta.

## Referência

Chicco, D.; Jurman, G. *Machine learning can predict survival of patients with heart failure from serum creatinine and ejection fraction alone.* BMC Medical Informatics and Decision Making, 20, 16 (2020).
