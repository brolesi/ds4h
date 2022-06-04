# Apresentação

O presente projeto foi originado no contexto das atividades da disciplina de pós-graduação [*Ciência e Visualização de Dados em Saúde*](https://ds4h.org), oferecida no primeiro semestre de 2022, na Unicamp.


| Nome                          | RA     | Especialização |
| ----------------------------- | ------ | -------------- |
| Bruna Osti                    | 231024 | Computação     |
| Fabio Fogliarini Brolesi      | 023718 | Computação     |
| Ingrid Alves de Paiva Barbosa | 182849 | Computação     |

Tabela 1 - Equipe autora do projeto.

# Referência bibliográfica do artigo lido
Brattig Correia, R., de Araújo Kohler, L., Mattos, M.M. et al. City-wide electronic health records reveal gender and age biases in administration of known drug–drug interactions. npj Digit. Med. 2, 74 (2019). https://doi.org/10.1038/s41746-019-0141-x

# Resumo
O artigo escolhido se chama "_City-wide electronic health records reveal gender and age biases in administration of known drug–drug interactions_" e aborda sobre as reações adversas a medicamentos (ADR, do inglês _Adverse drug reactions_) de interações medicamentosas (DDI, do inglês _drug–drug interactions_), que é um tema muito discutido na medicina ao redor do Mundo. 

A ADR é uma resposta prejudicial ou indesejável, não intencional, do organismo, que ocorre após utilizar a um medicamento. Dentre as razões que levam a uma ADR, 30% delas são devidas às interações medicamentosas, e os pacientes com complicações decorrentes da DDI tendem a reentrar no sistema de saúde em um nível mais caro. Além disso, estima-se que 52% das reações em pacientes ambulatoriais eram evitáveis. Se essas reações forem evitadas, haverá uma grande economia no setor de saúde, principalmente no âmbito público, além de reduzir os riscos de problemas de saúde para os pacientes.

Diante desse cenário, o trabalho visou realizar um estudo longitudinal em larga escala do fenômeno DDI nos níveis de atenção primária, nas Unidades Básicas de Saúde (UBS), e secundária, nas Unidades de Pronto Atendimento (UPA 24h), em uma cidade inteira, usando janelas de tempo consideravelmente maiores e contando com padrões públicos de DDI e ADR. Para realiar este estudo, foi feito um estudo de caso referente à população de Blumenau, no estado de Santa Catarina, Sul do Brasil. Blumenau é uma cidade que no ano da realização do trabalho tinha uma população aproximada de 340mil habitantes e um IDH igual a 0,806, considerado um valor muito alto. Em Blumenau, o custo anual com problemas de saúde resultantes de DDI em pessoas acima de 65 anos é US$2 (per capita), podendo chegar á US$7 se aplicadas taxas e inflações. Estes valores mostram que este problema é mais grave do que se pensava anteriormente, e quanto mais velha é a população, maior é esse custo.  

Os autores do trabalho tiveram acesso à todos os protuários prontuários médicos de 18 meses, fornecidos pela prefeitura do município. Após as análises de dados e construção das redes complexas, foram constatados 181 pares de medicamentos que podem interagir e causar reações foram dispensados, mas 4% dos pacientes receberam pares de medicamentos que provavelmente resultarão em ADR. Ao profundar na análise dos dados, também foi identificado que mulheres tem um risco 60% maior de DDI do que homens, e essa taxa pode aumentar para 90% quando considera-se as interações com graves reações, e curiosamente, foi identificado que isso esta relacionado a causas sociais ou biológicas não conhecidas ainda. As pessoas idosas, com idade entre 70 e 79 anos têm um risco de 34% de DDI. Por fim, eles também forneceram uma rede de medicamentos e fatores demográficos paran categorizar o DDI.

# Breve descrição do experimento/análise do artigo que foi replicado
> Descreva brevemente a parte do artigo cujo experimento ou análise foi reproduzido. Explique o que foi usado como entrada e saída.

## Dados usados como entrada
Dataset | Endereço na Web | Resumo descritivo
----- | ----- | -----
comorbidity_odds_matrix | [comorbidity_odds_matrix.csv](https://github.com/brolesi/ds4h/blob/main/p3/comorbidity_odds_matrix.csv) | Uma matriz de 95x95 de adjacência de comorbidades.
comorbidity_pmat_matrix | [comorbidity_pmat_matrix.csv](https://github.com/brolesi/ds4h/blob/main/p3/comorbidity_pmat_matrix.csv) | Uma matriz 95x95 de p-values da regressão logística
comorbidity_sexeffect_odds_matrix | [comorbidity_pmat_matrix.csv](https://github.com/brolesi/ds4h/blob/main/p3/comorbidity_sexeffect_odds_matrix.csv) | Uma matriz 95x95 de efeito de interação do sexo
comorbidity_pmat_matrix | [comorbidity_sexeffect_pval_matrix.csv](https://github.com/brolesi/ds4h/blob/main/p3/comorbidity_sexeffect_pval_matrix.csv) | Uma matriz 95x95 de p-values da interação do sexo a partir da regressão logística

# Método
> Método usado para a análise -- adaptações feitas, ferramentas utilizadas, abordagens de análise adotadas e respectivos algoritmos.
> Etapas do processo reproduzido.

# Resultados
> Apresente os resultados obtidos pela sua adaptação.
> Confronte os seus resultados com aqueles do artigo.
> Esta seção opcionalmente pode ser apresentada em conjunto com o método.
