# Relatório Final de Projeto P2 - Ciência de Dados em Saúde

# Projeto de Predição de Prognóstico de Mortalidade com Dados Sintéticos

# 1. Apresentação

O presente projeto foi originado no contexto das atividades da disciplina de pós-graduação [*Ciência e Visualização de Dados em Saúde*](https://ds4h.org), oferecida no primeiro semestre de 2022, na Unicamp e elaborado pelos seguintes alunos:

| Nome                          | RA     | Especialização |
| ----------------------------- | ------ | -------------- |
| Bruna Osti                    | 231024 | Computação     |
| Fabio Fogliarini Brolesi      | 023718 | Computação     |
| Ingrid Alves de Paiva Barbosa | 182849 | Computação     |

# 2. Contextualização da Proposta

## 2.1. Estudo de prognósticos

A área de pesquisa de prognósticos busca entender e melhorar os resultados de prognósticos em pessoas com uma determinada doença ou condição de saúde. O objetivo geral de estudos prognósticos em contextos clínicos é ajudar clínicos, pacientes e familiares a tomar decisões esclarecidas a respeito de cuidados de saúde com base em informações disponíveis sobre cada paciente no presente para prever desfechos no futuro [1]. Além disso, ajuda os pacientes e os familiares a tomar decisões adequadas a respeito do fim da vida daqueles cujo risco de morte é muito alto e a identificar intervenções personalizadas para evitar futuras hospitalizações [2].

Os modelos prognósticos usam vários fatores em combinação para prever o risco de resultados clínicos futuros em pacientes. Um bom modelo deve (i) fornecer previsões precisas que informam os pacientes e seus cuidadores, (ii) apoiar a pesquisa clínica e (iii) permitir decisões para melhorar os resultados dos tratamentos aos pacientes [1]. Um modelo prognóstico tem três fases principais: desenvolvimento do modelo (incluindo validação interna), validação externa e investigações de impacto na prática clínica. Embora muitos modelos prognósticos sejam propostos, poucos são atualmente usados na prática clínica [1].

Entre os prognósticos existentes, alguns são apresentados abaixo:

* Simplified Acute Physiology Score III (SAPS3): Considera a idade, se já vêm de pré-hospitalização, qual a localização anterior (urgência, UCI e outros), se possui alguma comorbidade, qual o motivo da entrada, e variáveis medidas no momento do atendimento, para gerar a mortalidade prevista [3]. 
* Palliative Prognostic Index (PPI): Prediz a mortalidade em pacientes terminais com base em cinco critérios, sendo eles a escala de desempenho paliativo, a ingestão oral, presença de edemas, se há dispneia em repouso ou se há delírios. Com base nestes parâmetros, é informado em até quantas semanas é esperado que este paciente venha à óbito [4].
* Palliative Prognostic Score (PaP): Semelhante ao anterior, também foca em pacientes em cuidados paliativos. Neste caso os parâmetros de entrada são Dispneia, Anorexia, Karnofsky Performance Status, Previsão Clínica de Sobrevivência (semanas), Total de WBC e Linfócitos %, para predizer a probabilidade de sobrevivência em 30 dias [5]. 
* MAGGIC Risk Calculator for Heart Failure (MAGGIC RCHF): Este prognóstico foca apenas em pessoas que sofrem da condição de insuficiência cardíaca, e com base nos parâmetros de entrada, estima a mortalidade em 1 e 3 anos [6].

## 2.2. Proposta de projeto

A proposta deste projeto é montar um ou mais modelos de prognóstico que realizem a predição de mortalidade de pacientes sintéticos gerados em pelo menos dois cenários de dados fictícios. Será necessário estabelecer os parâmetros de predição, definir quais os dados sobre o paciente que serão usados para a predição, construir modelos de aprendizagem de máquina que realizem predições e apresentar o resultado do modelo de predição aplicado.

Ao analisar os prognósticos apresentados na seção 2.1, percebeu-se que a maioria deles foca em um determinado cenário, e que há entradas e saídas muito bem definidas e apresentadas de forma clara para que o médico e a família possam interpretar. Diante deste cenário, a pergunta de pesquisa levantada então para este projeto é a seguinte:

> Com dados de eventos e condições de pacientes a partir dos seus registros disponíveis, é possível predizer o prognóstico de evolução para óbito de pacientes dentro de 7 dias e 15 dias?

## 2.3. Ferramentas

Para o presente trabalho, utilizou-se as seguintes ferramentas:

* Tecnologia _Python_, para desenvolver as provas de conceito;
* Bibliotecas _Panda, Glob, OS, Matplotlib.pyplot, Seaborn e Datetime_ como suporte para as funções necessárias;
* _Notebook Jupyter_, para escrita dos códigos em ambiente de execução;
* _Orange Data Mining_, para treinar e testar o sistema, além de gerar as saídas;
* _Scripts Shell_, para execução de fluxo de dados (_data pipeline_);
* Base de dados _Synthea_, para geração do modelo de prognóstico e também para testes.

## 2.4. Organização do repositório

O trabalho desenvolvido é divulgado de forma pública em [*repositório do GitHub*](https://github.com/brolesi/ds4h) e sua organização foi feita da seguinte forma:

~~~
├── README.md                         <- relatório do projeto (você está aqui!)
│
├── data
│   ├── interim                        <- dados intermediários usados para gerar os dados de saída
|     ├── results7_1_1.csv             <- resultado treinado no cenário 1 e testado no cenário 1 para 7 dias
|     ├── results7_2_2.csv             <- resultado treinado no cenário 2 e testado no cenário 2 para 7 dias
|     ├── results15_1_1.csv            <- resultado treinado no cenário 1 e testado no cenário 1 para 15 dias
|     └── results15_2_2.csv            <- resultado treinado no cenário 2 e testado no cenário 2 para 15 dias
│   ├── processed                      <- dados finais usados para a modelagem
|     ├── (...)15days_scenario1.csv    <- resultado do notebook "Dataset Generator" para 15 dias no cenário 1
|     ├── (...)15days_scenario2.csv    <- resultado do notebook "Dataset Generator" para 15 dias no cenário 2
|     ├── (...)7days_scenario1.csv     <- resultado do notebook "Dataset Generator" para 7 dias no cenário 1
|     └── (...)7days_scenario2.csv     <- resultado do notebook "Dataset Generator" para 7 dias no cenário 2
│   ├── output                         <- dados de saída que podem ser usados pelos médicos
|     ├── output_1_1.csv               <- resultado do notebook "output_generator" para treino no cenário 1 e teste no cenário 1
|     └── output_2_2.csv               <- resultado do notebook "output_generator" para treino no cenário 2 e teste no cenário 2
│   └── raw                            <- dados originais sem modificações copiados da base do Synthea.
│
├── notebooks                          <- Jupyter notebooks gerados  
│   ├── Dataset Generator.ipynb        <- Notebook que gera dos arquivos que serão usados de entrada no Orange
|   └── output_generator.ipynb         <- Notebook que gera os arquivos de saída com o prognóstico, com base na saída do Orange
├── src                                <- workflows desenvolvidos no Orange
|   ├── cenarios-07-dias.ows           <- Workflow para prognóstico de óbito em 7 dias
|   ├── cenarios-15-dias.ows           <- Workflow para prognóstico de óbito em 15 dias
│   └── README.md                      <- instruções básicas de execução
│
└── assets                             <- mídias usadas no projeto
~~~

# 3. Metodologia

O presente trabalho trata-se de um estudo de caso que utiliza a metodologia CRISP-DM (CRoss-Industry Standard Process for Data Mining), criado pela SPSS Inc [7]. Este modelo é composto de 6 fases, e suas interações podem ser vistas na figura a seguir [7]: 

1. Entendimento do negócio/contexto
2. Entendimento dos dados
3. Preparação dos dados
4. Modelagem
5. Avaliação
6. Aplicação (*Deployment*)

```mermaid
flowchart  RL;
subgraph 99[Entendimento]
id1(Entendimento do negócio) --> id2(Entendimento dos dados);
id2(Entendimento dos dados) --> id1(Entendimento do negócio) ;
end
id2(Entendimento dos dados) --> id100[Preparação dos dados];
subgraph id100[Preparação dos dados]
id101(Pacientes) --> id104(Datamart);
id102(Eventos)   --> id104(Datamart);
id103(Condições) --> id104(Datamart);
end

id100[Preparação dos dados] --> id4(Modelagem)
id4(Modelagem) --> id100[Preparação dos dados];
id4(Modelagem) --> id5(Validação);
id5(Validação) --> id1(Entendimento do negócio);
id5(Validação) --> id6("Aplicação (deploy)");

```
Figura 1: Metodologia CRISP-DM [7].

A seguir será explicado o objetivo de cada fase, com sua respectiva aplicação para solucionar o problema proposto neste projeto.

## 3.1 Entendimento do negócio e Entendimento dos dados

Para realizar o prognóstico, há o desafio de identificar quais serão seus dados de entrada e seus dados de saída. Além disso, também há o desafio de escolher como estes dados de entrada serão processados para gerar os dados de saída.

Neste trabalho, optou-se pela tecnologia _Machine Learning_, que usa de fórmulas e métodos para ensinar a máquina de acordo com um alvo, assim, ela sabe como agir em determinadas situações [ref ML]. Neste trabalho, com base em dados já existentes, será ensinado a uma máquina quais os parâmetros que influenciaram mais ou menos na evolução de uma pessoa à òbito, para que ela aprenda e diga qual a probabilidade de que uma determinada pessoa viva, com determinadas características, venha a óbito em 7 dias ou em 15 dias.

Foram usados dados dos cenários sintéticos do [Synthea](https://synthea.mitre.org/) presentes no repositório [Github](https://github.com/santanche/lab2learn/tree/master/data/synthea). As bases utilizadas para o presente projeto são as que seguem:

* [scenario01](/data/raw/scenario01/)
* [scenario02](/data/raw/scenario02/)

O Synthea tem a missão de produzir dados de pacientes sintéticos e realistas, mas não reais, de alta qualidade e registros de saúde associados, cobrindo todos os aspectos da saúde. Os dados resultantes estão livres de restrições de custo, privacidade e segurança. Ele pode ser usado sem restrições para uma variedade de usos secundários na academia, pesquisa, indústria e governo. Cada paciente sintético do Synthea é gerado de forma independente, à medida que progride desde o nascimento até a morte por meio de representações modulares de várias doenças e condições. Cada paciente percorre todos os módulos do sistema. Quando um paciente morre ou a simulação chega ao dia atual, esse registro do paciente pode ser exportado em vários formatos diferentes [8]. A figura 2 abaixo apresenta uma sinteze da organização dos dados do Synthea.

![alt text](assets/architecture.png)

Figura 2 - organização dos dados do Synthea [8].

Na base de dadosdo Synthea, os dados estão presentes em arquivos CSV (*comma separeted values*) e são os que seguem, conforme a Tabela 1 [9]:

| Aruivo                    | Descrição                                                                     |
| ------------------------- | ----------------------------------------------------------------------------- |
| `allergies.csv`           | Dados de alergia do paciente.                                                 |
| `careplans.csv`           | Dados do plano de atendimento ao paciente.                                    |
| `claims.csv`              | Dados de solicitações do paciente.                                            |
| `claims_transactions.csv` | Transações por item de linha por solicitação.                                 |
| `conditions.csv`          | Condições ou diagnósticos do paciente.                                        |
| `devices.csv`             | Dispositivos permanentes e semipermanentes utilizados pelo paciente.          |
| `encounters.csv`          | Dados de encontro do paciente.                                                |
| `imaging_studies.csv`     | Metadados de imagem do paciente.                                              |
| `immunizations.csv`       | Dados de imunização do paciente.                                              |
| `medications.csv`         | Dados de medicação do paciente.                                               |
| `observations.csv`        | Observações do paciente, incluindo sinais vitais e relatórios de laboratório. |
| `organizations.csv`       | Organizações fornecedoras, incluindo hospitais.                               |
| `patients.csv`            | Dados demográficos do paciente.                                               |
| `payer_transitions.csv`   | Dados de transição do pagador (ou seja, alterações no seguro de saúde).       |
| `payers.csv`              | Dados do plano de saúde.                                                      |
| `procedures.csv`          | Dados do procedimento do paciente, incluindo cirurgias.                       |
| `providers.csv`           | Médicos que prestam assistência ao paciente.                                  |
| `supplies.csv`            | Materiais utilizados na prestação de cuidados.                                |

Tabela 1: arquivos disponíveis no repositório e descrição de cada um [9].

Utilizou-se as tabelas `patients`, `encounters` e `conditions` para fazer uma análise exploratória dos dados. Posteriormente, utilizou-se as tabela `patients` e `encounters` para cálculo da probabilidade de óbito em até 7 dias e em até 15 dias.

Estas três tabelas são as mais relevantes de toda a base, e a integração dos dados pode ser melhor compreendida a partir da estrutura apresentada na figura 3. 

![alt text](assets/synthea.png)

Figura 3 - Integração das tabelas `patients` e `encounters`.

## 3.3 Preparação dos dados

Para gerar os dados que seriam aplicados no modelo, foi desenvolvido o _notebook_ ["Dataset generator"](notebooks/Dataset Generator.ipynb). Com a estruturação dos dados a partir do cruzamento entre eles, realizou-se a criação de colunas (características, ou, em inglês na área de ciência de dados, *feature*) sintéticas a partir de colunas originas de de dados categóricos para, a partir do aumento da dimensionalidade, trazer maior riquza para a criação do modelo considerando aspectos relevantes para a análise.

Ao final, as colunas relevantes para o desenvolvimento do _datamart_ foram as presentes na Tabela 2:

| **Tabela origem** | **Campo**                 | **Coluna origem**   | **Descrição**                                                                               |
| ----------------- | ------------------------- | ------------------- | ------------------------------------------------------------------------------------------- |
| ENCOUNTERS        | TOTAL_CLAIM_COST          | TOTAL_CLAIM_COST    | Custo total do encontro                                                                     |
| ENCOUNTERS        | ENCOUNTERCLASS_wellness   | ENCOUNTERCLASS      | Classe de encontro marcada como rotineira                                                   |
| ENCOUNTERS        | ENCOUNTERCLASS_urgentcare | ENCOUNTERCLASS      | Classe de encontro marcada como de urgência                                                 |
| ENCOUNTERS        | ENCOUNTERCLASS_snf        | ENCOUNTERCLASS      | Classe de encontro marcada como centro de enfermagem especializada                          |
| ENCOUNTERS        | ENCOUNTERCLASS_outpatient | ENCOUNTERCLASS      | Classe de encontro marcada como ambulatorial                                                |
| ENCOUNTERS        | ENCOUNTERCLASS_inpatient  | ENCOUNTERCLASS      | Classe de encontro marcada como internação                                                  |
| ENCOUNTERS        | ENCOUNTERCLASS_home       | ENCOUNTERCLASS      | Classe de encontro marcada como domiciliar                                                  |
| ENCOUNTERS        | ENCOUNTERCLASS_emergency  | ENCOUNTERCLASS      | Classe de encontro marcada como emergência                                                  |
| ENCOUNTERS        | ENCOUNTERCLASS_ambulatory | ENCOUNTERCLASS      | Classe de encontro marcada como ambulatorial                                                |
| ENCOUNTERS        | PAYER_COVERAGE            | PAYER_COVERAGE      | Valor do custo coberto pelo Pagador                                                         |
| PATIENTS          | BIRTHDATE                 | BIRTHDATE           | Data em que o paciente nasceu.                                                              |
| PATIENTS          | MARITAL                   | MARITAL             | Estado civil. M é casado, S é solteiro, divórciado (D) ou viuvo (W)                         |
| PATIENTS          | HEALTHCARE_COVERAGE       | HEALTHCARE_COVERAGE | Custo total ao longo da vida dos serviços de saúde que foram cobertos pela seguradora       |
| PATIENTS          | HEALTHCARE_EXPENSES       | HEALTHCARE_EXPENSES | Custo total ao longo da vida dos cuidados de saúde que o paciente pagou                     |
| PATIENTS          | LON                       | LON                 | Longitude do endereço do paciente                                                           |
| PATIENTS          | LAT                       | LAT                 | Latitude do endereço do paciente                                                            |
| PATIENTS          | ZIP                       | ZIP                 | Código postal do paciente                                                                   |
| PATIENTS          | GENDER                    | GENDER              | Gênero. M é masculino, F é feminino.                                                        |
| PATIENTS          | ETHNICITY                 | ETHNICITY           | Descrição da etnia primária do paciente                                                     |
| PATIENTS          | RACE                      | RACE                | Descrição da raça primária do paciente                                                      |
| ENCOUNTERS        | BASE_ENCOUNTER_COST       | BASE_ENCOUNTER_COST | Custo do encontro, sem incluir medicamentos, imunizações, procedimentos ou outros serviços. |

Tabela 2: Campos utilizados para composição do *datamart* a ser considerado para a geração do modelo de aprendizado de máquina. A **coluna origem** indica que o campo foi criado artificialmente de uma *feature* categórica da tabela origem.

Ainda neste arquivo gerado, foi adicionado um parâmetro chamado de _death_threshould_ que possui valor **_True_** quando o paciente foi a óbito em 7 dias ou em 15 dias e **_False_** em caso contrário. Esse parâmetro será o alvo posteriormente no modelo. 

Os 04 arquivos preparados, para 7 dias e 15 dias em ambos os cenários, podem ser vistos [no repositório](https://github.com/brolesi/ds4h/tree/main/data/processed).

## 3.4 Modelagem, Avaliação e Aplicação

### 3.4.1 Análise descritiva

Como já citado, primeiramente foram realizadas algumas análises dos dados das tabelas `patients`, `encounters` e `conditions` para entender quais condições mais levavam os pacientes á óbitos, e tomar as decisões para escolha de cenário de aplicação do prognóstico.

No cenário 1, foram encontrados 1174 pacientes diferentes, sendo que 174 deles já haviam ido à òbito, enquanto no cenário 2 haviam 1121 pacientes, com 121 óbitos. Em ambos os cenários haviam 1000 pacientes ainda em vida. 

Foi análisado a idade com que os pacientes foram à òbito, e a estratificação por gênero é apresentada na figura 4, na qual é possível analisar que os homens morreram em idades mais avançadas que as mulheres, em ambos os cenários. 

![alt text](assets/boxplot.png)

Figura 4 - Idade de falecimento por gênero em cada cenário.

Foi analisada ainda as condições que mais levaram a óbito considerando o prazo de até 7 dias. É possível perceber na figura 5 que a maior causa de morte no cenário 1 é a Leucemia mielóide aguda (*Acute myeloid leukemia* - 91861009) e no cenário 2 é a Hiperlipidemia (*Hyperlipidemia* - 55822004). 

![alt text](assets/contagem_morte.png)

Figura 5 - maiores causas de morte por cenário

Entretanto, percebeu-se que nem todas as condições que levaram a òbito no cenário 1 estavam presentes no cenário 2. Isso poderia atrapalhar no modelo que seria gerado posteriormente. Por essa razão, foi analisado quais eram as causas de morte presente em ambos os cenários, conforme apresentado na Figura 6.

![alt text](assets/contagem_morte2.png)

Figura 6 - Maiores causas de morte em comum nos dois cenários.

É possível perceber que a condição que mais levou à óbito, somando os dois cenários, é a Insuficiência cardíaca congestiva crônica (*Chronic congestive heart failure* - 88805009).

### 3.4.2 Modelo

Para compreensão do modelo de forma mais simples, ele será dividido em 3 fases, sendo:

* Fase 1 - Preparação dos dados de entrada
* Fase 2 - Treino e teste do sistema
* Fase 3 - Extração dos resultados para geração dos dados de saída

#### 3.4.2.1 Fase 1 - Preparação dos dados de entrada

Para o modelo, primeiro foi necessário eliminar todos os dados que poderiam causar bias, restando os parâmetros já apresentados na seção 3.3. Em seguida, os pacientes de cada cenário foram divididos em dois grupos, sendo um daqueles que já haviam ido à òbito e o outro daqueles que ainda estavam em vida. A proposta inicial seria de utilizar os dados do grupo dos pacientes que já foram a obito para treinar o sistema e os dados do grupo dos pacientes que estão em vida para prognóstico. Entretanto, o número de pacientes que já foram a obito com a condição pré-definida não possuía o tamanho suficiente para treinar o sistema. Por essa razão, foram usados os dados dos pacientes em vida como contraponto no treinamento.

Utilizou-se a técnica de *Support Vector Machines* afim de identificar a segregação a partir de hiperplanos dos dados em dois: prognóstico de evolução à óbito em até $7$ dias (ou em até $7$ dias) e prognóstico de evolução à óbito igual ou superior a $7$ dias (ou superior a $15$ dias). Dado que a quantidade de registros era pequena, o resultado não foi como o esperado, por isso, utilizou-se da técnica de *data augmentation* (ou, aumento de dados, a partir da criação de dados sintéticos, baseados nos dados originais), para que as amostras ficassem balanceadas.

#### 3.4.2.2 Fase 2 - Treino e teste do sistema

É o núcleo do modelo em si foi feito em dois _Workflows_ do _Orange Data Mining_, sendo um para óbito em até 7 dias e outro para óbito em até 15 dias. Ambos seguem o mesmo padrão e conceitos.

Para cada _workflow_ foram construidas 4 estruturas diferentes, para comparação dos resultados:

* Treino com o cenário 1 e teste com o cenário 1:
 
 ![alt text](assets/7d_1_1.png)

Figura x- Configuração do treino com o cenário 1 e teste com o cenário 1
 
* Treino com o cenário 2 e teste com o cenário 2:

![alt text](assets/7d_2_2.png)

Figura x- Configuração do treino com o cenário 2 e teste com o cenário 2

* Treino com o cenário 1 e teste com o cenário 2:

![alt text](assets/7d_1_2.png)

Figura x- Configuração do treino com o cenário 1 e teste com o cenário 2

* Treino com o cenário 2 e teste com o cenário 1:

![alt text](assets/7d_2_1.png)

Figura x- Configuração do treino com o cenário 2 e teste com o cenário 1


Já para o _pipeline_ de dados foram usados 3 modelos, a saber:

**A. Regressão logística**

O modelo de regressão logística tem como objetivo estudar a probabilidade de ocorrência de eventos, aqui chamado de $Y$, apresentado na forma qualitativa dicotômica (aqui usados no caos os valores $0$ para um não-evento e $1$ para um evento). Para isso, foi definido um vetor de variáveis explicativas, com seus respectivos parâmetros estimados, na forma [REF Regressão]:

$$Z_i = \alpha + \beta_1X_{1i} +  \beta_2X_{2i} + \cdots + \beta_kX_{ki}  $$

em que, $Z$ é o *logito*, $\alpha$ representa a constante (bias), $\beta_j$ são os parâmetros estimados de cada variável explicativa, $X_j$ são as variáveis explicativas (métricas ou *dummies*) e $i$ é a i-ésima observação da amostra.

É importante destacar que Z não é variável dependente, já que é definida por $Y$. O objetivo é definir a expressão da **probabilidade de óbito em 7 ou 15 dias** $p_i$ de ocorrência, em função do logito $Z_i$, ou seja, em função dos parâmetros estimados para cada variável explicativa. Para tanto, devemos definir o conceito de **chance** de ocorrência de um evento, também conhecida por *odds*, da seguinte forma: 

$$chance(odds)_{Y_i=1} = \frac{p_i}{1-p_i}$$

A regressão logística binária define o logito $Z$ como o logaritmo natural da chance, de modo que:

$$ ln(chance_{Y_i=1}) = Z_i$$

De onde tem-se:

$$ ln\left(\frac{p_i}{1-p_i}\right) = Z_i $$

Na medida em que é necessário identificar uma expressão para a probabilidade de **ocorrência do evento**  em função do logito, pode-se matematicamente isolar $p_i$ da seguinte maneira:

$$ \frac{p_i}{1-p_i} = e^{Z_i} $$

logo

$$ p_i =(1-p_i)e^{Z_i}$$

então

$$ p_i(1+e^{Z_i})=e^{Z_i} $$

Disso então tem-se finalmente que:

**Chance de ocorrência do evento:**

$$ p_i = \frac{e^{Z_i}}{1+e^{Z_i}} = \frac{1}{1+e^{-Z_i}}$$

**Chance de ocorrência de um não evento:**

$$ 1 - p_i = 1 - \frac{e^{Z_i}}{1+e^{Z_i}} = \frac{1}{1+e^{Z_i}}$$

No workflow, a regressão logística foi configurada conforme apresentado na figura X.

![alt text](assets/orange-lr-params.png)

Figura x- Configuração da regressão logística

**B. Árvore de decisão**

O segundo método usado foi a árvore de decisão, onde na primeira divisão ou raiz, todas as features são consideradas e os dados de treinamento são divididos em grupos com base nessa divisão. Serão feitas $n$ divisões, tais quais forem as variedades da _feature_ candidata. Para calcular quanta precisão cada divisão custará, usando uma função custo. A divisão que custa menos é escolhida. Este algoritmo é recursivo por natureza, pois os grupos formados podem ser subdivididos usando a mesma estratégia. Devido a este procedimento, este algoritmo também é conhecido como algoritmo guloso (*greedy*). Isso torna o nó raiz o melhor preditor/classificador.

Ao olhar para o custo de uma divisão, é necessário olhar as funções de custo usadas para classificação e regressão. Em ambos os casos as funções de custo tentam encontrar ramos mais homogêneos, ou ramos com grupos com respostas semelhantes. Isso dá mais certeza de que uma entrada de dados de teste seguirá um determinado caminho.

Há ainda a chamada pontuação Gini, conforme equação a seguir:

$$ G = \sum(pk * (1 — pk)) $$

Uma pontuação Gini dá uma ideia de quão boa é uma divisão pelo quão mistas são as classes de resposta nos grupos criados pela divisão. Aqui, $pk$ é a proporção de entradas da mesma classe presentes em um determinado grupo. Uma pureza de classe perfeita ocorre quando um grupo contém todas as entradas da mesma classe, caso em que $pk$ é $1$ ou $0$ e $G = 0$, onde um nó com uma divisão de $50-50$ classes em um grupo tem a pior pureza, então para uma classificação binária terá $pk = 0,5$ e $G = 0,5$.

Como um problema geralmente possui um grande conjunto de _features_, ele resulta em grande número de divisões, o que por sua vez resulta em uma árvore grande. Essas árvores são complexas e podem levar a _overfitting_, então, é necessário saber quando parar. Uma maneira de fazer isso é definir um número mínimo de entradas de treinamento para usar em cada folha. Outra maneira é definir a profundidade máxima do seu modelo. A profundidade máxima refere-se ao comprimento do caminho mais longo de uma raiz a uma folha. 

O desempenho de uma árvore pode ser aumentado ainda mais pela poda. Trata-se de remover as ramificações que fazem uso de recursos de baixa relevância. Dessa forma, se reduz a complexidade da árvore e, assim, aumenta-se seu poder preditivo, reduzindo o _overfitting_.

Entre as vantagens da árvore de decisão estão:

* É simples de entender, interpretar, visualizar.
* As árvores de decisão executam implicitamente a triagem de variáveis ou a seleção de recursos.
* Pode lidar com dados numéricos e categóricos. Também pode lidar com problemas de várias saídas.
* As árvores de decisão exigem relativamente pouco esforço dos usuários para a preparação dos dados.
* As relações não lineares entre os parâmetros não afetam o desempenho da árvore.

Mas também há desvantagens:

* Os aprendizes de árvores de decisão podem criar árvores supercomplexas que não generalizam bem os dados. Isso é chamado de sobreajuste.
* As árvores de decisão podem ser instáveis porque pequenas variações nos dados podem resultar na geração de uma árvore completamente diferente. Isso é chamado de variação , que precisa ser reduzido por métodos como ensacamento e aumento.
* Algoritmos gananciosos não podem garantir o retorno da árvore de decisão globalmente ótima. Isso pode ser mitigado treinando várias árvores, onde os recursos e as amostras são amostrados aleatoriamente com substituição.
* Os aprendizes de árvores de decisão criam árvores tendenciosas se algumas classes dominarem . Portanto, é recomendado balancear o conjunto de dados antes de ajustar com a árvore de decisão.

No workflow, a árvore de decisão foi configurada conforme apresentado na figura X.

![alt text](assets/orange-tree-params.png)

Figura x- Configuração da árvore de decisão

**C. _Gradient boosting_ (mais especificamente XGBoosting)**

#### 3.4.2.3 XGBoost [TODO: fonte https://xgboost.readthedocs.io/en/stable/tutorials/model.html]

XGBoost significa "Extreme Gradient Boosting", onde o termo "Gradient Boosting" tem origem no artigo **Greedy Function Approximation: A Gradient Boosting Machine**, de Friedman.

As árvores impulsionadas (com *boosting*) por gradiente já existem há algum tempo. 

O XGBoost é usado para problemas de aprendizado supervisionado, onde usamos os dados de treinamento (com várias *features*) para prever uma variável resposta.

O modelo de conjunto de árvores consiste em um conjunto de árvores de classificação e regressão (*classification and regression trees* ou CART).
Um CART é um pouco diferente das árvores de decisão, nas quais a folha contém apenas valores de decisão. No CART, uma pontuação real é associada a cada uma das folhas, o que nos dá interpretações mais ricas que vão além da classificação. Isso também permite uma abordagem unificada e baseada em princípios para otimização, como veremos em uma parte posterior deste tutorial.

Normalmente, uma única árvore não é forte o suficiente para ser usada na prática. O que é realmente usado é o modelo ensemble, que soma a previsão de várias árvores juntas.

As pontuações de previsão de cada árvore individual são somadas para obter a pontuação final. Se você observar o exemplo, um fato importante é que as duas árvores tentam se complementar . Matematicamente, podemos escrever nosso modelo na forma

$$ \hat{y}_i = \sum_{k=1}^K f_k(x_i), f_k \in \mathcal{F} $$

Onde $K$ é o número de árvores,$f_k$ é uma função no espaço funcional $\mathcal{F}$, e $\mathcal{F}$ é o conjunto de todos os CARTs possíveis. 
A função objetivo a ser otimizada é dada por

 $$ \text{obj}(\theta) = \sum_i^nl(y_i, \hat{y}_i) + \sum_{k=1}^K \omega(f_k) $$

Onde $\omega(f_k)$ é a complexidade da árvore $f_k$.

Florestas aleatórias e árvores impulsionadas são realmente os mesmos modelos; a diferença surge de como eles são treinados.

No workflow, o XGBoosting foi configurado conforme apresentado na figura X.

![alt text](assets/orange-xgboost-params.png)

Figura x- Configuração do XGBoosting

#### 3.4.2.3 Fase 3 - Extração dos resultados para geração dos dados de saída

Ao final da execução do workflow é gerado um arquivo de saída com as probabilidades de óbito em 7 dias e em 15 dias para cada modelo gerado. Este arquivo não é entendivel por uma pessoa leiga, por essa razão precisa ser tratado. O notebook ["output_generator"](https://github.com/brolesi/ds4h/blob/main/notebooks/output_generator.ipynb) foi desenvolvido para esta finalidade. Poderiam ser mantidos todos os modelos e também todas as 4 formas de treino/teste apresentadas, mas elas gerariam muitas saídas diferentes, então foram geradas apenas duas para exemplo e comparação. Foi feito um arquivo de saída para treino e teste no cenário 1 e um arquivo de saída para treino e teste no cenário 2. Este arquivo de saída é do tipo .csv possui apenas 3 colunas, sendo elas, o Id do paciente, sua probabilidade de ir à óbito em 7 dias e sua probabilidade de ir à obito em 15 dias.

# 4. Resultados e Discussão

Para compreender os resultados dos modelos, foram gerados a curva ROC, a matriz de confusão e a tabela de Scores, com AUC, CA, F1, Precision e Recall. Como são 04 tipos de treino e teste, para 2 cenários diferentes (07 dias e 15 dias), o volume de figuras para trazer ao relatório seria muito alto, por isso, aqui serão apresentados apenas um de cada, e os resultados completos podem ser vistos no relatórios gerados pelo Orange: [Relatório 7 dias](https://github.com/brolesi/ds4h/blob/main/assets/report-7-days-roc.pdf) e [Relatório 15 dias](https://github.com/brolesi/ds4h/blob/main/assets/report-15-days-roc.pdf)

* Curva ROC:


![alt text](assets/roc7_1_1.png)

Figura x- Curva ROC para 7 dias com treino no cenário 1 e teste no cenário 1


* Matriz de confusão:


![alt text](assets/mc7_1_1.png)

Figura x- Matriz de confusão para 7 dias com treino no cenário 1 e teste no cenário 1


* Scores:


![alt text](assets/score7_1_1.png)

Figura x- Tabela de Score para 7 dias com treino no cenário 1 e teste no cenário 1

Para analisar o resultado final, é apresentado na Figura x uma parte da base de saída gerada para treino no cenário 1 e teste no cenário 1, e na Figura x  uma parte da base de saída gerada para treino no cenário 2 e teste no cenário 2.


![alt text](assets/print_output_1_1.png)

Figura x- Saída com treino no cenário 1 e teste no cenário 1.

![alt text](assets/print_output_2_2.png)

Figura x- Saída com treino no cenário 2 e teste no cenário 2.

# 5. Conclusão
> Destacar as principais conclusões obtidas no desenvolvimento do projeto.
>
> Destacar os principais desafios enfrentados.
>
> Principais lições aprendidas.
>
> Trabalhos Futuros:
> * o que poderia ser melhorado se houvesse mais tempo?

A partir dos resultados obtidos, existe a possibilidade de evolução do ponto de vista de utilizar / captar mais dados que possam ser relevantes para a análise e construção de um modelo mais robusto e que possa ter a capacidade de previsão mais acurada, para apoiar a definição do prognóstico médico [TODO: incrementar].

Outra possibilidade é a geração de mais dados sintéticos para, a partir de uma massa de dados maior, termos um modelo com maior robustez.

# Referências Bibliográficas

[1] Patino, C.M.,  Ferreira J.C. "Prognostic studies for health care decision making". CONTINUING EDUCATION: SCIENTIFIC METHODOLOGY, J Bras Pneumol., 43 (04), Aug 2017. https://doi.org/10.1590/S1806-37562017000000241

[2] Steyerberg E.W., Moons K.G.M., van der Windt D.A., Hayden J.A., Perel P., Schroter S., et al. (2013) Prognosis Research Strategy (PROGRESS) 3: Prognostic Model Research. PLoS Med 10(2): e1001381. https://doi.org/10.1371/journal.pmed.1001381

[3] https://www.rccc.eu/ppc/indicadores/saps3.html

[4] https://www.mdapp.co/palliative-prognostic-index-ppi-calculator-402/

[5] https://www.mdapp.co/palliative-prognostic-score-pap-calculator-401/

[6] https://www.mdcalc.com/maggic-risk-calculator-heart-failure

[7] P. Chapman et al., "CRISP-DM 1.0 - Step-by-step data mining guide". SPSS Inc., 2000.

[8]  Synthea (2020). Getting Started [código fonte]. Disponível em: https://github.com/synthetichealth/synthea/wiki/Getting-Started Acessado em Maio de 2022.

[9] Synthea (2020). CSV File Data Dictionary [código fonte]. Disponível em: https://github.com/synthetichealth/synthea/wiki/CSV-File-Data-Dictionary Acessado em Maio de 2022.

[10] ref ML

[11] livro

[12] https://towardsdatascience.com/decision-trees-in-machine-learning-641b9c4e8052

[13] ref gradiente

