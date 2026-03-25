# 🚀 BlazeDemo Performance Test

## 📌 Overview

Este projeto contém testes de performance para o fluxo completo de compra de passagem aérea no site:

🔗 https://www.blazedemo.com

O objetivo é avaliar o comportamento da aplicação sob diferentes padrões de carga e verificar o atendimento aos critérios definidos.

---

## 🧪 Cenário testado

Fluxo completo de compra:

1. Acesso à home (`GET /`)
2. Seleção de voo (`POST /reserve.php`)
3. Compra (`POST /purchase.php`)
4. Confirmação (`POST /confirmation.php`)

Validação de sucesso:

* Texto esperado: **"Thank you for your purchase today!"**

---

## 🎯 Critério de Aceitação

* **250 requisições por segundo (RPS)**
* **Tempo de resposta p90 < 2 segundos**

---

## ⚙️ Ferramentas

* Apache JMeter 5.6.3
* Execução em modo **non-GUI (CLI)**

---

## 📂 Estrutura do projeto

```
jmeter/
 └── blazedemo-test.jmx

results/
 └── report/
      └── index.html
```

---

## ▶️ Como executar

```bash
jmeter -n -t jmeter/blazedemo-test.jmx -l results/results.jtl -e -o results/report
```

---

## 🧪 Estrutura do teste

O plano de teste foi implementado em um único arquivo `.jmx`, contendo dois cenários:

### 🔹 Load Test

* 250 usuários
* Ramp-up: 10s
* Duração: 60s
* Controle de throughput: 250 RPS (Constant Throughput Timer)

---

### 🔹 Spike Test

* 500 usuários
* Ramp-up: 1s
* Duração: 30s
* Sem controle de throughput (simulação de pico real)

---

## 📊 Resultados

### 🔹 Load Test

| Métrica    | Resultado  |
| ---------- | ---------- |
| Throughput | 32.3 req/s |
| p90        | 2005 ms    |
| Error Rate | 0%         |

---

### 🔹 Spike Test

| Métrica    | Resultado  |
| ---------- | ---------- |
| Throughput | 23.0 req/s |
| p90        | 4830 ms    |
| Error Rate | 0.01%      |

---

## 📈 Análise dos Resultados

### ❌ Load Test

* O sistema não atingiu o throughput esperado (250 RPS).
* O p90 ficou ligeiramente acima do limite.
* Não houve erros, indicando estabilidade funcional.

📌 **Interpretação:**
O sistema responde corretamente sob carga moderada, porém não escala para o volume esperado.

---

### ⚠️ Spike Test

* Redução do throughput.
* Aumento significativo do tempo de resposta.
* Pequena taxa de erro.

📌 **Interpretação:**
O sistema apresenta degradação sob carga abrupta, indicando limitação de escalabilidade.

---

## 🧠 Análise Técnica

* O throughput é diretamente impactado pelo tempo de resposta das requisições.
* O endpoint inicial (`GET /`) apresentou maior latência, afetando todo o fluxo.
* Como o teste simula uma transação completa, o tempo total da jornada impacta o volume de requisições por segundo.
* O sistema demonstra comportamento estável, porém não escalável.

---

## 🧪 Decisões Técnicas

* Uso de **Constant Throughput Timer** para controle de carga no Load Test.
* Remoção do controle de throughput no Spike Test para simular cenários reais.
* Uso de **Response Assertion** para garantir sucesso funcional.
* Configuração de headers HTTP adequados para requisições POST.

---

## ⚠️ Observações

* O BlazeDemo é uma aplicação de demonstração e não foi projetada para alta carga.
* Os resultados não refletem um ambiente produtivo real.

---

## 📌 Conclusão Final

O sistema não atende ao critério de aceitação definido.

Apesar de não apresentar falhas funcionais, a aplicação demonstra limitações claras de escalabilidade, sendo incapaz de sustentar 250 requisições por segundo.

O comportamento sob teste de pico reforça essa limitação, evidenciando degradação significativa de performance.

---

## 📎 Relatório

O relatório completo pode ser acessado em:

```
results/report/index.html
```

---
