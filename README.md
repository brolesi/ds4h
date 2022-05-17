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

A área de pesquisa de prognósticos busca entender e melhorar os resultados de prognósticos em pessoas com uma determinada doença ou condição de saúde. O objetivo geral de estudos prognósticos em contextos clínicos é ajudar clínicos, pacientes e familiares a tomar decisões esclarecidas a respeito de cuidados de saúde com base em informações disponíveis sobre cada paciente no presente para prever desfechos no futuro [1]. Além disso, ajuda os pacientes e os familiares a tomar decisões adequadas a respeito do fim da vida daqueles cujo risco de morte é muito alto e a identificar intervenções personalizadas para evitar futuras hospitalizações [2].

Os modelos prognósticos usam vários fatores em combinação para prever o risco de resultados clínicos futuros em pacientes. Um bom modelo deve (i) fornecer previsões precisas que informam os pacientes e seus cuidadores, (ii) apoiar a pesquisa clínica e (iii) permitir decisões para melhorar os resultados dos tratamentos aos pacientes [1]. Um modelo prognóstico tem três fases principais: desenvolvimento do modelo (incluindo validação interna), validação externa e investigações de impacto na prática clínica. Embora muitos modelos prognósticos sejam propostos, poucos são atualmente usados na prática clínica [1].

A proposta deste projeto é montar um ou mais modelos de prognóstico que realizem a predição de mortalidade de pacientes sintéticos gerados em pelo menos dois cenários de dados fictícios. Será necessário estabelecer os parâmetros de predição, definir quais os dados sobre o paciente que serão usados para a predição, construir modelos de aprendizagem de máquina que realizem predições e apresentar o resultado do modelo de predição aplicado.

Para realizar o prognóstico de forma mais precisa, é importante escolher um cenário específico. Dentre os dados disponibilizados, a equipe optou por trabalhar com os indivíduos que sofrem de Insuficiência Cardíaca Congestiva Crônica (ICC) [3]. Esta condição ocorre porque o coração não tem força para bombear a quantidade necessária de sangue para o corpo, e com a diminuição da circulação muitas funções corporais ficam prejudicadas, pois falta oxigênio. As causas mais comuns da ICC são doenças arteriais coronarianas, infarto, hipertensão arterial, doenças das válvulas cardíacas, diabetes, outras doenças do coração ou doenças congênitas [4].

Diante deste cenário, a pergunta de pesquisa levantada para este projeto é a seguinte:

> Com dados de eventos e condições de pacientes a partir dos seus registros disponíveis, é possível predizer o prognóstico de evolução para óbito de pacientes com Insuficiência Cardíaca Congestiva Crônica dentro de 7 dias?

## 2.1. Ferramentas

Para o presente trabalho, utilizou-se as seguintes ferramentas:

* Tecnologia _Python_, para desenvolver as provas de conceito;
* Bibliotecas _Panda, Glob, OS, Matplotlib.pyplot, Seaborn e Datetime_ como suporte para as funções necessárias;
* _PyCaret_, para escolher o melhor modelo a ser utilizado;
* _Notebook Jupyter_, para escrita dos códigos em ambiente de execução;
* _Scripts Shell_, para execução de fluxo de dados (_data pipeline_);
* Base de dados _Synthea_, para geração do modelo de prognóstico e também para testes.

# 3. Metodologia

O presente trabalho trata-se de um estudo de caso que utiliza a metodologia CRISP-DM (CRoss-Industry Standard Process for Data Mining), criado pela SPSS Inc [5]. Este modelo é composto de 6 fases, e suas interações podem ser vistas na figura a seguir [5]: 

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
Figura 1: Metodologia CRISP-DM.

A seguir será explicado o objetivo de cada fase, com sua respectiva aplicação para solucionar o problema proposto neste projeto.

## 3.1 Entendimento do problema (entendimento de negócio)

