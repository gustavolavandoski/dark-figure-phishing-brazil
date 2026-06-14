# A Cifra Negra do Cibercrime: Padrões de Subnotificação de Phishing no Brasil a partir dos Dados do CERT.br (2016–2025)

Repositório de replicação do artigo submetido à *Revista Pesquisa e Planejamento Econômico* (Instituto de Pesquisa Econômica Aplicada — IPEA), chamada especial sobre Economia do Crime no Brasil.

> **Citação:** Lavandoski da Silva, Luiz Gustavo. "The dark figure of cybercrime: underreporting patterns in Brazilian phishing incidents based on CERT.br data (2016–2025)." *Revista Pesquisa e Planejamento Econômico* (sob revisão). Dados e scripts de replicação: https://doi.org/10.5281/zenodo.20692595

---

## Visão Geral

Este repositório contém os dados e scripts utilizados para produzir todos os resultados empíricos, tabelas e figuras do artigo. A análise examina a série mensal de notificações de phishing ao CERT.br entre janeiro de 2016 e dezembro de 2025 (120 observações) por meio de três procedimentos econométricos complementares: decomposição STL, detecção endógena de quebras estruturais via PELT-Bai-Perron e regressão OLS log-linear com erros padrão HAC.

---

## Estrutura do Repositório

```
dark-figure-phishing-brazil/
├── README.md
├── README.pt-br.md
├── requirements.txt
├── data/
│   ├── raw/                        # Arquivos .txt brutos do CERT.br (ver Fontes de Dados)
│   └── serie_mensal.csv            # Série mensal consolidada (saída do 01_parse.py)
├── scripts/
│   ├── 01_parse.py                 # Parseia os arquivos brutos e gera serie_mensal.csv
│   └── 02_analise.py               # Análise empírica completa: STL, PELT, OLS, figuras
└── outputs/
    ├── tabela1_descritivas_anuais.csv
    ├── tabela2_componente_sazonal.csv
    ├── tabela3_quebras_estruturais.csv
    ├── tabela4_regressao.csv
    └── tabela5_efeitos_sazonais.csv
```

---

## Fontes de Dados

### Dados primários: CERT.br

A série mensal de notificações de incidentes é publicada em domínio público pelo CERT.br (NIC.br/CGI.br) e pode ser acessada em:

- https://stats.cert.br/incidentes/
- https://cert.br/stats/

Para cada ano de 2016 a 2025, baixe os arquivos abaixo e coloque-os em `data/raw/`:

| Arquivo | Conteúdo |
|---------|----------|
| `AAAA_incidentes-mensal.txt` | Total mensal de incidentes notificados |
| `AAAA_tipos-incidente-mensal.txt` | Incidentes por categoria (DoS, Fraude, Invasão, etc.) |
| `AAAA_tipos-fraude-mensal.txt` | Subcategorias de fraude (phishing, malware) |

Substitua `AAAA` pelo ano correspondente (2016, 2017, ..., 2025). Os arquivos são texto simples com colunas separadas por espaço e linhas de comentário iniciadas com `#`.

Caso prefira pular o parsing, o arquivo consolidado `data/serie_mensal.csv` já está incluído no repositório e pode ser usado diretamente como entrada para o `02_analise.py`.

### Fontes secundárias

As fontes secundárias utilizadas para triangulação qualitativa (Kaspersky, Febraban, DataSenado, CETIC.br, Seade) estão citadas no artigo e são publicamente acessíveis nos portais institucionais das respectivas organizações. Elas não são incorporadas ao modelo econométrico.

---

## Reproduzindo a Análise

### 1. Instalar dependências

```bash
pip install -r requirements.txt
```

### 2. (Opcional) Reprocessar os dados brutos

Necessário apenas se quiser reconstruir o `serie_mensal.csv` a partir dos arquivos originais do CERT.br:

```bash
python scripts/01_parse.py
```

O script espera os arquivos brutos em `data/raw/` e salva `serie_mensal.csv` em `data/`.

### 3. Executar a análise completa

```bash
python scripts/02_analise.py
```

Este script lê `data/serie_mensal.csv` e produz:

- As cinco tabelas de resultados em `outputs/`
- Todas as figuras em `figuras/` (PNG a 300 dpi e PDF)

A execução completa leva aproximadamente 30 a 60 segundos em um laptop padrão.

---

## Dependências

```
pandas>=1.5
numpy>=1.23
statsmodels>=0.14
ruptures>=1.1
matplotlib>=3.6
scipy>=1.10
```

Consulte o `requirements.txt` para as versões exatas utilizadas na análise original (Python 3.11).

---

## Principais Resultados

| Achado | Valor |
|--------|-------|
| Coeficiente de tendência (mensal) | −0,0154 (p = 0,007) |
| Declínio anualizado | ≈ −16,9% ao ano |
| Quebra estrutural endógena | Setembro de 2017 |
| Média mensal pré-quebra | 6.796 notificações |
| Média mensal pós-quebra | 2.562 notificações |
| R² ajustado (modelo completo) | 0,477 |
| Dummy pandemia | −0,302 (p = 0,004) |

---

## Licença

Os scripts deste repositório são disponibilizados sob a Licença MIT. Os dados do CERT.br são de domínio público. As fontes secundárias mantêm seus direitos autorais originais e não são reproduzidas aqui.

---

## Contato

Luiz Gustavo Lavandoski da Silva  
Doutorando em Economia Política Mundial, Universidade Federal do ABC (UFABC)  
gustavo.lavandoski@ufabc.edu.br
