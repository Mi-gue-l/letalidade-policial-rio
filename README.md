# Vítimas de intervenção policial no Município do Rio de Janeiro (1º quadrimestre, 2003–2022)

[![Código: MIT](https://img.shields.io/badge/c%C3%B3digo-MIT-blue)](LICENSE)
[![Dados e texto: CC BY 4.0](https://img.shields.io/badge/dados%20e%20texto-CC%20BY%204.0-lightgrey)](LICENSE-DATA.md)
[![FAIR](https://img.shields.io/badge/FAIR-F%20A%20I%20R-green)](#princípios-fair)
![Python 3.13](https://img.shields.io/badge/python-3.13-blue)

> **Artigo (GitHub Pages):** https://mi-gue-l.github.io/letalidade-policial-rio/

**Autor:** Miguel Garcia Lopes do Amaral · **Professor orientador:** Alexandre Neves Louzada · **Curso:** Bacharelado em Sistemas de Informação (6º período) · **Disciplina:** Linguagem de Programação (Análise e Visualização de Dados com Python) · **Instituição:** Unilasalle RJ

## Resumo executivo
Análise exploratória e totalmente reprodutível da série oficial de vítimas de oposição à intervenção policial (antes “autos de resistência”) no Município do Rio de Janeiro, no 1º quadrimestre (jan–abr) de 2003 a 2022 (20 observações). Três perguntas de pesquisa:

* **RQ1** — Há tendência monotônica de queda ou mudanças de patamar?
* **RQ2** — A normalização pela população (taxa por 100 mil hab.) altera a leitura da série?
* **RQ3** — Quais variações interanuais são atípicas, inclusive frente ao ruído de Poisson?

**Achados centrais:** o total cai de 291 para 123 (−57,7 %), mas **não há tendência monotônica significativa** (τ = −0,16; p = 0,33). Há uma **quebra de nível em 2011** (média 243 → 148; p de permutação = 0,014), com retorno a patamar elevado em 2019–2021 e recuo em 2022. A taxa por 100 mil cai −63,4 % (a população cresceu 15,6 %), mas preserva o padrão temporal (ρ = 0,97). 6 das 19 variações anuais excedem o ruído de Poisson; nenhuma é atípica por Tukey/z modificado. **Os resultados são descritivos; não permitem inferência causal.**

**Palavras-chave:** letalidade policial; Rio de Janeiro; séries temporais; análise exploratória de dados; dados abertos; FAIR.

## Fonte dos dados (link perene)
* Instituto de Segurança Pública do RJ (ISP-RJ): <http://www.ispdados.rj.gov.br/>
* DATA.RIO / Instituto Pereira Passos — *Tabela 2300*: <https://www.data.rio/> (consulta ao ISP registrada em 24/05/2022)
* Arquivo bruto versionado: `data/raw/tabela_2300_data_rio.xls` — SHA-256 `179553629368d5962295634ed9eeb118cb0b1188d6ddf7f02a854182fa7b2d35`

## Estrutura do repositório
```
.
├── README.md  LICENSE  LICENSE-DATA.md  CITATION.cff  requirements.txt  .gitignore
├── data/
│   ├── raw/tabela_2300_data_rio.xls          # bruto, intocado
│   ├── processed/                            # csv.gz + json + etl_log.json + metricas_agregadas.json
│   └── data_dictionary.md                    # dicionário de variáveis (FAIR)
├── notebooks/analise_exploratoria.ipynb      # EDA com saídas executadas
├── src/
│   ├── etl.py                                # ingestão, limpeza, validação, persistência
│   ├── analysis.py                           # métricas e testes estatísticos
│   ├── figures.py                            # figuras estáticas
│   ├── build_article.py + templates/         # gera docs/index.html a partir dos resultados
│   ├── notebook_tools.py                     # executa o notebook sem Jupyter
│   ├── check_repo.py                         # verificação final (FAIR/estrutura)
│   └── project_meta.json                     # autoria e URLs usadas no artigo
├── tests/test_pipeline.py                    # testes automatizados (unittest)
└── docs/                                     # artigo publicado via GitHub Pages
    ├── index.html
    └── assets/                               # figuras PNG, CSV tratado, dicionário
```

## Como reproduzir
```bash
git clone https://github.com/Mi-gue-l/letalidade-policial-rio.git
cd letalidade-policial-rio
python -m venv .venv && source .venv/bin/activate     # Windows: .venv\Scripts\activate
pip install -r requirements.txt

python -m src.etl              # data/raw -> data/processed
python -m src.analysis         # metricas_agregadas.json
python -m src.figures          # docs/assets/*.png
python -m src.build_article    # docs/index.html
python -m unittest discover -s tests -v
python -m src.check_repo       # verificação final
```
Notebook: abra `notebooks/analise_exploratoria.ipynb` (JupyterLab: `pip install jupyterlab`) ou reexecute sem Jupyter com `python -m src.notebook_tools notebooks/analise_exploratoria.ipynb`.

## Publicar no GitHub Pages
1. Suba o repositório (público) para o GitHub.
2. *Settings → Pages → Build and deployment → Source: Deploy from a branch → Branch: `main` / folder: `/docs`*.
3. Aguarde ~1 min; a página ficará em `https://mi-gue-l.github.io/letalidade-policial-rio/`.
4. Ajuste `src/project_meta.json` (instituição, URLs), rode `python -m src.build_article` e faça commit de `docs/`.

## Princípios FAIR
| Princípio | Como é atendido |
|---|---|
| **Findable** | Este README (resumo, palavras-chave, fonte), `CITATION.cff`, metadados *schema.org/Dataset* no artigo, autoria no cabeçalho |
| **Accessible** | Repositório público; ingestão sem autenticação; artigo estático responsivo sem dependências externas |
| **Interoperable** | `data/data_dictionary.md`; dados em `.csv.gz` e `.json` |
| **Reusable** | MIT (código) + CC BY 4.0 (dados/texto); `requirements.txt` com versões pinadas; testes; `etl_log.json` com hashes |

## Limitações
Apenas 1º quadrimestre; agregado municipal; registros policiais com classificação provisória; n = 20; regimes definidos *a posteriori*; sem inferência causal. Detalhes no artigo (seção 4) e no dicionário.

## Licenças
Código: [MIT](LICENSE). Dados tratados, figuras e texto: [CC BY 4.0](LICENSE-DATA.md). Dados originais: termos do ISP-RJ/DATA.RIO.
