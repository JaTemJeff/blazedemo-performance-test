# 🚀 BlazeDemo Performance Test

## 📌 Overview

Este projeto contém testes de performance para o fluxo de compra de passagem aérea no site:

🔗 https://www.blazedemo.com

O objetivo é validar se a aplicação suporta a carga definida no critério de aceitação.

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

---

## 📂 Estrutura do projeto

```
jmeter/
 ├── blazedemo-load-test.jmx
 ├── blazedemo-spike-test.jmx

results/
 ├── load-test-report/
 ├── spike-test-report/
```

---

## ▶️ Como executar

### 🔹 Load Test

```bash
jmeter -n -t jmeter/blazedemo-load-test.jmx -l results/load.jtl -e -o results/load-test-report
```

### 🔹 Spike Test

```bash
jmeter -n -t jmeter/blazedemo-spike-test.jmx -l results/spike.jtl -e -o results/spike-test-report
```

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

* O sistema atingiu aproximadamente **32.3 req/s**, muito abaixo do esperado (250 req/s).
* O tempo de resposta p90 ficou em **2005 ms**, ligeiramente acima do limite definido.
* Não foram observados erros, indicando estabilidade funcional.

📌 **Conclusão:**
O sistema não atende ao critério de aceitação, principalmente por não conseguir atingir o throughput esperado.

---

### ⚠️ Spike Test

* O throughput caiu ainda mais para **23 req/s**.
* O tempo de resposta degradou significativamente (p90 = **4830 ms**).
* Pequena taxa de erro foi observada (0.01%).

📌 **Conclusão:**
O sistema apresenta forte degradação sob carga abrupta, indicando baixa capacidade de escalabilidade.

---

## 🧠 Análise Técnica

* O baixo throughput indica que o sistema não consegue processar requisições em paralelo na escala exigida.
* O aumento do tempo de resposta sob carga demonstra saturação de recursos.
* O endpoint inicial (`GET /`) apresentou maior latência, impactando o fluxo completo.
* Como o teste simula um fluxo completo, o tempo total da transação influencia diretamente o throughput.

---

## 🧪 Decisões Técnicas

* Foi utilizado **Constant Throughput Timer** no Load Test para controle de 250 RPS.
* No **Spike Test**, o controle de throughput foi removido para simular picos reais.
* Utilizado **Response Assertion** para validar sucesso da compra.
* Configurado **HTTP Header Manager** com `Content-Type: application/x-www-form-urlencoded`.

---

## ⚠️ Observações

* O BlazeDemo é uma aplicação de demonstração, não otimizada para alta carga.
* Os resultados não representam ambientes reais de produção.

---

## 📌 Conclusão Final

O sistema não atende ao critério de aceitação definido.

Embora não apresente falhas funcionais, a aplicação demonstra limitações claras de escalabilidade, sendo incapaz de sustentar o volume de 250 requisições por segundo.

Sob carga de pico, há degradação significativa de performance, reforçando a limitação estrutural do sistema.

---