Insuficiência cardíaca é uma das principais causas de mortalidade e morbidade no mundo, e está associada ao alto uso de recursos e custos com saúde. No Brasil, a prevalência de insuficiência cardíaca é de aproximadamente 2 milhões de pacientes, e sua incidência é de aproximadamente 240.000 novos casos por ano, sendo que as cidades localizadas no sul, sudeste, nordeste e centro-oeste mostraram maior risco relativo para mortalidade causada por insuficiência cardíaca, e a maioria das cidades do norte foi classificada como um fator protetivo contra esta causa de morte [6]. 
Observando-se os datasets usados, nota-se que as causas da morte que mais contem dados é a ICC. Diante dessa realidade, é relevante que seja realizado um estudo prognóstico de pacientes com esta condição.

## 3.2 Entendimento dos dados

Foram usados dados dos cenários sintéticos do [Synthea](https://synthea.mitre.org/) presentes no repositório [Github](https://github.com/santanche/lab2learn/tree/master/data/synthea). As bases utilizadas para o presente projeto são as que seguem:

* [scenario01](/data/raw/scenario01/)
* [scenario02](/data/raw/scenario02/)

O Synthea tem a missão de produzir dados de pacientes sintéticos e realistas, mas não reais, de alta qualidade e registros de saúde associados, cobrindo todos os aspectos da saúde. Os dados resultantes estão livres de restrições de custo, privacidade e segurança. Ele pode ser usado sem restrições para uma variedade de usos secundários na academia, pesquisa, indústria e governo. Cada paciente sintético do Synthea é gerado de forma independente, à medida que progride desde o nascimento até a morte por meio de representações modulares de várias doenças e condições. Cada paciente percorre todos os módulos do sistema. Quando um paciente morre ou a simulação chega ao dia atual, esse registro do paciente pode ser exportado em vários formatos diferentes [7]. A figura 2 abaixo apresenta uma sinteze da organização dos dados do Synthea.

![alt text](assets/architecture.png)

Figura 2 - organização dos dados do Synthea [7].

Na base de dadosdo Synthea, os dados estão presentes em arquivos CSV (*comma separeted values*) e são os seguintes:

* `allergies`
* `careplans`
* `claims`
* `conditions`
* `devices`
* `encounters`
* `imaging_studies`
* `immunizations`
* `medications`
* `observations`
* `organizations`
* `patients`
* `payers`
* `payer_transitions`
* `procedures`
* `providers`
* `supplies`

Este projeto foi dividido em duas etapas, sendo que na primeira utilizou-se as tabelas `patients`, `encounters` e `conditions` para entender quais condições mais levavam os pacientes á óbitos, e tomar as decisões para escolha de cenário de aplicação do prognóstico. Posteriormente, na segunda etapa, utilizou-se apenas a tabela `patients` para cálculo da probabilidade de óbito em até 7 dias.

Estas três tabelas são as mais relevantes de toda a base, e a integração dos dados pode ser melhor compreendida a partir da estrutura apresentada na figura 3. 

![alt text](assets/synthea.png)

Figura 3 - Integração das tabelas `patients`, `encounters` e `conditions`.

## 3.3 Preparação dos dados

Após o entendimento dos dados, na primeira etapa, foram retirados dados das tabelas `patients`, `encounters` e `conditions`. A combinação dos dados de interesse presentes em cada uma das tabelas fornecidas foi feita a partir do vínculo entre elas conforme a Tabela 1, gerando um _datamart_ final para ser utilizado no modelo proposto.

|                  | **`patients`** | **`encounters`** | **`conditions`** |
| ---------------- | -------------- | -----------------| ---------------- |
| **`patients`**   |                |   `patient_id`   |                  |
| **`encounters`** | `patient_id`   |                  |  `encounter_id`  |
| **`conditions`** |                |  `encounter_id`  |                  |

 Tabela 1: Apresentação do vínculo entre tabelas para criação do datamart a ser utilizado pelo modelo.

Já na segunda etapa, com a estruturação dos dados a partir do cruzamento entre eles, realizou-se a criação de colunas (características, ou, em inglês na área de ciência de dados, *feature*) sintéticas a partir de colunas originas de de dados categóricos para, a partir do aumento da dimensionalidade, trazer maior riquza para a criação do modelo considerando aspectos relevantes para a análise.

Ao final, as colunas relevantes para o desenvolvimento do _datamart_ foram as presentes na Tabela 2:

| **Tabela origem** | **Campo**                 | **Coluna origem**   | **Descrição**                                                      |
| ----------------- | ------------------------- | ------------------- | ------------------------------------------------------------------ |
| ENCOUNTERS        | TOTAL_CLAIM_COST          | TOTAL_CLAIM_COST    | Custo total do encontro
| ENCOUNTERS        | ENCOUNTERCLASS_wellness   | ENCOUNTERCLASS      | Classe de encontro marcada como rotineira                          |
| ENCOUNTERS        | ENCOUNTERCLASS_urgentcare | ENCOUNTERCLASS      | Classe de encontro marcada como de urgência                        |
| ENCOUNTERS        | ENCOUNTERCLASS_snf        | ENCOUNTERCLASS      | Classe de encontro marcada como centro de enfermagem especializada |
| ENCOUNTERS        | ENCOUNTERCLASS_outpatient | ENCOUNTERCLASS      | Classe de encontro marcada como ambulatorial                       |
| ENCOUNTERS        | ENCOUNTERCLASS_inpatient  | ENCOUNTERCLASS      | Classe de encontro marcada como internação                         |
| ENCOUNTERS        | ENCOUNTERCLASS_home       | ENCOUNTERCLASS      | Classe de encontro marcada como domiciliar                         |
| ENCOUNTERS        | ENCOUNTERCLASS_emergency  | ENCOUNTERCLASS      | Classe de encontro marcada como emergência                         |
| ENCOUNTERS        | ENCOUNTERCLASS_ambulatory | ENCOUNTERCLASS      | Classe de encontro marcada como ambulatorial                       |
| ENCOUNTERS        | PAYER_COVERAGE            | PAYER_COVERAGE      | Valor do custo coberto pelo Pagador                                |
| PATIENTS          | BIRTHDATE                 | BIRTHDATE           | Data em que o paciente nasceu.                                     |
| PATIENTS          | MARITAL                   | MARITAL             | Estado civil. M é casado, S é solteiro, divórciado (D) ou viuvo (W)|
| PATIENTS          | HEALTHCARE_COVERAGE       | HEALTHCARE_COVERAGE | Custo total ao longo da vida dos serviços de saúde que foram cobertos pela seguradora|
| PATIENTS          | HEALTHCARE_EXPENSES       | HEALTHCARE_EXPENSES | Custo total ao longo da vida dos cuidados de saúde que o paciente pagou|
| PATIENTS          | LON                       | LON                 | Longitude do endereço do paciente                                  |
| PATIENTS          | LAT                       | LAT                 | Latitude do endereço do paciente                                   |
| PATIENTS          | ZIP                       | ZIP                 | Código postal do paciente                                          |
| PATIENTS          | GENDER                    | GENDER              | Gênero. M é masculino, F é feminino.                               |
| PATIENTS          | ETHNICITY                 | ETHNICITY           | Descrição da etnia primária do paciente                            |
| PATIENTS          | RACE                      | RACE                | Descrição da raça primária do paciente                             |
| ENCOUNTERS        | BASE_ENCOUNTER_COST       | BASE_ENCOUNTER_COST | Custo do encontro, sem incluir medicamentos, imunizações, procedimentos ou outros serviços.|

Tabela 2: Campos utilizados para composição do *datamart* a ser considerado para a geração do modelo de aprendizado de máquina. A **coluna origem** indica que o campo foi criado artificialmente de uma *feature* categórica da tabela origem.

Após a criação do *datamart*, realizou-se a extração de informações relevantes para a análise dos pacientes de interesse considerando apena a condição de Insuficiência Cardíaca Congestiva Crônica para que a partir do modelo, a entrada de dados fosse feita apenas considerando a condição de interesse.

## 3.4 Modelagem

### 3.4.1 Análise descritiva

[TODO: penso em colocar boxplot, outros pontos descritivos de análises iniciais]

### 4.3.2 Modelo

[TODO: colocar a criação de samples / treino/teste, SVM e decision tree]

Sobre o modelo, utilizou-se a técnica de *Support Vector Machines* afim de identificar a segregação a partir de hiperplanos dos dados em dois: prognóstico de evolução à óbito em até $7$ dias e prognóstico de evolução à óbito igual ou superior a $7$ dias. Dado que a quantidade de registros era pequena, utilizou-se da técnica de *data augmentation* (ou, aumento de dados, a partir da criação de dados sintéticos, baseados nos dados originais), para que as amostras (óbito em até 7 dias e óbito a partir de 7 dias) ficassem balanceadas.

## 3.5 Avaliação

Os resultados obtidos a partir da análise descritiva, criação e refinamento do modelo são os que seguem

## 3.6 Aplicação

Uma com o modelo desenhado e validado, realizou-se então o deploy do mesmo, tornando-o produtivo para ser executado no dispositivo final, e a partir daí, o uso do mesmo a partir da entrada de dados conforme estrutura da Tabela 3.

| **Campo** | **Descrição**                |
| --------- | ---------------------------- |
| A         | Identifica o tipo de sintoma |
| D         | Identifica o tempo de vida   |

Tabela 3: Campos utilizados na entrada dos dados para processamento do modelo desenvolvido.

# 4. Resultados Obtidos

> Esta seção pode opcionalmente ser apresentada em conjunto com a metodologia, intercalando método e resultados.
>
> Descreva etapas para obtenção do modelo, incluindo tratamento de dados, se houve.
>
> Apresente o seu modelo de predição e resultados alcançados.
> Para cada modelo, apresente no mínimo:
> * quais os dados sobre o paciente que serão usados para a predição;
> * qual a abordagem/modelo adotado;
> * resultados do preditor (apresente da forma mais rica possível, usando tabelas e gráficos - métricas e curva ROC são bem vindos);
> * breve discussão sobre os resultados obtidos.
>
> Nesta seção, lembre-se das sugestões de análise:
> * analisar diferentes composições de treinamento e análise do modelo:
>   * um modelo treinado/validado no cenário 1 e testado no cenário 2 e vice-versa;
>   * um modelo treinado e validado com os dados dos dois cenários;
>   * nos modelos dos dois itens anteriores:
>     * houve diferença de resultados?
>     * como analisar e interpretar as diferenças?
> * testar diferentes composições de dados sobre o paciente para a predição (por exemplo, quantidade diversificadas de número de itens).

# 5. Evolução do Projeto

> Seção opcional se houver histórico de mudanças e evolução relevantes.
> Relate aqui a evolução do projeto: possíveis problemas enfrentados e possíveis mudanças de trajetória. Relatar o processo para se alcançar os resultados é tão importante quanto os resultados.


# 6. Discussão
> Fazer um breve debate sobre os resultados alcançados. Aqui pode ser feita a análise dos possíveis motivos que certos resultados foram alcançados. Por exemplo:
> * por que seu modelo alcançou (ou não) um bom resultado?
> * por que o modelo de um cenário não se desempenhou bem em outro?
>
> A discussão dos resultados também pode ser feita opcionalmente na seção de Resultados, na medida em que os resultados são apresentados. Aspectos importantes a serem discutidos: É possível tirar conclusões dos resultados? Quais? Há indicações de direções para estudo? São necessários trabalhos mais profundos?

# 7. Conclusão
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
> Lista de artigos, links e referências bibliográficas (se houver).
>
> Fiquem à vontade para escolher o padrão de referenciamento preferido pelo grupo.

[1] https://www.scielo.br/j/jbpneu/a/4v6FmcMGxnnSpCmJ3dZZrbG/?format=pdf&lang=pt
[2] https://journals.plos.org/plosmedicine/article?id=10.1371/journal.pmed.1001381
Steyerberg EW, Moons KG, van der Windt DA, Hayden JA, Perel P, Schroter S, et al. Prognosis Research Strategy (PROGRESS) 3: prognostic model research. PLoS Med. 2013;10(2):e1001381. https://doi.org/10.1371/journal.pmed.1001381
[3] https://www.findacode.com/snomed/88805009--chronic-congestive-heart-failure.html?hl=88805009
[4] https://wippesaude.com.br/2018/06/12/o-que-e-o-icc-insuficiencia-cardiaca-cronica-congestiva/
[5] crisp
[6] https://doi.org/10.36660/abc.20201325
[7] https://github.com/synthetichealth/synthea/wiki/Getting-Started
[8] https://github.com/synthetichealth/synthea/wiki/CSV-File-Data-Dictionary

