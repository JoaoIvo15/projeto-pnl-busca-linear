# Projeto Computacional — Programação Não Linear

**Instituição:** Universidade Federal do Ceará (UFC)  
**Curso:** Bacharelado em Ciência de Dados  
**Disciplina:** Programação Não Linear  
**Professor:** Ricardo Coelho  
**Autor:** João Ivo Rocha Alves  
**Matrícula:** 581734  
**Modalidade:** Individual

## 1. Visão geral

Este projeto integra a modelagem de um problema real de otimização irrestrita à implementação e análise de métodos de busca linear em funções de teste clássicas.

O trabalho foi desenvolvido em duas frentes complementares: a formulação de um problema de mínimos quadrados a partir de retornos históricos de mercado e a realização de experimentos numéricos envolvendo três problemas de teste, três direções de busca e três métodos de determinação do comprimento do passo.

O código-fonte comentado está organizado em dois notebooks Jupyter, enquanto o relatório técnico apresenta a fundamentação matemática, a metodologia, os resultados, suas limitações e a auditoria crítica do uso de inteligência artificial.

## 2. Parte I — Modelagem de um problema real

A aplicação investiga a relação histórica entre os retornos percentuais da ação VALE3 e os retornos do Ibovespa, por meio de um modelo linear ajustado por mínimos quadrados.

O modelo adotado é

$$
\widehat{r}_t=\alpha+\beta m_t,
$$

em que $m_t$ representa o retorno observado do Ibovespa, $r_t$ representa o retorno observado da VALE3, $\alpha$ é o intercepto e $\beta$ é o coeficiente angular.

A função objetivo é

$$
f(\alpha,\beta)=
\frac{1}{2n}\sum_{t=1}^{n}
(\alpha+\beta m_t-r_t)^2.
$$

O problema foi formulado como uma minimização irrestrita em duas variáveis reais. Foram deduzidos analiticamente o gradiente e a matriz Hessiana, examinadas as propriedades de convexidade e verificada a existência de um único minimizador global.

A base utilizada contém 1.246 pares de retornos, no período de 4 de janeiro de 2021 a 30 de dezembro de 2025. Os dados foram preparados a partir de séries históricas de preços de fechamento obtidas por meio do Google Sheets e da função `GOOGLEFINANCE`. Os procedimentos de preparação e os ajustes pontuais realizados na base estão documentados no relatório técnico.

O primeiro passo de busca linear foi executado pela Regra de Armijo, reduzindo a função objetivo de aproximadamente $1{,}467227$ para $1{,}448836$.

A análise é descritiva: o modelo não estabelece causalidade, não constitui recomendação de investimento e não garante capacidade preditiva fora da amostra.

## 3. Parte II — Experimentos computacionais

Foram utilizados os três problemas de teste atribuídos, com seus respectivos pontos iniciais:

| Grupo | Problema | Ponto inicial |
|---|---|---|
| G1 | TP_3 | $(1,1,1)^T$ |
| G2 | TP_7 | $(15,-2)^T$ |
| G3 | TP_18 | $(2,2)^T$ |

Os métodos de busca linear implementados foram:

- **Grupo A:** Seção Áurea.
- **Grupo B:** Interpolação quadrática/cúbica protegida.
- **Grupo C:** Regra de Armijo.

Cada método foi combinado com três direções:

- Gradiente negativo;
- Newton clássico;
- Newton modificada ou regularizada.

A combinação dos três problemas, das três direções e dos três métodos resultou em **27 execuções de passo**.

### 3.1. Resultados gerais

Na execução final validada, foram obtidos os seguintes resultados:

| Verificação | Resultado |
|---|---:|
| Combinações executadas | 27 |
| Combinações únicas | 27 |
| Direções de descida válidas | 27 |
| Reduções estritas da função objetivo | 27 |
| Buscas por intervalo que atingiram a tolerância | 18 |
| Passos aceitos por Armijo | 9 |

Os resultados evidenciam diferenças entre as direções e os métodos. No TP_3, a direção de Newton clássica alcançou o minimizador da função quadrática em um passo. No TP_7, os comprimentos de passo e os custos obtidos variaram conforme a direção e o método. No TP_18, a escala da direção do gradiente negativo exigiu atenção à inicialização e à delimitação do intervalo de busca.

As execuções representam um passo a partir dos pontos iniciais especificados. A redução da função objetivo e o atendimento ao critério de parada não são interpretados automaticamente como convergência para um minimizador global.

## 4. Estrutura do repositório

```text
projeto-pnl-busca-linear/
├── notebooks/
│   ├── 01_modelagem_real.ipynb
│   └── 02_experimentos_pnl.ipynb
├── data/
│   └── market_data_ibov_vale3_2020_2025.csv
├── relatorio/
│   ├── Modelagem_e_Busca_Linear_em_Otimizacao_Irrestrita_Joao_Ivo.pdf
├── .gitignore
├── LICENSE
├── README.md
```

Os nomes apresentados correspondem à organização prevista para os arquivos finais; caso algum nome ou caminho no repositório seja diferente, deve-se atualizar esta representação para refletir a estrutura efetivamente publicada.

## 5. Entregáveis

