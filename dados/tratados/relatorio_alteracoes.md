# Relatório das alterações nas bases

## 1. Base oficial do CNJ → `jn_trt_2025.csv` (dicionário: `dicionario_jn_trt.csv`)
Origem: `dados/brutos/jn2026_base_17-set-2026.zip`, arquivo `JN_23-Jun-2026.csv`: 1.595 linhas × 1.314 colunas, sem alteração.
1. Filtro: só a Justiça do Trabalho.
2. A linha "TRT" (soma dos 24 TRTs, feita pelo CNJ) foi separada e usada só como total de referência.
3. Ficaram 17 colunas, renomeadas para português.
4. Taxas convertidas de fração para %.
5. Criadas 5 medidas: meses_ate_sentenca, meses_idade_estoque, meses_para_zerar_estoque, casos_novos_por_juiz, pct_suspensos.
6. Conferência: taxa de congestionamento e índice de conciliação recalculados batem com os do CNJ.
7. Regra de alerta (valor de 2025 menor que 1/3 ou maior que 3 vezes a mediana 2016–2025 do próprio TRT): dias_ate_sentenca: nenhum; dias_idade_estoque: TRT19; pendentes: nenhum. Investigado e MANTIDO (mudança real: redução do estoque do TRT19 em 2023).
8. Resultado: 24 linhas (os 24 TRTs em 2025). Nenhum valor foi apagado ou editado à mão.

## 2. Amostra da API do DataJud → `datajud_trt_amostra_2025.csv` (dicionário: `dicionario_datajud_amostra.csv`)
Origem: `dados/brutos/datajud/`, com as respostas da API guardadas sem alteração.
- TRTs com amostra completa: 22 de 24. Fora: TRT2, TRT5 (a API não entregou a amostra inteira).
- Processos baixados: 45.827.

| Passo da limpeza | Antes | Depois | Removidos |
|---|---|---|---|
| A. duplicatas | 45.827 | 45.827 | 0 |
| B. 1ª sentença não é de 2025 | 45.827 | 17.176 | 28.651 |
| C. data inválida | 17.176 | 17.176 | 0 |
| D. sentença antes da entrada | 17.176 | 17.175 | 1 |
| E. entrada antes de 2015 | 17.175 | 17.175 | 0 |

- Colunas criadas: rito, encerramento, n_assuntos, faixa_assuntos, 11 temas (os que aparecem em pelo menos 3% dos processos) e peso.
- O Juízo 100% Digital ficou fora da análise: o registro dele está concentrado nos processos que entraram em 2025 (seção 3.1 do notebook).
- Privacidade: o número do processo foi trocado por um código (hash SHA-256, 12 caracteres). Nenhuma base tem nomes de pessoas: a API pública não traz as partes, e todos os processos da amostra são públicos (nível de sigilo 0).
- Resultado: 17.175 processos.
