# Relatório Consolidado de Testes de Performance - Blazedemo

## 1. Objetivo
Avaliar a capacidade e a resiliência do sistema Blazedemo em suportar uma carga de alta intensidade para o fluxo de compra de passagens aéreas, verificando o cumprimento dos Critérios de Aceitação estabelecidos.

## 2. Cenário
* Compra de passagem aérea - Passagem comprada com sucesso.

## 3. Critérios de Aceitação
*   **Vazão:** 250 requisições por segundo (TPS).
*   **Tempo de Resposta:** 90th Percentile (Percentil 90) inferior a 2 segundos (2000ms).

## 4. Configuração do Cenário (Comum aos Testes)
*   **Ferramenta:** Apache JMeter 5.6.3 (Executado via modo CLI/Non-GUI).
*   **Cenário de Teste:** Fluxo completo (Home > Reserva > Compra > Confirmação).
*   **Usuários Virtuais (Threads):** 250 usuários.
*   **Carga Alvo:** 15.000 amostras por minuto (250 req/s) via Constant Throughput Timer. 
    *   *Cálculo:* 250 requisições × 60 segundos = 15.000 por minuto. 
    *   *Objetivo:* Ajustar o JMeter para enviar exatamente a carga de 250 req/s exigida pelo critério de aceitação.

---

## 5. Teste de Carga (Estabilidade)
*Foco em avaliar a estabilidade do sistema com uma subida gradual de usuários.*

*   **Ramp-up:** 250 segundos (entrada de 1 usuário por segundo). Usei um ramp-up linear para evitar um pico artificial de carga e observar como o servidor se comporta conforme a demanda cresce.
*   **Duração:** 10 minutos (600 segundos).

### Resultados Obtidos (Carga):


| Métrica | Valor Obtido | Status |
| :--- | :--- | :--- |
| **Vazão (Throughput)** | 159.07 req/s | **Reprovado** |
| **Tempo de Resposta (90th pct)** | 9.85 segundos | **Reprovado** |
| **Taxa de Erro** | 1.10% | **Alerta** |

**Análise de Erros:** Foram identificadas **392 ocorrências de Erro HTTP 429 (Too Many Requests)**. Esse erro indica um mecanismo de proteção (*Rate Limit*) que bloqueou as requisições ao atingir o limite de intensidade.

---

## 6. Teste de Pico (Spike Test)
*Foco em avaliar a resiliência a um aumento massivo de acessos.*

*   **Ramp-up:** 10 segundos. O objetivo de um ramp-up curto é simular um "pico" de carga (acesso simultâneo massivo) para validar a capacidade de resposta imediata do servidor.
*   **Duração:** 5 minutos (300 segundos).

### Resultados Obtidos (Pico):


| Métrica | Valor Obtido | Status |
| :--- | :--- | :--- |
| **Vazão (Throughput)** | 180.62 req/s | **Reprovado** |
| **Tempo de Resposta (90th pct)** | 7.94 segundos | **Reprovado** |
| **Taxa de Erro** | 0.58% | **Alerta** |

**Análise de Erros:** Foram identificadas **323 ocorrências de Erro HTTP 429**. A ativação deste erro ocorreu de forma mais agressiva, indicando que o impacto repentino aciona as proteções do servidor mais rapidamente.

---

## 7. Conclusão Geral
Eu monitorei minha máquina local durante os testes e o consumo de CPU e Memória foi baixo. Isso prova que o atraso de 9 segundos e os erros 429 vieram da rede ou do servidor, e não de um gargalo na ferramenta de teste.
O sistema **não atende** aos critérios de aceitação sob a carga solicitada de 250 req/s em nenhum dos cenários.

**Observações Técnicas:**
1.  **Saturação:** O servidor atingiu o ponto de saturação antes da carga alvo, estabilizando entre 159 e 180 req/s.
2.  **Degradação de Performance:** O tempo de resposta de 90º percentil ficou entre 7.9s e 9.8s, sendo muito superior ao limite aceitável de 2s.
3.  **Mecanismo de Defesa:** A presença de erros 429 confirma que a infraestrutura possui limitadores que impedem este volume de tráfego.

## 8. Recomendações
*   Revisar as configurações de *Rate Limiting* do servidor.