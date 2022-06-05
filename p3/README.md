# 1. Apresentação

O presente projeto foi originado no contexto das atividades da disciplina de pós-graduação [*Ciência e Visualização de Dados em Saúde*](https://ds4h.org), oferecida no primeiro semestre de 2022, na Unicamp.


| Nome                          | RA     | Especialização |
| ----------------------------- | ------ | -------------- |
| Bruna Osti                    | 231024 | Computação     |
| Fabio Fogliarini Brolesi      | 023718 | Computação     |
| Ingrid Alves de Paiva Barbosa | 182849 | Computação     |

Tabela 1 - Equipe autora do projeto.

# 2. Referência bibliográfica do artigo lido
Brattig Correia, R., de Araújo Kohler, L., Mattos, M.M. et al. City-wide electronic health records reveal gender and age biases in administration of known drug–drug interactions. npj Digit. Med. 2, 74 (2019). https://doi.org/10.1038/s41746-019-0141-x

# 3. Resumo
O artigo escolhido se chama "_City-wide electronic health records reveal gender and age biases in administration of known drug–drug interactions_" e aborda sobre as reações adversas a medicamentos (ADR, do inglês _Adverse drug reactions_) de interações medicamentosas (DDI, do inglês _drug–drug interactions_), que é um tema muito discutido na medicina ao redor do Mundo. 

A ADR é uma resposta prejudicial ou indesejável, não intencional, do organismo, que ocorre após utilizar um medicamento. Dentre as razões que levam a uma ADR, 30% delas são devidas às interações medicamentosas, e os pacientes com complicações decorrentes da DDI tendem a reentrar no sistema de saúde em um nível mais caro. Além disso, estima-se que 52% das reações em pacientes ambulatoriais eram evitáveis. Se essas reações forem evitadas, haverá uma grande economia no setor de saúde, principalmente no âmbito público, além de reduzir os riscos de problemas de saúde para os pacientes.

Diante desse cenário, o trabalho visou realizar um estudo longitudinal em larga escala do fenômeno DDI nos níveis de atenção primária, nas Unidades Básicas de Saúde (UBS), e secundária, nas Unidades de Pronto Atendimento (UPA 24h), em uma cidade inteira, usando janelas de tempo consideravelmente maiores e contando com padrões públicos de DDI e ADR. Para realiar este estudo, foi feito um estudo de caso referente à população de Blumenau, no estado de Santa Catarina, Sul do Brasil. Blumenau é uma cidade que no ano da realização do trabalho tinha uma população aproximada de 340mil habitantes e um IDH igual a 0,806, considerado um valor muito alto. Em Blumenau, o custo anual com problemas de saúde resultantes de DDI em pessoas acima de 65 anos é US$2 (per capita), podendo chegar á US$7 se aplicadas taxas e inflações. Estes valores mostram que este problema é mais grave do que se pensava anteriormente, e quanto mais velha é a população, maior é esse custo.  

Os autores do trabalho tiveram acesso à todos os prontuários médicos de 18 meses (Jan 2014 – Jun 2015), fornecidos pela prefeitura do município. Após as análises de dados e construção das redes complexas, foram constatados que 181 pares de medicamentos que podem interagir e causar reações foram dispensados, mas 4% dos pacientes receberam pares de medicamentos que provavelmente resultarão em ADR. Ao profundar na análise dos dados, também foi identificado que mulheres tem um risco 60% maior de DDI do que homens, e essa taxa pode aumentar para 90% quando considera-se as interações com graves reações, e curiosamente, foi identificado que isso esta relacionado a causas sociais ou biológicas não conhecidas ainda. As pessoas idosas, com idade entre 70 e 79 anos têm um risco de 34% de DDI. Por fim, eles também forneceram uma rede de medicamentos e fatores demográficos paran categorizar o DDI.

A equipe disponibilizou um [repositório no GitHub](https://github.com/rionbr/DDIBlumenau) que contém todos os códigos desenvolvidos, além dos arquivos com os dados de entrada para que pudessem ser reproduzidos. Além disso, há um [material complementar](https://static-content.springer.com/esm/art%3A10.1038%2Fs41746-019-0141-x/MediaObjects/41746_2019_141_MOESM1_ESM.pdf) disponibilizado que contém informações, gráficos e resultados extras para auxiliar na reprodução.

# 4. Breve descrição do experimento que foi replicado
O experimento deste trabalho iniciou com a análise e tratamento dos dados. Em seguida, foi desenvolvido o método em si, em que calcula-se o risco relacionado ao gênero e à idade, com a geração da rede de DDI, e com os classificadores de aprendizado de máquina. Uma breve explicação de cada etapa do método usado será apresentado a seguir. 

## 4.1. Tratamento dos dados
A equipe recebeu os prontuários da prefeitura de Blumenau de 18 meses, entre Janeiro de 2014 e Junho de 2015, e analisaram os relatórios de medicamentos e dosagens prescritos no período. Estes dados foram anonimizados na fonte e manteve-se apenas as dosagens com os medicamentos e algumas variáveis demográficas como gênero, idade, bairro, estado civil e escolaridade. O conscentimento para coleta dos dados não é de responsabilidade da equipe, já que não foram eles os responsáveis pela coleta.

Os nomes dos medicamentos foram traduzidos para o inglês, foram eliminados os casos ambíguos, e foram usados identificadores para os medicamentos (baseado no DrugBank ID, que é explicado melhor na seção X). Medicamentos com vários componentes, como por exemplo a Amoxicilina 500 mg e Clavulanato 125 mg, foram divididos por seus componentes. Além disso, foram dispensadas substância que não estão presentes no DrugBank, como por exemplo, o leite em pó e complexos vitamínicos. Após todos os tratamentos, 122 medicamentos únicos foram mantidos para análise.

Dentro do período considerado, foram registradas 1.573.678 administrações de medicamentos à 132.722 pacientes distintos, equivalente à 17% da população da cidade. Destes pacientes, 41,5% são homens e 58,5% são mulheres. Um total de 46% destes pacientes declararam a escolaridade, e destes, 46,77% relatou ter o ensino fundamental incompleto e 20,49% o ensino médio completo ou superior. O percentual de pacientes que receberam pelo menos dois medicamentos é de 78,97%, e é com esse grupo que a equipe trabalhou, afinal, apenas eles poderiam ter alguma DDI.

É importante ressaltar que a equipe afirmou que não existiam meios para saber se os pacientes realmente tomaram os medicamentos prescritos, então a análise pressupõe que todos os medicamentos prescritos foram administrados.

## 4.2. Método do artigo
Para realizar o trabalho os autores se basearam na versão de 2011 do DrugBank, uma base de dados aberta de medicamentos, que possuí informações de DDI. Essa base possuí identificadores para cada medicamento, chamados de DBID. Foram criadas variáveis para analizar a prescrição de mais de um medicamento simultaneamente, conforme a Figura x, onde "a" é o numero de dias por intervalo de uso do medicamento, "y" é o total de dias do uso do medicamento somando todos os intervalos. Quando mais de um medicamento foi utilizado simultâneamente, eles são analisados em pares, e a quantidade de pares é representada pela variavel "t". Se aquela interação está presente no DrugBank é marcado em vermelho (e recebe valor s = 1), caso contrário é marcado em laranja (e recebe valor s = 0).

Para cada par de DDI observado, existe uma gravidade definida pela base [Drugs.com](drugs.com), sendo classificada em maior, moderada, menor ou n/a. Essas gravidades foram numericamente normalizadas, e baseado nisso foram gerados os pesos das arestas. Os pesos das arestas representam a probilidade de um medicamento ser prescrito simultaneamente com outro medicamento, e seu risco de gerar comorbidades por terem sido co-administrados.

EQ. GERAL PESO AQUI

A partir desta definição acima, foram criadas duas equações para representar o risco de interação em mulheres e em homens, sendo que um é o inverso do outro. 

EQUAÇÕES RRI(F)(M) AQUI

Na geração da rede, os nós representam os medicamentos e as arestas representam as interações entre cada medicamento entre si, sendo que os pesos das arestas são definidos pela equação X já apresentada. Já o tamanho dos nós representam a probabilidade de interação daquele medicamento com outros, e é definido pela equação Y abaixo, sendo que os nós maiores são considerados os mais perigosos de serem co-administrados.

EQ. PROBABILIDADE AQUI.

Por fim, as cores das arestas se baseam nos riscos de interação para homens e mulheres. A cor azul representa os riscos em homens e a cor vermelha representa os riscos em mulheres, e quanto mais escura a cor, maior o risco (consequentemente, quando mais clara, menor o risco). 

Para melhor compreensão da idade neste cenário, os pacientes foram divididos em grupos por idade, e o risco de cada grupo foi calculado, seguindo o mesmo conceito do cálculo por gênero, conforme apresentado na equação Z.

EQ RISCO IDADE AQUI.

A equipe definiu um modelo nulo para treinar um sistema que seja capaz de identifica o aumento esperado do risco de determinada interação considerando o gênero e a faixa etária. Para isso, foram utilizadas ferramentas de aprendizado de máquina, com os classificadores lineares Support Vector Machine (SVM) e Regressão Logística, fazendo a validação cruzada estratificada 4 vezes, para garantir um bom desempenho. As variáveis demográficas usadas foram a Idade (faixa etária), gênero, número de medicamentos, e número de co-administrações. Já como característica binária usou-se uma variável definida como 1 caso o paciente tenha recebido determinado medicamento, e como 0 caso contrário. Isso permite que os classificadores sejam treinados para definir a probabilidade de uma determinada combinação de medicamentos. 

Os classificadores são comparados a três modelos nulos:

* Modelo nulo do tipo "lançamento de moedas" imparcial, em que cada classe tem a mesma probabilidade
* Modelo nulo do tipo "lançamento de moedas" tendencioso, com base na frequência de cada classe
* Modelo nulo que encontra a melhor idade de corte para cada gênero, sendo que acima do crote, todos são considerado afetados pela DDI. 

Para avaliar o desempelho dos classficadores, considerou-se o coeficiente de correlação de Matthew (MCC), a área sober a curva ROC (AUC ROC), e a área sob a curva de precisão e recuperação (AUC P/R).

## 4.3. Dados usados como entrada

COLOCAR TUDO? OU SÓ O QUE USAMOS?

Dataset | Endereço na Web | Resumo descritivo
----- | ----- | -----
comorbidity_odds_matrix | [comorbidity_odds_matrix.csv](https://github.com/brolesi/ds4h/blob/main/p3/comorbidity_odds_matrix.csv) | Uma matriz de 95x95 de adjacência de comorbidades.
comorbidity_pmat_matrix | [comorbidity_pmat_matrix.csv](https://github.com/brolesi/ds4h/blob/main/p3/comorbidity_pmat_matrix.csv) | Uma matriz 95x95 de p-values da regressão logística
comorbidity_sexeffect_odds_matrix | [comorbidity_pmat_matrix.csv](https://github.com/brolesi/ds4h/blob/main/p3/comorbidity_sexeffect_odds_matrix.csv) | Uma matriz 95x95 de efeito de interação do sexo
comorbidity_pmat_matrix | [comorbidity_sexeffect_pval_matrix.csv](https://github.com/brolesi/ds4h/blob/main/p3/comorbidity_sexeffect_pval_matrix.csv) | Uma matriz 95x95 de p-values da interação do sexo a partir da regressão logística

# 5. Método
> Método usado para a análise -- adaptações feitas, ferramentas utilizadas, abordagens de análise adotadas e respectivos algoritmos.
> Etapas do processo reproduzido.

# 6. Resultados
> Apresente os resultados obtidos pela sua adaptação.
> Confronte os seus resultados com aqueles do artigo.
> Esta seção opcionalmente pode ser apresentada em conjunto com o método.
