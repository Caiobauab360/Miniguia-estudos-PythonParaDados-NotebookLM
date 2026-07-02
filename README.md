# Miniguia de Estudos: Python para Análise de Dados com NotebookLM

# 📌 Contexto e Objetivos

Este projeto foi desenvolvido como parte do curso de Automação de Dados com IA e tem como objetivo explorar o uso do Google NotebookLM como ferramenta de aprendizagem ativa e consulta técnica. A proposta é criar um caderno temático que funcione como um assistente especializado para consulta de códigos e revisão de conceitos sobre Python voltado para análise de dados.

# 🎯 Objetivos do miniguia de Python para dados

O NotebookLM criado neste projeto tem como propósito principal servir como:

📝 Assistente de Consulta Rápida: Fornecer respostas imediatas sobre sintaxe, funções e boas práticas em Python para análise de dados;

💻 Banco de Códigos Referência: Disponibilizar exemplos práticos de código baseados no conteúdo do curso e documentações oficiais;

🔍 Ferramenta de Debugging: Auxiliar na identificação e correção de erros comuns em códigos de análise de dados;

📖 Material de Estudo: Oferecer explicações conceituais e contextuais quando necessário para aprofundamento;

🔄 Suporte a Revisão: Ajudar na revisão de tópicos específicos através de prompts direcionados e perguntas frequentes.

# 📂 Curadoria de Fontes

Para construir um NotebookLM confiável e especializado em consulta de códigos, realizei uma curadoria cuidadosa de fontes, combinando materiais do curso com documentações oficiais e livros especializados.

1. Portfólio do Curso de Análise de Dados com Python (Fontes Primárias em .PDF)
O coração do projeto são os materiais do meu curso de Analise de dados com Python realizado na plataforma do Datacamp, que incluem:
📓 Notas de aula: registros detalhados de cada módulo abordado

💻 Anotações de código: explicações sobre a lógica e funcionamento dos códigos

📝 Exercícios práticos: problemas resolvidos para fixação dos conceitos

📊 Projetos desenvolvidos: aplicações práticas dos conhecimentos adquiridos

🐛 Registros de erros: documentação de problemas enfrentados e soluções encontradas

2. Documentação Oficial do Python (Convertida para PDF)
Para utilizar a documentação oficial do Python como fonte no NotebookLM, foi necessário um processo de conversão, já que a documentação está disponível em formato HTML. O processo envolveu:

* Download da documentação HTML: Versão 3.14 disponível em docs.python.org;

* Conversão para PDF: Utilização de script Python com a biblioteca Playwright para converter cada arquivo HTML em PDF, mantendo a formatação e estrutura;

* Seleção de arquivos relevantes: Filtragem dos PDFs mais importantes para análise de dados, organizados por temas.

Script utilizado para conversão:
```python
import asyncio
from pathlib import Path
from playwright.async_api import async_playwright

INPUT_DIR = Path(r"C:\caminho\python-3.14-docs-html")
OUTPUT_DIR = Path(r"C:\caminho\python_docs_pdf")

async def main():
    OUTPUT_DIR.mkdir(parents=True, exist_ok=True)
    html_files = list(INPUT_DIR.glob("**/*.html"))
    
    async with async_playwright() as p:
        browser = await p.chromium.launch()
        page = await browser.new_page()
        
        for html_path in html_files:
            out_path = OUTPUT_DIR / html_path.relative_to(INPUT_DIR).with_suffix(".pdf")
            out_path.parent.mkdir(parents=True, exist_ok=True)
            await page.goto(html_path.resolve().as_uri())
            await page.pdf(path=str(out_path), format="A4")
            print(f"✅ Convertido: {html_path.name}")
        
        await browser.close()

asyncio.run(main())
```
Arquivos selecionados da documentação Python:

* Tutorial: Guia introdutório completo (8 arquivos);

* Biblioteca Essencial: Módulos como csv, json, datetime, math, statistics (15 arquivos);

* Referência: Estrutura da linguagem, expressões e sintaxe (5 arquivos);

* HowTo: Guias práticos de uso (4 arquivos).

3. Bibliotecas Essenciais para Análise de Dados
Para complementar a documentação padrão do Python, foram adicionadas 13 fontes especializadas, formando a base do ecossistema de análise de dados:

