# Projeto 4 – Classificação de lesões de substância branca no Lúpus

# Caracterização de lúpus eritematoso sistêmico como lesão isquêmica (acidente vascular cerebral) ou  desmielinizante (esclerose múltipla) através de máquina de vetores de suporte

# Apresentação

O presente projeto foi originado no contexto das atividades da disciplina de pós-graduação [*Ciência e Visualização de Dados em Saúde*](https://ds4h.org), oferecida no primeiro semestre de 2022, na Unicamp.


| Nome                          | RA     | Especialização |
| ----------------------------- | ------ | -------------- |
| Bruna Osti                    | 231024 | Computação     |
| Fabio Fogliarini Brolesi      | 023718 | Computação     |
| Ingrid Alves de Paiva Barbosa | 182849 | Computação     |

# Introdução

O Lúpus Eritematoso Sistêmico (LES, ou do inglês *systemic lupus
erythematosus* - SLE) é uma doença inflamatória crônica e de causa ainda
desconhecida. São reconhecidos dois tipos principais de lúpus, sendo (i)
o cutâneo, que se manifesta apenas com manchas na pele, e (ii) o
sistêmico, em que um ou mais órgãos internos são acometidos
[@borba2008].

Sua natureza é auto-imune, devido à presença de diversos
auto-anticorpos. O SLE evolui com manifestações clínicas polimórficas,
sendo mais exacerbadas em alguns períodos, e menos em outros, chamado de
período de remissão. Sua etiologia ainda não foi totalmente esclarecida,
mas já foi identificado que o desenvolvimento da doença está ligado a
predisposição genética e fatores ambientais, como luz ultravioleta e
alguns medicamentos [@magalhaes2021].

Devido às incertezas relacionadas com essa doença, há vários hipóteses
levantadas e sendo estudadas para identificar etiologias mais prováveis
das lesões presentes em pacientes de SLE.

Um dos órgãos afetados pelo SLE é o cérebro. É muito comum entre os
estudos médicos se falar em lesões na substância branca cerebral (do
inglês *White Matter Lesions* - WMLs), já que elas podem causar deficit
funcional significativo. A etiologia de uma WML pode ser isquêmica ou
desmielinizante [@rittner2022].

Sabe-se que as lesões isquêmicas podem acontecer, por exemplo, em
pacientes que sofreram um Acidente Vascular Cerebral (AVC) [@castro2003]
[@telemedicina2021]. Já dentre as doenças desmielinizantes, a principal
e mais conhecida é a Esclerose Múltipla (EM) [@ineuro2013].

A etiologia da lesão é importante para um tratamento adequado e um
método que seja capaz de distinguir lesões isquêmicas de
desmielinizantes pode ser usado para caracterizar lesões com etiologia
desconhecida, como é o caso do SLE.

Até o momento não existem estudos publicados que analisaram WMLs de
diferentes etiologias [@rittner2022]. Diante disso, as perguntas a serem
respondidas neste estudo seguem abaixo:

**RQ1: Qual é a etiologia mais provável das lesões presentes em
pacientes de SLE (isquêmica ou desmielinizante)?**

**RQ2: Quais características da lesão mais influenciaram na sua
classificação?**

# Objetivo

Diante do exposto, o objetivo do presente trabalho é criar um
classificador baseado em *machine learning* que seja capaz de
identificar, a partir de imagens médicas, qual a etiologia mais provável
das WMLs (isquêmicas ou desmielinizantes) presentes em pacientes de
Lúpus Eritematoso Sistêmico (SLE).

Além disso, pretende-se identificar quais características da lesão mais
influenciam na classificação da lesão como isquêmica ou desmielinizante,
para a partir disso, aproximar-se de uma etiologia para o SLE.

# Metodologia {#Metodologia}


Para responder as questões de pesquisa levantadas e atingir o objetivo
deste trabalho, é necessário um método capaz de classificar lesões a
partir de imagens médicas. A inteligência humana não é capaz de
identificar as características necessárias para a classificação, por
isso, recorre-se à inteligências artificiais.

Entre as abordagens existentes, optou-se pelo uso de técnicas de
aprendizado de máquina (do inglês *machine learning*). Apesar de não ter
precisão tão alta quanto técnicas de *deep learning*, o *machine
learning* possui explicabilidade, fator importante para responder a RQ2
[@britannica0000].

Entre as técnicas de *machine learning* existentes, escolheu-se o
*Support-vector machine* (SVM). O SVM é um modelo de aprendizado
supervisionado, que pode ser usado para análises de regressão e de
classificação, como é o caso deste estudo. Além disso, o SVM é um dos
métodos de previsão mais robustos, atuando como um classificador linear
binário não probabilístico. Ele mapeia exemplos de treinamento para
pontos no espaço para maximizar a largura da lacuna entre as duas
categorias existentes. Novos exemplos são então mapeados nesse mesmo
espaço e previstos para pertencer a uma categoria com base em qual lado
da lacuna eles se enquadram [@corin1995].

O aprendizado do modelo será feito com base em imagens de lesões
isquêmicas ou desmielinizantes. Para lesões isquêmicas serão
consideradas imagens de pacientes com Acidente Vascular Cerebral (AVC) e
para lesões desmielinizantes serão consideradas imagens de pacientes com
Esclerose Múltipla (EM).

O classificador, sabendo identificar apenas AVC ou EM, irá dizer a qual
categoria cada imagem de SLE pertence. Com base neste resultado, será
possível dizer se existe alguma etiologia mais provável para as lesões
de SLE.

O processo de classificação é divido em etapas, conforme apresentado nas
sessões a seguir.

## Pré-processamento

Antes de entrar com as imagens no classificador, elas são
pré-processadas para ressaltar características importantes, e eliminar
algum provável viés ou ruído que pode te sido incluído pelo processo de
captura.

Entre as formas de pré-processamento possíveis, considerou-se a
normalização e o recorte da região de interesse.

### Normalização

No processo de captura é comum que algumas imagens fiquem mais claras,
outras mais escuras, e que suas intensidades variem em escalas
diferentes. Por isso, a normalização é aplicada, com a intenção de fazer
todas as imagens terem intensidades que variam dentro de uma mesma
escala. Foram estudadas diversas formas de normalização, foi escolhida
uma técnica apresentada no trabalho [@rittner2015]. Escolhemos usá-la
porque nos estudos, este método apresentou um resultado muito bom nos
classificadores. Essa normalização se baseia no seguinte passo a passo
[@collewet2004]:

-   Retira-se a média de todas as imagens a serem usadas no treino.

-   Encontra-se a maior média de todas computadas anteriormente.

-   Calcula-se a razão entre a maior média e a média da imagem a ser
    normalizada.

-   Multiplica-se toda a imagem por essa razão.

Com esse método, a imagem terá a intensidade de todos os seus *pixels*
alterados, podendo variar em mais de 255, e isso deve ser levado em
consideração. Entretanto, ela faz com que todas as imagens tenham o
mesmo valor médio, eliminando algum provável viés incluído no processo
de captura.

Não é possível afirmar com certeza se o classificador terá melhores
resultados para imagem com normalização ou sem. Por isso, ambas as
possibilidades serão testadas e terão seus resultados comparados para
que seja feita a melhor decisão.

### Região de Interesse (ROI)

No caso de imagens com WMLs, a região que realmente interessa é onde
está a lesão, e há uma grande fundo em preto. Isso pode atrapalhar nos
valores dos parâmetros da imagem, e pode ser interessante recortar
apenas a região de interesse. Há algumas formas de fazer esse recorte, e
a que escolhida aqui é a partir das máscaras com dilatação.

Na aplicação das máscaras, apenas a região da lesão fica com as cores
originais e o restante se torna um fundo preto. Depois, pode-se recortar
uma caixa (*bounding-box*) para eliminar um pouco do fundo. Além disso,
é interessante dilatar a máscara, para que possa ser observado uma
pequena da parte saudável do cérebro, para fins de comparação com a
parte lesionada.

Da mesma forma que na normalização, não é possível afirmar com certeza
se o classificador terá melhores resultados para imagem com ROI (com
dilatação ou não) ou o uso da imagem inteira. Por essa razão, ambas as
possibilidades serão testadas e terão seus resultados comparados para
que seja feita a melhor decisão.

## Extração de características

Depois de pré-processar as imagens, já é possível extrair as suas
características, que serão analisadas pelo classificador SVM.

Os dados a serem extraídos são os seguintes:

-   Dados da imagem:

    -   média das intensidades dos *pixels* da imagem

    -   mediana das intensidades dos *pixels* da imagem

    -   variância das intensidades dos *pixels* da imagem

    -   desvio padrão das intensidades dos *pixels* da imagem

-   Dados do histograma (frequência das intensidades dos *pixels* da
    imagem):

    -   curtose do histograma da imagem

    -   assimetria (*skewness*) do histograma da imagem

-   dados de matriz de co-ocorrência:

    -   dissimilaridade

    -   contraste

    -   energia

    -   correlação

    -   homogeneidade

    -   segundo momento angular

-   matriz de comprimento de corrida:

    -   corrida longa

    -   corrida curta

    -   uniformidade de níveis de cinza

    -   uniformidade de tamanho de corrida

    -   porcentagem de corrida

-   Resultados imagem e de histograma da imagem a partir de padrões
    binários locais a partir das imagens com máscaras aplicadas e o
    recorte para a região de interesse feito

## Treino e validação

O treino será feito com 80% das imagens (escolhidas aleatoriamente) e as
20% restantes serão usadas como validação. É importante ressaltar que os
pacientes que tiveram suas imagens usadas no treino não serão
considerados novamente para validação.

## Teste

Após a análise dos resultados do treino e validação, será possível dizer
qual é a melhor forma de pré-processar a imagens, e também quais são as
características relevantes na classificação.

Com base nestas decisões, as imagens de SLE serão adicionadas ao
classificador, que irá dizer se elas se referem à AVC ou EM.

## Análise

Após a realização dos testes, a etapa mais importante deste trabalho
será feito. Com base no resultado, saberemos se há alguma provável
etiologia para SLEs.

Além disso, iremos observar as imagens classificadas em cada tipo para
buscar alguma explicabilidade, ou seja, dizer quais características
influenciaram mais na classificação de cada tipo. Também serão feitos
testes de solidez e excentricidade para verificar se estas
características estão relacionadas ao resultado identificado.

Os métodos escolhido para explicabilidade, solidez e excentricidade
serão explicados abaixo.

### Explicabilidade

Compreender as razões por trás das previsões é algo muito importante
para avaliar a confiança do modelo. Apesar de o classificador ser o
núcleo da solução, o especialista da área da saúde continua sendo a peça
fundamental para agir com base em uma previsão. É preciso ter essa
compreensão do modelo para fornecer dados ao especialista, permitindo
que ele avalie, com base nos conceitos da área se aquela previsão é ou
não confiável [@ribeiro2016].

Entre as formas de buscar essa explicabilidade, há uma técnica chamada
LIME, que explica as previsões de qualquer classificador de maneira
interpretável e fiel, aprendendo um modelo interpretável localmente em
torno da previsão. Esta técnica é flexível também para classificação de
imagem [@ribeiro2016].

Com o uso do LIME, espera-se ser possível explicar quais características
mais influenciaram na decisão do classificador pela lesão isquêmica ou
desmielinizante.

### Solidez

Para o presente trabalho, foi feita a análise das máscaras das lesões
considerando a solidez, calculada como:

$$solidez = \frac{\acute{a}rea \; dobjeto \; imagem}{fecho \; convexo \; do \; objeto} $$

Ela indica o quando to fecho convexo é utilizado para compor o objeto.
Quanto maior a solidez, maior área dentro do fecho ele ocupa.

Para a análise, foi avaliada cada um dos objetos da imagem, ou seja, se
a máscara possuísse mais de uma marcação de lesão, cada uma delas foi
avaliada individualmente no aspecto de solidez.

### Excentricidade

Para a definição de excentricidade nos apoiamos na figura
[1](#fig:excentricidade){reference-type="ref"
reference="fig:excentricidade"} conforme [@pedriniintroduccao]
(adaptado) foi obtida a equação
[\[eq:Excentricidade\]](#eq:Excentricidade){reference-type="ref"
reference="eq:Excentricidade"}:

![Objetos e suas respectivas dimensões para cálculo de excentricidade
[@pedriniintroduccao] (adaptado) - (a) ilustração de eixos maior e menor
de um objeto; (b) objeto com excentricidade alta; (c) objeto com
excentricidade baixa.](assets/excentricidade2.png)

$$excentricidade = \frac{A}{B}$$

Para o cálculo, foi utilizada a abordagem de [@pedriniintroduccao].

Conforme [@wirth2001shape], o eixo maior são os pontos do maior segmento
de reta que pode ser definido dentro do objeto. Eles são computados a
partir da avaliação de todos os pixels de borda e encontrando o par com
a maior distância, como $P_1={x_1, y_1}$ e $P_2 = {x_2, y_2}$.

Então a dimensão do eixo maior do objeto foi dada por:

$$tamanho \; do \; eixo \; maior = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$$

Para a análise, avaliou-se cada um dos objetos da imagem, ou seja, caso
a máscara possuísse mais de uma marcação de lesão, cada uma delas foi
avaliada individualmente no aspecto de excentricidade.

# Resultados e Discussões

Seguem abaixo os resultados obtidos, juntamente com as discussões.

## Pré-processamento, treino e validação

A equipe teve acesso às imagens de pacientes com lesões de AVC e EM,
sendo que apenas às que possuíam máscaras tinham alguma lesão visível.
No total 511 imagens de AVC com máscara, de 50 pacientes distintos e 628
imagens de EM com máscara, de 51 pacientes distintos, usadas no treino e
validação.

Para identificar o melhor pré-processamento, foram treinados 4
classificadores diferentes: (i) com normalização e com a combinação de
ROI e dilatação (c\_norm\_roi), (ii) apenas com a combinação de ROI e
dilatação (c\_roi), (iii) apenas com normalização (c\_norm), (iv) sem
pré-processamento (c\_x).

Após o treino e validação, obteve-se as seguintes métricas para cada
classificador (Tab. [1](#tab:metricas){reference-type="ref"
reference="tab:metricas"}):

::: {#tab:metricas}
  Classificador   Acurácia   Precisão   *Recall*   f1        Matriz de Confusão
  --------------- ---------- ---------- ---------- --------- ------------------------
  c\_norm\_roi    0.64486    0.51049    0.92405    0.65766   \[65, 70\], \[6, 73\]
  c\_roi          0.94860    0.89535    0.97468    0.93333   \[126, 9\], \[2, 77\]
  c\_norm         0.84579    0.73958    0.89873    0.81143   \[110, 25\], \[8, 71\]
  c\_x            0.98131    1.0        0.94937    0.97403   \[135, 0\], \[4, 75\]

  : Métricas dos classificadores.
:::

[\[tab:metricas\]]{#tab:metricas label="tab:metricas"}

Conforme os resultados apresentados na Tab.
[1](#tab:metricas){reference-type="ref" reference="tab:metricas"}, é
possível perceber que os classificadores c\_norm\_roi e c\_norm tiveram
resultados ruins, por isso foram desconsiderados. O classificador c\_roi
teve um resultado muito bom, porém o classificador sem pré-processamento
(c\_x) teve um valor 0 na matriz de confusão. Isso significa que o
classificador acertou todas as classificações do conjunto de validação,
resultando em uma precisão de 100%.

Essa situação normalmente é vista como um provável *overfitting*, que
faria o classificador ter um péssimo resultado no teste. Porém, quando a
divisão de treino e validação é de 80%/20% ou superior, a chance de
*overfitting* reduz bastante. Além disso, a acurácia foi maior para o
c\_x, e o f1 foi tão bom quanto o c\_roi. Por isso, o risco de
*overfitting* passa a ser baixo, e optou-se por seguir com esse
classificador, que não usa nenhuma normalização ou recorte de ROI.

Como não havia nenhum pré-processamento, foi necessário adicionar
minimamente um ajuste de escala, com a técnica *StandardScale*. Esta
técnica faz com que as intensidades das imagens variem sempre entre -1 e
1, sendo que -1 equivale ao preto, e 1 equivale ao branco. Todos os
outros valores dentro dessa faixa são tons de cinza.

Verificou-se que o *k-fold* de 5 seria melhor para o treino/validação, a
fim de montar um algoritmo mais robusto antes de executar a validação
nos 20% propriamente ditos. Reforça-se aqui que os pacientes usados no
treino não foram usados na validação.

Teste
-----

Foram disponibilizadas 697 imagens de SLE com máscara, de 78 pacientes
distintos. Tendo sido o classificador escolhido treinado e validado,
partiu-se para o teste.

Cada uma das imagens de teste também tiveram suas escalas ajustadas pelo
*StandardScale*, da mesma forma que na validação.

Entre as 697 imagens testadas, 463 foram classificadas como AVC (66,43%)
e 234 foram classificadas como EM (33,57%).

Como as imagens apenas apresentam duas dimensões, para que seja possível
ver o cérebro em profundidade, são feitos vários cortes. Teoricamente,
para um mesmo paciente, todos os cortes deveriam ter as lesões
classificadas como um mesmo tipo. Porém, na prática, não foi isso que
aconteceu. Houveram 35 pacientes que tiveram seus recortes classificados
de forma diferente. Por isso, a tabela
[2](#tabela_resultado){reference-type="ref"
reference="tabela_resultado"} apresenta o paciente, o tipo de
classificação e o percentual de cada tipo. Quando o percentual é de
100%, significa que todos os cortes daquele paciente tiveram a mesma
classificação. Quando o percentual é menor que 100% significa que houve
uma parte dos cortes daquele paciente classificado como AVC e outra
parte como EM.

::: {#tabela_resultado}
  Paciente   Tipo             Percentual
  ---------- ------ --------------------
  600        AVC                     1.0
  601        EM                      1.0
  602        EM                      1.0
  603        AVC      0.4444444444444444
  603        EM       0.5555555555555556
  604        AVC                     1.0
  605        AVC      0.3333333333333333
  605        EM       0.6666666666666666
  606        EM                      1.0
  \...       \...                   \...
  676        AVC                     1.0
  677        AVC                     1.0
  678        AVC                     1.0

  : Resultado do modelo aplicado aos pacientes com lúpus
:::