**Notebook 1 — Modelagem do problema real:** [`notebooks/01_modelagem_real.ipynb`](notebooks/01_modelagem_real.ipynb)

Contém a preparação e a verificação dos dados, o cálculo dos retornos, a formulação da função objetivo, as deduções do gradiente e da Hessiana, a análise de convexidade e a execução do primeiro passo pela Regra de Armijo.

**Notebook 2 — Experimentos computacionais:** [`notebooks/02_experimentos_pnl.ipynb`](notebooks/02_experimentos_pnl.ipynb)

Contém as funções de teste, os gradientes e as Hessianas, o cálculo e a verificação das direções, as implementações dos três métodos de busca linear e a execução consolidada das 27 combinações.

**Relatório técnico:** [`relatorio/Modelagem_e_Busca_Linear_em_Otimizacao_Irrestrita_Joao_Ivo.pdf`](relatorio/Modelagem_e_Busca_Linear_em_Otimizacao_Irrestrita_Joao_Ivo.pdf)

Documento final em PDF. O relatório reúne a modelagem matemática, os experimentos, os resultados e suas limitações, o manual de uso dos notebooks, a matriz de responsabilidades individuais e o apêndice de transparência e auditoria crítica do uso de IA.

Os notebooks constituem o código-fonte executável e comentado do projeto. Não foi mantida uma terceira versão redundante das implementações em arquivos Python separados.

## 6. Reprodutibilidade e validação

Os notebooks foram organizados para execução sequencial em Jupyter ou Google Colab.

Para reproduzir os experimentos:

1. Acesse o repositório e abra o notebook correspondente.
2. Reinicie o ambiente de execução e execute as células de cima para baixo.
3. No Notebook 1, confira a base carregada, os retornos calculados, as estatísticas, o gradiente, a Hessiana, a convexidade e a condição de Armijo.
4. No Notebook 2, confira as funções e os pontos iniciais, a validação numérica das derivadas, os produtos direcionais e os resultados individuais dos três problemas.
5. Execute a célula de consolidação e verifique a quantidade de combinações, os estados das buscas e as reduções registradas.

As derivadas analíticas dos problemas de teste foram comparadas com aproximações por diferenças finitas centrais. Também foram verificadas as condições de descida, as dimensões dos vetores e das matrizes, a finitude dos resultados e a consistência das reduções da função objetivo.

A reprodução do Notebook 1 depende da disponibilidade da base de dados utilizada. A origem, o período, as transformações e os ajustes realizados no CSV estão documentados no relatório. A reutilização ou redistribuição dos dados deve respeitar as condições aplicáveis da fonte.

## 7. Transparência no uso de inteligência artificial

Ferramentas de inteligência artificial foram utilizadas como apoio à modelagem, à implementação, à revisão do código e à redação técnica.

O relatório contém um apêndice específico de auditoria, dividido em duas partes:

1. **Modelagem do problema real:** apoio à estruturação da função objetivo, revisão das deduções analíticas, interpretação da convexidade, organização do notebook e validação do primeiro passo de busca linear.
2. **Simulações computacionais:** apoio à implementação das direções e dos métodos de busca, revisão das saídas, análise de inconsistências numéricas e organização dos experimentos.

A auditoria documenta uma falha real identificada durante o desenvolvimento: a delimitação inadequada do intervalo inicial nas primeiras versões da Seção Áurea e da interpolação no TP_18. A inconsistência foi identificada comparando passos ao longo da mesma direção, e as rotinas foram corrigidas e revalidadas.

O registro distingue sugestões aceitas e verificadas de recomendações que precisaram de revisão. A responsabilidade pelas decisões metodológicas, execução, validação e interpretação dos resultados permanece com o autor.

## 8. Limitações

A modelagem financeira representa uma relação linear simplificada entre duas séries de retornos e não inclui variáveis adicionais, defasagens, avaliação fora da amostra ou identificação causal.

Os experimentos numéricos avaliam um passo a partir dos pontos iniciais atribuídos. Os resultados devem ser interpretados considerando as características das funções, as direções utilizadas, a delimitação dos intervalos e os critérios de parada de cada método. Uma redução estrita do custo não implica, por si só, convergência global.

Além disso, o comportamento observado nos pontos iniciais não permite generalizar automaticamente as propriedades locais para todo o domínio de cada função.

## 9. Considerações finais

O projeto integrou a formulação de um problema real de otimização irrestrita à implementação e avaliação de métodos de busca linear em problemas de teste clássicos.

A modelagem aplicada demonstrou a dedução das derivadas, a convexidade estrita e a redução do custo em um primeiro passo de busca. Os experimentos computacionais permitiram comparar as três direções e os três métodos, com registro dos resultados e validações numéricas.

Por fim, a documentação das limitações e a auditoria crítica do uso de IA reforçam a importância de verificar analiticamente as fórmulas, testar numericamente as implementações e interpretar os resultados com cautela.

---

**Autor:** João Ivo Rocha Alves  
**Matrícula:** 581734  
**Universidade Federal do Ceará — Bacharelado em Ciência de Dados**  
**Disciplina:** Programação Não Linear  
**Professor:** Ricardo Coelho
