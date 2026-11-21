# Detecção de Spam e Phishing — Classificação de Emails

[![Python](https://img.shields.io/badge/Python-3.11%2B-blue)](https://www.python.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.5%2B-orange)](https://scikit-learn.org/)
[![NLTK](https://img.shields.io/badge/NLTK-3.9%2B-green)](https://www.nltk.org/)
[![Licença](https://img.shields.io/badge/Licen%C3%A7a-MIT-lightgrey)](LICENSE.md)

Pipeline de **NLP end-to-end** que distingue emails de spam/phishing de emails legítimos, treinado sobre **100.000 emails reais de 2024**. Construído de raiz no Databricks, célula a célula, e pronto a correr em qualquer ambiente Python (Jupyter, Databricks, Kaggle).

▶ **Versão interativa no Kaggle:** [Deteção de Spam e Phishing em Emails — NLP do Zero](https://www.kaggle.com/code/gonaldomichael/dete-o-de-spam-e-phishing-em-emails-nlp-do-zero)

## Dataset

Kaggle [`isuranga/spammalicious-detection`](https://www.kaggle.com/datasets/isuranga/spammalicious-detection) ("Email Classification Data"):

* **100.000 emails reais de 2024**, 30 colunas
* Texto completo, assunto, remetente, domínio, nº de links, anexos, tokens de rastreio, score SpamAssassin, autenticação SPF/DKIM/DMARC
* **Multilingue**: inglês (60%), francês, italiano, espanhol, alemão
* Etiqueta binária: `spam` (50%) / `legítimo` (50%)

O download é feito no próprio notebook com `kagglehub`, **sem chave de API**.

## Estrutura do notebook

| Bloco | Conteúdo |
| --- | --- |
| STEP 0 | Instalação de dependências (kagglehub, nltk) |
| STEP 1 | Download do dataset e carregamento em pandas |
| STEP 2 / 2B | Análise exploratória (EDA) e inspeção da estrutura |
| STEP 3 | Divisão treino/teste estratificada 80/20 — antes de qualquer transformação, para evitar *data leakage* |
| STEP 4 | Limpeza de texto multilingue com NLTK |
| STEP 5 | Features: TF-IDF (5.000 termos, uni+bigramas) + 23 comportamentais |
| STEP 6 | Treino: Naive Bayes e Random Forest, com validação cruzada no treino |
| P1–P7 | Sete perguntas de negócio respondidas com tabelas e conclusões |
| STEP 7 | Avaliação final no teste + teste de honestidade |
| Conclusão | Resumo, respostas e limitações |

## Resultados

Nos 20.000 emails de teste (nunca vistos), os dois modelos:

| Modelo | F1 | Acurácia | Recall | Precisão |
| --- | --- | --- | --- | --- |
| Naive Bayes | 1,000 | 1,000 | 1,000 | 1,000 |
| Random Forest | 1,000 | 1,000 | 1,000 | 1,000 |

Metas do projeto (F1 ≥ 0,90 | Acurácia ≥ 0,90 | Recall ≥ 0,85): todas cumpridas. Features mais importantes do Random Forest: `x_spam_score` (14%), `num_phone_numbers`, `num_urls`.

## As sete perguntas de negócio

| # | Pergunta | Resposta |
| --- | --- | --- |
| P1 | Quanto spam há? | 50% (50.000 spam, 50.000 legítimos) |
| P2 | Que temas dominam? | Spam: urgência e prémios (verify, offer, limited). Legítimo: trabalho de equipa (thanks, review, attached) |
| P3 | Que domínios enviam mais spam? | Taxa: icloud.com (80,4%). Volume: proton.me (5.896) |
| P4 | Mais links, mais spam? | Sim — 100% dos emails com links são spam; 0% dos que não têm |
| P5 | O score SpamAssassin explica a etiqueta? | Sim — correlação 0,926; a etiqueta parece derivar do score |
| P6 | Anexos tornam o email suspeito? | Sim — 62,4% de spam com anexos contra 46,9% sem |
| P7 | Alguma língua tem mais spam? | Alemão (51%), mas a diferença entre línguas é de apenas 1,7 pontos |

## Como reproduzir

1. Instalar as dependências: `pip install -r requirements.txt`
2. Executar o notebook de topo a fundo — o dataset é descarregado automaticamente (sem credenciais)
3. Tempo de execução: ~5 minutos em máquina standard (CPU)
4. **No Kaggle:** anexar o dataset ao notebook (**Add Input** → *Email Classification Data*, de isuranga) e ativar a **Internet** nas definições — é o que faz a [versão publicada](https://www.kaggle.com/code/gonaldomichael/dete-o-de-spam-e-phishing-em-emails-nlp-do-zero)

## Uma nota de honestidade

O F1 perfeito tem uma explicação: a etiqueta deste dataset foi muito provavelmente criada a partir do score do SpamAssassin, e os padrões são determinísticos (nenhum email legítimo tem links; os temas das duas classes quase não se cruzam). Até um Naive Bayes treinado apenas com o texto atinge F1 = 1,000. Em emails reais, ninguém alcança este resultado — o valor deste projeto está no **pipeline**: limpeza multilingue, vetorização, features comportamentais e avaliação rigorosa, prontos a reaproveitar em dados reais.

## Requisitos

Ver `requirements.txt` (kagglehub, nltk, scikit-learn, pandas, numpy).

## Licença

[MIT](LICENSE.md) — © 2026 phdprojects