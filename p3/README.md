# Apresentação

O presente projeto foi originado no contexto das atividades da disciplina de pós-graduação [*Ciência e Visualização de Dados em Saúde*](https://ds4h.org), oferecida no primeiro semestre de 2022, na Unicamp.


| Nome                          | RA     | Especialização |
| ----------------------------- | ------ | -------------- |
| Bruna Osti                    | 231024 | Computação     |
| Fabio Fogliarini Brolesi      | 023718 | Computação     |
| Ingrid Alves de Paiva Barbosa | 182849 | Computação     |

Tabela 1 - Equipe autora do projeto.

# Referência bibliográfica do artigo lido
Alexander-Bloch Aaron F., Raznahan Armin, Shinohara Russell T., Mathias Samuel R., Bathulapalli Harini, Bhalla Ish P., Goulet Joseph L., Satterthwaite Theodore D., Bassett Danielle S., Glahn David C. and Brandt Cynthia A. 2020The architecture of co-morbidity networks of physical and mental health conditions in military veteransProc. R. Soc. A.4762019079020190790
[http://doi.org/10.1098/rspa.2019.0790](http://doi.org/10.1098/rspa.2019.0790)

# Resumo
> Escreva um breve do artigo (com as suas palavras, não deve ser copiado texto do artigo).

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