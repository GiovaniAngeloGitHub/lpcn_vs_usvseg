# **Estudo Comparativo entre LPCN e USVSEG**

Este documento resume o plano de trabalho para um estudo comparativo entre os métodos **LPCN** e **USVSEG**, incluindo conversão de software, análise, benchmarking e implementação de um classificador. Os parâmetros bioacústicos devem ser calculados de acordo com a metodologia utilizada pelo **DeepSqueak**.

---

## 🎯 **Objetivos do Projeto**

### **1. Converter o software LPCN para Python**

* Reescrever o código original do LPCN em Python.
* Garantir compatibilidade com bibliotecas modernas (NumPy, SciPy, PyTorch, etc.).
* Validar a equivalência dos resultados com a versão original.

---

### **2. Familiarizar-se com o USVSEG**

* Estudar o funcionamento interno do USVSEG.
* Compreender:

  * Pré-processamento
  * Segmentação
  * Extração de parâmetros
* Reproduzir experimentos básicos para entender o pipeline.

---

### **3. Realizar Benchmark Comparativo (LPCN vs. USVSEG)**

* Definir métricas de comparação (ex.: F1-score, sensibilidade, precisão, tempo de execução).
* Utilizar base de dados padronizada para os testes.
* Gerar tabelas e gráficos comparativos.
* Avaliar robustez em diferentes níveis de ruído.

---

### **4. Implementar um Classificador**

* Treinar um classificador utilizando as features extraídas por ambos os métodos.
* Comparar desempenho quando alimentado com:

  * Parâmetros do LPCN
  * Parâmetros do USVSEG
* Avaliar modelos supervisionados (SVM, Random Forest, Redes Neurais).

---

## 🔬 **Observação Importante**

Para a extração de parâmetros bioacústicos, **utilizar a forma de cálculo empregada pelo DeepSqueak**, garantindo consistência metodológica com padrões atuais da área.
