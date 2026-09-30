# Framing do Conflito Israel-Palestina na Imprensa

Projeto de PLN que analisa como diferentes veículos de imprensa (EUA, Brasil, Oriente Médio e Ásia
Oriental) enquadram (*framing*) o conflito Israel-Palestina, a partir de um corpus de notícias
coletado via NewsAPI.

## Estrutura do repositório

```
├── notebooks/
│   ├── 01_coleta_corpus.ipynb              # Coleta via NewsAPI, extração, tradução
│   ├── 02_limpeza_regex.ipynb              # Limpeza do texto com expressões regulares
│   ├── 03_normalizacao_tokenizacao.ipynb   # Normalização e tokenização (spaCy)
│   ├── 04_stopwords_pos.ipynb              # Stopwords, lematização/stemming, POS tagging
│   ├── 05_analise_comparativa.ipynb        # Nuvens de palavras, adjetivos por lado, resumo
│   └── 06_bow_ngrams.ipynb                 # Bag of Words, N-grams, similaridade de cosseno
├── corpus/
│   ├── dados_brutos/          # Texto original por veículo (pt/en), antes da tradução
│   ├── dados_processados/     # Texto final em inglês (traduzido quando necessário)
│   ├── dados_limpos/          # Texto após limpeza por regex (saída do Notebook 02)
│   ├── metadados/             # CSVs/JSONs com id, veículo, grupo, url, etc., em cada etapa
│   └── resultados/            # Gráficos, tabelas e nuvens de palavras finais
├── relatorio_tecnico.docx
└── README.md
```

## Como rodar

Todos os notebooks foram feitos para o Google Colab.

1. Suba a pasta `corpus/` para o seu Google Drive (ou rode o Notebook 01 do zero para gerá-la).
2. Abra cada notebook no Colab, na ordem 01 → 06, e rode "Ambiente de execução → Executar tudo".
3. Cada notebook tenta localizar automaticamente a pasta do corpus dentro do seu Drive (procurando
   pelo arquivo de metadados esperado); se não encontrar, ajuste manualmente a variável `raiz`.
4. O Notebook 01 pede uma chave gratuita da [NewsAPI](https://newsapi.org/register) via prompt
   seguro (`getpass`) (não fica salva em nenhum arquivo)

## Pipeline

1. **Coleta** — busca de artigos por veículo/domínio na NewsAPI, extração do texto completo
   (`trafilatura`), detecção de idioma (`langdetect`) e tradução automática para inglês quando
   necessário (`deep-translator`, com fallback entre motores).
2. **Limpeza** — remoção de boilerplate, normalização de pontuação/aspas, tratamento de datas e de
   eventuais falhas de tradução, tudo via expressões regulares, com auditoria quantitativa de cada
   regra aplicada.
3. **Normalização e tokenização** — spaCy (`en_core_web_sm`).
4. **Stopwords, lematização e POS tagging** — spaCy; extração de adjetivos associados a cada lado
   do conflito, por sentença e por veículo.
5. **Análise comparativa** — nuvens de palavras, bigramas, tabela-resumo por veículo.
6. **Bag of Words / N-grams** — `CountVectorizer` (scikit-learn), similaridade de cosseno entre
   veículos, bigramas mais distintivos de cada um.

## Veículos no corpus

| Grupo | Veículos |
|---|---|
| EUA | CNN, Fox News, AP News |
| Brasil | UOL, Metrópoles, Terra |
| Oriente Médio | Al Jazeera, Anadolu Agency, Jerusalem Post |
| Ásia Oriental | Channel News Asia, Straits Times, South China Morning Post |

## Limitações conhecidas

Ver seção de limitações do `relatorio_tecnico.docx` — resumidamente: plano gratuito da NewsAPI
(janela de ~1 mês, cobertura desigual por veículo/idioma), um único artigo de erro de tradução
definitivo foi excluído do corpus em vez de forçado, e a comparação por "grupo/região" reflete no
máximo 3 veículos por grupo, não a imprensa de cada região como um todo.
