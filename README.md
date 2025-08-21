# Consumo de Energia Residencial (UCI) — Entrega

Este repositório reúne a solução dos **20 exercícios** usando o dataset **Individual Household Electric Power Consumption** (UCI, ID 235).  
O notebook principal está em `notebooks/01_analise_power_refeito.ipynb` — com código comentado, respostas e gráficos.

> Objetivo: analisar o consumo elétrico em nível de minuto, criar agregações (dia/mês), explorar correlações, segmentar dias com *k-means*, decompor a série e treinar uma regressão simples.

---

## 🔗 Dataset
- Nome: *Individual Household Electric Power Consumption*  
- Fonte: https://archive.ics.uci.edu/dataset/235/individual+household+electric+power+consumption  
- Arquivo principal: `household_power_consumption.txt` (≈ 2M linhas; separador `;`, ausentes como `?`).

**Principais colunas**
- `Global_active_power` (kW) – potência ativa (consumo útil)  
- `Global_reactive_power` (kVAR) – potência reativa (não realiza trabalho útil)  
- `Voltage` (V), `Global_intensity` (A)  
- `Sub_metering_1..3` (Wh) – submedidores

---

## 📁 Estrutura

```
power-consumption/
├─ README.md
├─ environment.yml
├─ requirements.txt
├─ data/
│  ├─ raw/                # dados originais .txt
│  └─ processed/          # derivados (csv/parquet agregados)
└─ notebooks/
   └─ 01_analise_power_refeito.ipynb
```

> **Dica**: adicione `data/raw/*` ao `.gitignore` para evitar subir o arquivo de ~127 MB.

---

## 🚀 Como rodar

### Opção A — Conda (recomendada)
```bash
conda env create -f environment.yml
conda activate power-consumption
jupyter lab  # ou: jupyter notebook
```
Abra `notebooks/01_analise_power_refeito.ipynb` e **Run All**.

### Opção B — Pip + venv
```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate

pip install -r requirements.txt
jupyter lab
```

### Onde colocar os dados
- Coloque `household_power_consumption.txt` (ou o `.zip` da UCI) em `data/raw/`.
- O notebook já tenta **extrair** o `.txt` do ZIP automaticamente, se encontrar um arquivo compatível ali dentro.

---

## ✅ O que o notebook entrega
- Carregamento robusto (índice temporal com `Date+Time` em `DatetimeIndex`).
- Tratamento de ausentes e conversão numérica das colunas.
- Agregações por **dia** e **mês** (médias e somas).
- Gráficos: série diária de um dia, histograma de `Voltage`, `Voltage` por dia em um ano.
- Correlações (`Global_active_power`, `Global_reactive_power`, `Voltage`, `Global_intensity`).
- Variável `Total_Sub_metering` (soma dos submedidores).
- Segmentação em **3 clusters** (k-means) com *features diárias*.
- Decomposição sazonal (opcional; executa só se `statsmodels` estiver disponível).
- Regressão linear para prever `Global_active_power` a partir de `Global_intensity` (métricas: MAE, RMSE e R²).

---

## 🧩 Guia rápido dos 20 itens
1. `head(10)`
2. Explicação ativa vs reativa (texto)
3. Ausentes por coluna
4. `Date` → `datetime` e `weekday`
5. 2007: média diária de `Global_active_power`
6. Gráfico de um dia específico
7. Histograma de `Voltage`
8. Média mensal de `Global_active_power`
9. Dia com maior **soma** diária
10. Semana vs fim de semana (médias)
11. Correlações
12. `Total_Sub_metering`
13. Meses com `Total_Sub_metering` > `Global_active_power` (médias)
14. `Voltage` diário em **2008** (ou no ano disponível)
15. Verão (JJA) vs Inverno (DJF)
16. Amostra 1% vs base completa (distribuição)
17. Normalização Min–Max
18. K-means (k=3) com *features* diárias
19. Decomposição (6 meses)
20. Regressão linear (GAP ~ GI)

---

## 🛠️ Dicas e solução de problemas

- **`KeyError: '2008'` ao fazer `df.loc['2008']`:**  
  O seu `df` não tem 2008. O notebook já troca automaticamente para o ano disponível e imprime qual foi usado. Se quiser forçar 2008, garanta que você carregou o **arquivo completo** da UCI.

- **`TypeError: unexpected keyword 'squared'` no `mean_squared_error`:**  
  Versão mais antiga do scikit-learn. O notebook já calcula o **RMSE** como `np.sqrt(MSE)`, compatível com qualquer versão.

- **`NameError: df is not defined`:**  
  Rodar sempre a célula de **Setup** antes das demais (ou `Kernel > Restart & Run All`).

- **Lento/alto uso de memória:**  
  É um dataset grande. Evite criar muitas cópias do `df`. Use agregações (`resample`) sempre que possível.

---

## 🧪 Como avaliar a entrega
- Executar o notebook do começo ao fim sem erros.
- Checar as saídas textuais (ex.: dia de maior consumo, médias/estação, métricas da regressão).
- Conferir os gráficos gerados.
- Revisar rapidamente a interpretação curta em cada seção.

---

## 📜 Créditos
- UCI Machine Learning Repository — *Individual Household Electric Power Consumption* (ID 235).
- Código e comentários por **Lu Vieira Santos** (turma/projeto).