| Biblioteca | Tipo | Fonte/Referência |
|------------|------|------------------|
| NumPy | Numérico | [MIT NumPy Book](https://web.mit.edu/dvp/Public/numpybook.pdf) |
| SciPy | Científico | [Documentação Oficial](https://static.scipy.org/doc/scipy-1.8.0/scipy-ref-1.8.0.pdf) |
| Pandas | Dados | [Tabulares Python Data Analysis Book](https://python-course.eu/books/bernd_klein_python_data_analysis_a4.pdf) |
| Matplotlib | Visualização | [Documentação Oficial](https://matplotlib.org/stable/matplotlib.pdf) |
| Seaborn | Visualização | [Resumo GitHub](https://github.com/wangyingsm/Python-Data-Science-Handbook/blob/master/printable/04_14_Visualization-with-Seaborn.pdf) |
| Scikit-learn | Machine Learning | [Documentação Oficial](https://scikit-learn.org/0.22/__downloads/scikit-learn-docs.pdf) |
| Statsmodels | Estatística | [PDF 2014 - Perkold/Seabold](https://www.statsmodels.org/stable/index.html) |
| Requests | HTTP | Documentação disponível via biblioteca padrão |
| SQLAlchemy | Banco de Dados | Documentação disponível via biblioteca padrão |
| Plotly | Visualização Interativa | [Cheat Sheet](https://images.plot.ly/plotly-documentation/images/plotly_js_cheat_sheet.pdf) |
| Dask | Big Data | [Guia GitHub](https://raw.githubusercontent.com/SupareoDataScience/DE/master/readings/dask.pdf) |
| PyTorch | Deep Learning | [Livro](https://github.com/liberty/Deep-Learning-with-PyTorch.pdf) |
| TensorFlow | Deep Learning | [Livro](https://www.hlevkin.com/levkin/45MachineDeepLearning/DL/TensorFlow-for-Machine-Intelligence-A-Hands-On-Introduction-to-Learning-Algorithms.pdf) |
| NLTK | NLP | [Livro](https://t.jzhifeli.github.io/resources/NLTK.pdf) |
| spaCy | NLP | [Documentação](https://spacy.readthedocs.io/__downloads/en/latest/pdf/) |

# ⚙️ Engenharia de Prompts e "Cicatrizes": Refinando o Assistente de Código
É importante para este tipo projeto o processo iterativo de testar, errar e refinar prompts para transformar o NotebookLM em um assistente de código verdadeiramente útil. Mais do que apenas fazer perguntas, a engenharia de prompts envolveu entender como a IA "pensa" e como adaptar nossas perguntas para obter respostas precisas, contextualizadas e acionáveis.

Estudo de Caso 1: Consulta de Sintaxe Básica
🟡 Prompt Inicial (V1) - Resultado Insatisfatório
Prompt:

"Como usar o groupby no Pandas?"

Resposta Obtida:

"O groupby é uma função do Pandas que permite agrupar dados. Você pode usar df.groupby('coluna').sum() para somar os valores. Existem várias funções de agregação como mean(), count(), etc."

Análise da "Cicatriz":

❌ Resposta muito genérica e superficial

❌ Não utilizou as fontes carregadas (documentação e materiais do curso)

❌ Faltou contexto sobre quando e por que usar

❌ Sem exemplos práticos ou código executável

🟢 Prompt Refinado (V2) - Resultado Excelente
Prompt:

"Atue como um assistente de código Python especializado em análise de dados. Com base EXCLUSIVAMENTE no material do curso e nas documentações carregadas, forneça um guia completo sobre o uso do groupby() no Pandas. Sua resposta deve incluir:

A estrutura básica da sintaxe com todos os parâmetros principais

Três exemplos práticos em níveis crescentes de complexidade (básico, intermediário e avançado)

Um caso de uso real que mostre a aplicação em análise de dados

Os principais cuidados e erros comuns ao usar groupby

Código completo com comentários explicativos em português

Formato: Código Python com comentários, seguido de explicações breves para cada exemplo."

Resposta Obtida:
Resposta detalhada com exemplos de código, referências às fontes e aplicações práticas.

O que funcionou:

✅ Persona clara: "assistente de código Python especializado" direcionou o tom técnico

✅ Âncora nas fontes: "EXCLUSIVAMENTE no material do curso" eliminou alucinações

✅ Estrutura detalhada: Os 5 itens guiaram a IA para uma resposta completa

✅ Formato específico: "Código com comentários" gerou material pronto para uso

✅ Progressão de complexidade: Garantiu que tanto iniciantes quanto avançados se beneficiassem

Estudo de Caso 2: Debugging e Correção de Erros
🟡 Prompt Inicial (V1) - Resposta Frustrante
Prompt:

"Estou tendo erro KeyError no meu código. O que fazer?"

Resposta Obtida:

"KeyError ocorre quando você tenta acessar uma chave que não existe no dicionário ou DataFrame. Verifique se a chave está correta."

Análise da "Cicatriz":

❌ Resposta genérica que não resolveu o problema específico

❌ Não considerou que o erro poderia ter múltiplas causas

❌ Faltou um roteiro prático de diagnóstico

❌ A IA assumiu que eu sabia onde estava o erro

🟢 Prompt Refinado (V2) - Guia de Diagnóstico
Prompt:

"Estou recebendo um KeyError ao tentar acessar a coluna 'vendas' no meu DataFrame. Com base nas fontes carregadas sobre tratamento de erros e boas práticas no Pandas, crie um guia passo a passo para diagnosticar e resolver esse problema. Inclua:

Uma lista de verificação (checklist) com as 5 causas mais comuns para KeyError em DataFrames

Um código de diagnóstico que mostre como inspecionar as colunas disponíveis e seus tipos

Duas soluções alternativas (usando .get() e try/except)

Como prevenir esse erro no futuro com validação de dados

Exemplos de mensagens de erro personalizadas para facilitar o debugging

Estruture sua resposta como um guia de troubleshooting para um analista de dados."

Resposta Obtida:
Resposta detalhada com checklist prático, código de diagnóstico e estratégias de prevenção.

O que funcionou:

✅ Problema específico: "coluna 'vendas'" deu contexto real

✅ Formato de guia: "passo a passo" e "checklist" organizaram a resposta

✅ Múltiplas soluções: Ofereceu alternativas em vez de uma única resposta

✅ Foco preventivo: Incluiu como evitar o erro no futuro

✅ Persona de analista: O tom prático foi mais útil que um tom teórico

Estudo de Caso 3: Otimização de Performance
🟡 Prompt Inicial (V1) - Resposta Vaga
Prompt:

"Meu código com Pandas está muito lento. Como otimizar?"

Resposta Obtida:

"Use operações vetorizadas em vez de loops. Evite usar apply() quando possível. Use métodos como .loc[] para seleção."

Análise da "Cicatriz":

❌ Resposta vaga e sem exemplos concretos

❌ Não considerou o contexto do código

❌ Faltou comparação de performance (antes/depois)

❌ Sem métricas ou estimativas de ganho

🟢 Prompt Refinado (V2) - Otimização com Métricas
Prompt:

"Tenho um DataFrame com 2 milhões de linhas e estou usando um loop for para calcular uma nova coluna baseada em condições. O código está demorando mais de 5 minutos para executar. Com base nas técnicas de otimização do curso, forneça:

O código original ineficiente (que estou usando)

Pelo menos 3 alternativas otimizadas, com explicação de por que cada uma é mais rápida

Uma comparação de performance estimada (tempo de execução) para cada alternativa

Quando cada abordagem é mais adequada (trade-offs)

Um exemplo de código final que eu possa copiar e adaptar

Inclua comentários no código explicando cada otimização."

Resposta Obtida:
Resposta com código comparativo, estimativas de performance e recomendações baseadas no tamanho dos dados.

O que funcionou:

✅ Contexto quantitativo: "2 milhões de linhas" e "5 minutos" deram escala real

✅ Múltiplas alternativas: Ofereceu opções em vez de uma única solução

✅ Métricas de performance: Estimativas de tempo ajudaram na decisão

✅ Trade-offs explícitos: Explicou quando usar cada abordagem

✅ Código pronto: O formato final gerou material utilizável imediatamente

Estudo de Caso 4: Visualização de Dados
🟡 Prompt Inicial (V1) - Resposta Incompleta
Prompt:

"Como criar um gráfico de barras no Matplotlib?"

Resposta Obtida:

"Use plt.bar(x, y) para criar um gráfico de barras. Você pode personalizar com plt.title() e plt.xlabel()."

Análise da "Cicatriz":

❌ Código sem contexto de dados

❌ Não mostrou como preparar os dados para o gráfico

❌ Faltou personalização estética (cores, tamanhos, etc.)

❌ Sem instruções para salvar a figura

🟢 Prompt Refinado (V2) - Guia Visual Completo
Prompt:

"Preciso criar um gráfico de barras mostrando a distribuição de vendas por categoria de produto. Meus dados estão em um DataFrame do Pandas com as colunas 'categoria' e 'vendas'. Com base no material do curso sobre visualização, crie:

O código completo para gerar o gráfico com Matplotlib

Uma versão alternativa usando Seaborn (mais estilosa)

Personalizações: título, rótulos, cores, tamanho da figura, rotação dos labels

Como salvar a figura em alta resolução (300 dpi)

Um exemplo de como adicionar anotações ou valores sobre as barras

O código para criar uma versão horizontal do gráfico

Forneça o código completo com comentários explicativos e mostre o resultado esperado."

Resposta Obtida:
Resposta com código completo para Matplotlib e Seaborn, personalizações e dicas de design.

O que funcionou:

✅ Contexto dos dados: "colunas 'categoria' e 'vendas'" deu realismo

✅ Múltiplas bibliotecas: Ofereceu alternativas (Matplotlib vs Seaborn)

✅ Personalização completa: Incluiu ajustes estéticos profissionais

✅ Dicas de design: Rotação de labels e alta resolução

✅ Variações: Incluiu versão horizontal e anotações

📚 Resumo das "Cicatrizes" e Lições Aprendidas

| Desafio Encontrado | Sintoma | Solução Aplicada | Prompt Refinado |
|--------------------|---------|------------------|-----------------|
| Respostas genéricas | IA dá definições teóricas sem exemplos | Adicionar persona e âncora nas fontes | "Atue como assistente de código. Com base nas fontes..." |
| Código sem contexto | Código solto, sem explicação de uso | Solicitar comentários e contexto do problema | "Inclua comentários explicativos em cada linha..." |
| Falta de exemplos | Resposta conceitual, sem aplicação prática | Exigir exemplos reais com dados fictícios | "Forneça 3 exemplos práticos com dados fictícios..." |
| Sem progressão | Todos exemplos no mesmo nível | Pedir complexidade crescente | "Exemplos em níveis: básico, intermediário e avançado" |
| Falta de alternativas | Apenas uma solução apresentada | Solicitar múltiplas abordagens | "Forneça pelo menos 3 alternativas com trade-offs" |
| Respostas desconectadas das fontes | IA inventa informações | Ancorar explicitamente nas fontes | "Com base EXCLUSIVAMENTE nas fontes carregadas..." |
| Estrutura da resposta incorreta | Resposta em texto corrido | Especificar formato de saída | "Estrutura como: checklist, código, explicação" |

💡 Guia Prático: Como Extrair o Melhor do Seu NotebookLM
Com base nas cicatrizes acumuladas, aqui está meu fluxo de trabalho recomendado para consultas futuras:
1. Seja específico sobre o problema: Quanto mais contexto, melhor a resposta;
2. Ancore nas fontes: Diga explicitamente para a IA usar apenas o material carregado;
3. Estruture a resposta esperada: Defina formato, nível de detalhe e elementos obrigatórios;
4. Peça múltiplas perspectivas: Solicite alternativas, trade-offs e casos de uso;
5. Itere: Se a primeira resposta não for satisfatória, refine e pergunte novamente;
6. Documente os prompts que funcionaram: Crie seu próprio repositório de prompts reutilizáveis;

# 📖 Miniguia de Estudo: Python para Análise de Dados

📝 Resumos Estruturados para Consulta Rápida
1. Guia de Importação de Bibliotecas
```python
# Importação padrão para análise de dados
import pandas as pd          # Manipulação de dados
import numpy as np           # Computação numérica
import matplotlib.pyplot as plt  # Visualização básica
import seaborn as sns        # Visualização estatística

# Configurações úteis
pd.set_option('display.max_columns', None)  # Mostrar todas as colunas
pd.set_option('display.max_rows', 100)      # Limitar linhas exibidas
```
2. Operações Essenciais com Pandas

| Operação | Código | Descrição |
|----------|--------|-----------|
| Carregar dados | `pd.read_csv('arquivo.csv')` | Ler CSV para DataFrame |
| Explorar dados | `df.head()`, `df.info()`, `df.describe()` | Primeiras linhas, informações, estatísticas |
| Selecionar dados | `df['coluna']`, `df[['col1', 'col2']]` | Selecionar colunas |
| Filtrar dados | `df[df['col'] > 10]` | Filtro condicional |
| Agrupar dados | `df.groupby('categoria').mean()` | Agrupar e agregar |
| Combinar dados | `pd.merge(df1, df2, on='chave')` | Mesclar por coluna |
| Tratar dados faltantes | `df.dropna()`, `df.fillna(valor)` | Remover ou preencher NaN |

3. Operações Essenciais com NumPy
```python
# Criação de arrays
arr = np.array([1, 2, 3, 4, 5])        # Array 1D
zeros = np.zeros((3, 3))                # Array de zeros
ones = np.ones((3, 3))                  # Array de uns

# Operações básicas
arr.mean()          # Média
arr.std()           # Desvio padrão
arr.sum()           # Soma
```

📚 Glossário de Termos e Conceitos (Foco em Código)

| Termo | Definição | Exemplo de Código |
|-------|-----------|-------------------|
| DataFrame | Estrutura bidimensional do Pandas | `df = pd.DataFrame({'A': [1,2], 'B': [3,4]})` |
| GroupBy | Agrupamento para agregação | `df.groupby('categoria')['valor'].sum()` |
| Merge | Combinação de DataFrames por coluna | `pd.merge(df1, df2, on='chave')` |
| Vectorization | Operações sem loops | `df['col'] * 2` (vs loop) |
| NaN | Valor ausente | `df.isna()`, `df.fillna(0)` |

# 🔄 Conjunto de Prompts Reutilizáveis para Consulta de Código

🎯 Prompts por Categoria

1. Consulta Rápida de Sintaxe


📌 Prompt: "Atue como um assistente de código Python especializado em análise de dados. Com base no material do curso, forneça a sintaxe da função [nome da função] no Pandas/NumPy com: (a) estrutura básica, (b) parâmetros principais, (c) um exemplo prático com dados fictícios, (d) saída esperada. Inclua comentários no código."

3. Solução de Problemas Específicos


📌 Prompt: "Preciso resolver o seguinte problema de análise de dados: [descrever o problema com detalhes]. Com base no material do curso: (a) qual a abordagem recomendada, (b) código passo a passo para solução, (c) explicação de cada etapa, (d) possíveis variações ou alternativas."

4. Otimização de Código


📌 Prompt: "O seguinte código está lento para processar [quantidade] de dados: [inserir código]. Com base nas boas práticas do curso, sugira otimizações. Forneça: (a) código original com problemas identificados, (b) código otimizado, (c) explicação das melhorias, (d) estimativa de ganho de performance."

5. Debugging e Tratamento de Erros


📌 Prompt: "Estou recebendo o erro [descrição do erro] ao executar: [código que gera erro]. Com base no material do curso sobre debugging: (a) qual a causa provável, (b) como corrigir, (c) código corrigido, (d) como evitar esse erro no futuro."

6. Visualização de Dados


📌 Prompt: "Preciso criar uma visualização para [descrever dados e objetivo]. Com base no material do curso sobre Matplotlib/Seaborn: (a) qual tipo de gráfico é mais adequado, (b) código completo para criar o gráfico, (c) personalizações recomendadas, (d) como salvar em alta qualidade."

# 🏁 Conclusão
Este projeto transformou o NotebookLM em um assistente especializado para consulta de códigos Python voltados para análise de dados. A curadoria cuidadosa de fontes, combinando materiais do curso, documentação oficial convertida e livros especializados, criou uma base de conhecimento robusta e confiável.

🛠️ Ferramentas Utilizadas


* Google NotebookLM: Curadoria de fontes e assistente de código

* Playwright: Conversão de HTML para PDF no VS Code

* Python: Scripts de automação para processamento de documentos

* GitHub: Versionamento e portfólio do projeto


📌 Projeto desenvolvido como parte do curso de Automação de Dados com IA


🔧 Ferramentas: Google NotebookLM, Python, Playwright, VS Code


📅 Período: 2026


👤 Autor: Caio Pereira Bauab


🔗 Repositório:(https://github.com/Caiobauab360?tab=repositories)
