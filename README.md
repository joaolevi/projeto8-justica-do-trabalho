# Projeto 8: Tempo de tramitação e congestionamento na Justiça do Trabalho

[![Abrir no Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/joaolevi/projeto8-justica-do-trabalho/blob/main/projeto8_tramitacao_congestionamento.ipynb)

Projeto da disciplina **Introdução à Ciência de Dados (UFAL)**.

**Recorte:** Justiça do Trabalho, 1º grau, fase de conhecimento, 24 Tribunais Regionais do Trabalho (TRTs).
- **Duração:** dos processos encerrados (1ª sentença ou acordo) em 2025.
- **Estoque:** pendente em 31/12/2025.
- **Destaque:** o TRT19 (Alagoas).

Tudo está no notebook [`projeto8_tramitacao_congestionamento.ipynb`](projeto8_tramitacao_congestionamento.ipynb): extração, limpeza com a justificativa de cada decisão, análise, respostas e uma sugestão de modelo de *machine learning*. Ele foi escrito para quem não é da área.

## Perguntas e respostas curtas

| Pergunta | Resposta |
|---|---|
| **P1.** Como se distribuem a duração e o estoque nos 24 TRTs? | **Muito desigual.** A duração média até a 1ª sentença vai de 2,3 meses (TRT14) a 9,6 meses (TRT4), e quem demora mais também acumula mais estoque. Dentro de cada TRT, a maioria termina rápido, mas 10% dos processos passam de 14,7 meses. |
| **P2.** A demora vem do tribunal ou do perfil dos processos? | **Do tribunal.** Comparando processos de mesmo perfil, 93% da diferença entre os TRTs continua. |
| **P3.** O que está associado à demora e ao acúmulo? | **No processo:** acordo (−48% de tempo) e rito sumaríssimo (−31%). Temas como horas extras e indenizações alongam de 10% a 16%. **Entre tribunais:** nenhum indicador oficial de estrutura (casos ou servidores por juiz) explica com segurança a diferença. |

## Dados

| Fonte | O que é | Uso |
|---|---|---|
| [Justiça em Números 2026](https://www.cnj.jus.br/pesquisas-judiciarias/justica-em-numeros/) (CNJ) | indicadores oficiais de cada tribunal (ano-base 2025) | P1 e comparação entre TRTs |
| [API Pública do DataJud](https://datajud-wiki.cnj.jus.br/api-publica/) (CNJ) | amostra aleatória de **17.175 processos** de **22 TRTs** | P2 e fatores do processo (P3) |
| [Tabela Processual Unificada](https://www.cnj.jus.br/sgt/consulta_publica_classes.php) (CNJ) | árvore oficial de assuntos | agrupar assuntos em temas |

O **TRT2 (Grande SP)** e o **TRT5 (BA)** ficaram fora da amostra. A API do CNJ não conseguiu entregar a amostra deles: estourava o tempo até em consultas pequenas. Os dados oficiais cobrem os 24 TRTs.

## Ética e privacidade
Só usamos dados públicos do CNJ. A API pública não traz nomes das partes, e todos os processos da amostra são públicos (nível de sigilo 0). Na base tratada, o número do processo foi trocado por um código (hash). Detalhes na seção 9 do notebook.

## Estrutura

```
projeto8_tramitacao_congestionamento.ipynb   notebook principal (já executado)
figuras/                                     as 6 figuras (4 de análise + 2 do exercício de gráfico enganoso)
dados/brutos/                                dados como vieram das fontes
    jn2026_base_17-set-2026.zip              base do Justiça em Números (o notebook lê direto do zip)
    tpu_assuntos.json                        árvore de assuntos (TPU)
    datajud/                                 cache da amostra baixada da API (.json.gz)
dados/tratados/                              bases tratadas geradas pelo notebook (CSV ";" e vírgula decimal)
    jn_trt_2025.csv + dicionario_jn_trt.csv            indicadores oficiais dos 24 TRTs e o dicionário
    datajud_trt_amostra_2025.csv + dicionario_datajud_amostra.csv   amostra de processos e o dicionário
    relatorio_alteracoes.md                  o que foi alterado em cada base, passo a passo
arquivo_justica_estadual/                    primeira versão do projeto (Justiça Estadual), só para consulta
```

## Como rodar

**No Google Colab (sem instalar nada):** clique no botão "Abrir no Colab" acima e use *Ambiente de execução → Executar tudo*. A primeira célula de código baixa os dados deste repositório sozinha.

**No computador:**

```bash
pip install -r requirements.txt
jupyter notebook projeto8_tramitacao_congestionamento.ipynb   # depois: Run All
```

Roda em menos de 1 minuto. O notebook vem com `BAIXAR = False`, então usa só o cache em `dados/brutos/datajud/` e não acessa a API. Com `BAIXAR = True`, ele tenta baixar o que falta, o que pode levar muitas horas.
