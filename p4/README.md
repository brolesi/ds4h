# Projeto 4 – Classificação de lesões de substância branca no Lúpus

# Caracterização de lúpus eritematoso sistêmico como lesão isquêmica (acidente vascular cerebral) ou  desmielinizante (esclerose múltipla) através de máquina de vetores de suporte

# Apresentação

O presente projeto foi originado no contexto das atividades da disciplina de pós-graduação [*Ciência e Visualização de Dados em Saúde*](https://ds4h.org), oferecida no primeiro semestre de 2022, na Unicamp.

O relatório em formato PDF pode ser encontrado em [assets/Report_P4.pdf](assets/Report_P4.pdf).

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
[1].

Sua natureza é auto-imune, devido à presença de diversos
auto-anticorpos. O SLE evolui com manifestações clínicas polimórficas,
sendo mais exacerbadas em alguns períodos, e menos em outros, chamado de
período de remissão. Sua etiologia ainda não foi totalmente esclarecida,
mas já foi identificado que o desenvolvimento da doença está ligado a
predisposição genética e fatores ambientais, como luz ultravioleta e
alguns medicamentos [2].

Devido às incertezas relacionadas com essa doença, há vários hipóteses
levantadas e sendo estudadas para identificar etiologias mais prováveis
das lesões presentes em pacientes de SLE.

Um dos órgãos afetados pelo SLE é o cérebro. É muito comum entre os
estudos médicos se falar em lesões na substância branca cerebral (do
inglês *White Matter Lesions* - WMLs), já que elas podem causar deficit
funcional significativo. A etiologia de uma WML pode ser isquêmica ou
desmielinizante [3].

Sabe-se que as lesões isquêmicas podem acontecer, por exemplo, em
pacientes que sofreram um Acidente Vascular Cerebral (AVC) [4]
[5]. Já dentre as doenças desmielinizantes, a principal
e mais conhecida é a Esclerose Múltipla (EM) [6].

A etiologia da lesão é importante para um tratamento adequado e um
método que seja capaz de distinguir lesões isquêmicas de
desmielinizantes pode ser usado para caracterizar lesões com etiologia
desconhecida, como é o caso do SLE.

Até o momento não existem estudos publicados que analisaram WMLs de
diferentes etiologias [3]. Diante disso, as perguntas a serem
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
[7].

Entre as técnicas de *machine learning* existentes, escolheu-se o
*Support-vector machine* (SVM). O SVM é um modelo de aprendizado
supervisionado, que pode ser usado para análises de regressão e de
classificação, como é o caso deste estudo. Além disso, o SVM é um dos
métodos de previsão mais robustos, atuando como um classificador linear
binário não probabilístico. Ele mapeia exemplos de treinamento para
pontos no espaço para maximizar a largura da lacuna entre as duas
categorias existentes. Novos exemplos são então mapeados nesse mesmo
espaço e previstos para pertencer a uma categoria com base em qual lado
da lacuna eles se enquadram [8].

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
uma técnica apresentada no trabalho [9]. Escolhemos usá-la
porque nos estudos, este método apresentou um resultado muito bom nos
classificadores. Essa normalização se baseia no seguinte passo a passo
[10]:

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
não confiável [11].

Entre as formas de buscar essa explicabilidade, há uma técnica chamada
LIME, que explica as previsões de qualquer classificador de maneira
interpretável e fiel, aprendendo um modelo interpretável localmente em
torno da previsão. Esta técnica é flexível também para classificação de
imagem [11].

Com o uso do LIME, espera-se ser possível explicar quais características
mais influenciaram na decisão do classificador pela lesão isquêmica ou
desmielinizante.

### Solidez

Para o presente trabalho, foi feita a análise das máscaras das lesões
considerando a solidez, calculada como:

$$solidez = \frac{\acute{a}rea \ do\ objeto}{fecho \ convexo \ do \ objeto} $$

Ela indica o quando to fecho convexo é utilizado para compor o objeto.
Quanto maior a solidez, maior área dentro do fecho ele ocupa.

Para a análise, foi avaliada cada um dos objetos da imagem, ou seja, se
a máscara possuísse mais de uma marcação de lesão, cada uma delas foi
avaliada individualmente no aspecto de solidez.

### Excentricidade

Para a definição de excentricidade nos apoiamos na figura
[1] conforme [12]
(adaptado) foi obtida a equação:

![Objetos e suas respectivas dimensões para cálculo de excentricidade
[12] (adaptado) - (a) ilustração de eixos maior e menor
de um objeto; (b) objeto com excentricidade alta; (c) objeto com
excentricidade baixa.](assets/excentricidade2.png)
Figura 1: Objetos e suas respectivas dimensões para cálculo de excentricidade
[12] (adaptado) - (a) ilustração de eixos maior e menor
de um objeto; (b) objeto com excentricidade alta; (c) objeto com
excentricidade baixa.

$$excentricidade = \frac{A}{B}$$

Para o cálculo, foi utilizada a abordagem de [12].

Conforme [13], o eixo maior são os pontos do maior segmento
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
classificador (Tab. [1]):

| Classificador | Acurácia | Precisão | *Recall* | f1      | Matriz de Confusão     |
| ------------- | -------- | -------- | -------- | ------- | ---------------------- |
| c\_norm\_roi  | 0.64486  | 0.51049  | 0.92405  | 0.65766 | \[65, 70\], \[6, 73\]  |
| c\_roi        | 0.94860  | 0.89535  | 0.97468  | 0.93333 | \[126, 9\], \[2, 77\]  |
| c\_norm       | 0.84579  | 0.73958  | 0.89873  | 0.81143 | \[110, 25\], \[8, 71\] |
| c\_x          | 0.98131  | 1.0      | 0.94937  | 0.97403 | \[135, 0\], \[4, 75\]  |

Tabela 1: Métricas dos classificadores.

Conforme os resultados apresentados na Tab.
[1], é
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
[2] apresenta o paciente, o tipo de
classificação e o percentual de cada tipo. Quando o percentual é de
100%, significa que todos os cortes daquele paciente tiveram a mesma
classificação. Quando o percentual é menor que 100% significa que houve
uma parte dos cortes daquele paciente classificado como AVC e outra
parte como EM.

| Paciente | Tipo | Percentual         |
| -------- | ---- | ------------------ |
| 600      | AVC  | 1.0                |
| 601      | EM   | 1.0                |
| 602      | EM   | 1.0                |
| 603      | AVC  | 0.4444444444444444 |
| 603      | EM   | 0.5555555555555556 |
| 604      | AVC  | 1.0                |
| 605      | AVC  | 0.3333333333333333 |
| 605      | EM   | 0.6666666666666666 |
| 606      | EM   | 1.0                |
| \...     | \... | \...               |
| 676      | AVC  | 1.0                |
| 677      | AVC  | 1.0                |
| 678      | AVC  | 1.0                |

Tabela 2: Resultado do modelo aplicado aos pacientes com lúpus

Isso é considerado um erro do classificador, por isso, o tipo que
apresentou maior percentual será o tipo classificado para aquele
paciente. Feito isso, temos então o resultado da tabela
[3].

|             | AVC | EM  | Total |
| ----------- | --- | --- | ----- |
| Frequência  | 53  | 26  | 78    |
| Porcentagem | 68% | 32% | 100%  |

Tabela 3: Frequência e porcentagem dos resultados do modelo aplicados ao conjunto de dados de SLE disponibilizado


A figura [2] apresenta as lesões analisadas:

![Imagens típicas das lesões analisadas. À esquerda, cérebro com AVC, ao
centro, cérebro com EM e à direita cérebro com
SLE](assets/avc_em_sle.png)
Figura 2: Imagens típicas das lesões analisadas. À esquerda, cérebro com AVC, ao
centro, cérebro com EM e à direita cérebro com
SLE

## Explicabilidade

Conforme já explicado, utilizou-se o LIME como ferramenta de apoio para
buscar alguma explicabilidade ao resultado do classificador.

Na tabela [4], a primeira coluna representa as
características, e as outras colunas seus valores. Foi escolhido
aleatoriamente um paciente de AVC e outro de EM, e em ambos, as
características que mais influenciaram na decisão foram a variância e o
desvio padrão.

| Características | Paciente AVC | Paciente EM |
| --------------- | ------------ | ----------- |
| SRE             | 0.13         | -0.02       |
| LRE             | -0.80        | 1.10        |
| GLU             | -0.70        | 1.08        |
| RLU             | 1.59         | -0.74       |
| RPC             | 1.56         | -1.00       |
| Mean            | 1.33         | -1.23       |
| Median          | 1.81         | -0.55       |
| Kurtosis        | 1.02         | 1.02        |
| Skewness        | 1.02         | 1.02        |
| Var             | 1.41         | -1.24       |
| Std             | 1.33         | -1.32       |
| diss            | 1.02         | -1.18       |
| cont            | 0.77         | -1.25       |
| eng             | -1.01        | 1.20        |
| corr            | 0.89         | -0.62       |
| ASM             | -0.99        | 1.21        |
| homo            | -1.05        | 1.13        |
| lbp81Mean       | -1.07        | 1.10        |
| lbp81Median     | 0.00         | 0.00        |
| lbp81Kurtosis   | -1.01        | 1.08        |
| lbp81Skewness   | -1.01        | 1.08        |
| lbp81Var        | 1.12         | -1.07       |
| lbp81Std        | 1.09         | -1.06       |

Tabela 4: Explicabilidade para o Treino

![Gráfico de explicabilidade para um paciente com
AVC.](assets/10.png)

Figura 3: Gráfico de explicabilidade para um paciente com
AVC.

![Gráfico de explicabilidade para um paciente com
EM.](assets/212.png)

Figura 4: Gráfico de explicabilidade para um paciente com
EM.

Já para a tabela [5], a primeira coluna representa as
características, e as outras colunas seus valores. Foi escolhido
aleatoriamente um paciente de AVC e outro de EM, as características que
mais influenciaram na decisão foram o conjunto SRE e correlação para o
paciente 600, e conjunto correlação e desvio padrão para o paciente 601.
Nota-se que a textura auxilia na predição do AVC, assim como podemos
verificar que a correlação dos pontos da imagem são relacionadas, mas no
caso do EM são inversamente relacionadas.

| Característica | Paciente 600 (AVC) | Paciente 601 (EM) |
| -------------- | ------------------ | ----------------- |
| SRE            | -0.64              | 0.02              |
| LRE            | -0.66              | -0.32             |
| GLU            | -0.38              | -0.17             |
| RLU            | -0.31              | -0.21             |
| RPC            | 0.27               | -0.06             |
| Mean           | 0.71               | -0.38             |
| Median         | 1.06               | 0.02              |
| Kurtosis       | -1.00              | -1.00             |
| Skewness       | -1.00              | -1.00             |
| Var            | 0.04               | -0.91             |
| Std            | 0.11               | -0.85             |
| diss           | 0.17               | -0.14             |
| cont           | -0.01              | -0.20             |
| eng            | -1.11              | -0.16             |
| corr           | 0.12               | -1.12             |
| ASM            | -1.02              | -0.22             |
| homo           | -0.23              | 0.12              |
| lbp81Mean      | -0.29              | 0.09              |
| lbp81Median    | 0.00               | 0.00              |
| lbp81Kurtosis  | -0.20              | 0.39              |
| lbp81Skewness  | -0.20              | 0.39              |
| lbp81Var       | 0.32               | 0.01              |
| lbp81-0.17Std  | 0.35               | 0.06              |

  Tabela 5: Explicabilidade para o Teste

![Gráfico de explicabilidade para um paciente com Lúpus classificado
como AVC.](assets/600.png)

Figura 5: Gráfico de explicabilidade para um paciente com Lúpus classificado
como AVC.

![Gráfico de explicabilidade para um paciente com Lúpus classificado
como EM.](assets/601.png)

Figura 6: Gráfico de explicabilidade para um paciente com Lúpus classificado
como EM.
## Solidez

Com relação à solidez, o que foi obtido a partir da análise dos dados
presentes a partir das máscaras das lesões de cérebro (considerando AVC,
EM e SLE), a Figura [7] mostra o histograma para estes tipos.
Nota-se que um volume alto de registros entre 0.7 e 0.9, mostrando que
as lesões em sua maioria tem a área próxima ao fecho convexo da própria
lesão. Também é possível notar, a partir da estimativa de densidade por
*Kernel*, que SLE e EM parecem ter uma forma parecida.

![Resultado do cálculo de solidez das máscaras de lesões apresentadas
para o trabalho.](assets/solidez_resultado.png)

Figura 7: Resultado do cálculo de solidez das máscaras de lesões apresentadas
para o trabalho.

## Excentricidade

Observou-se a partir das imagens de lesão o resultado do histograma
presente na Figura
[8], ou seja, máscaras de AVC com
maior frequência e também com maior excentricidade, e excentricidade de
EM e SLE concentradas próximo a valores menores, mostrando que são
lesões menos longas.

![Resultado do cálculo de excentricidade das máscaras de lesões
apresentadas para o
trabalho.](assets/excentricidade_resultado.png)

Figura 8: Resultado do cálculo de excentricidade das máscaras de lesões
apresentadas para o
trabalho.

# Limitações


-   As análises foram feitas com imagens BMP e PNG projetadas para
    escala de cinza;

-   Para os padrões binários locais, uma das características da imagem,
    utilizamos apenas a parametrização com o raio como 1 e a quantidade
    de pontos vizinhos circularmente simétricos como 8;

-   Para a matriz de comprimento de corrida a orientação é de 0º e a
    normalização do nível de pixels é 8;

-   Apenas imagens com máscara foram consideradas para o treino;

-   Não foi feita uma análise de qualidade acerca das máscaras para
    avaliar se elas faziam sentido (não estavam invertidas, por
    exemplo);

-   Os resultados são limitados a imagens com características
    semelhantes à massa de dados que possuímos;

-   Não foram utilizadas técnicas de aumentação de dados, nem redes
    neurais profundas, mas as características das imagens já citadas em
    [3](#Metodologia){reference-type="ref" reference="Metodologia"};

-   Não foi feita uma validação sobre potenciais *outliers* com relação
    a características das máscaras de lesão;

-   As pessoas que criaram o presente documento não tem domínio completo
    do tema para realizar qualquer julgamento sobre qualidade das
    imagens e/ou máscaras.

# Conclusão


Conforme os resultados do classificador, entende-se que as lesões de SLE
sejam mais próximas da isquêmica (AVC). Porém, ao analisar os resultados
de solidez e excentricidade, pode-se dizer que o formato das lesões se
aproximaram mais das lesões desmielinizantes (EM).

O classificador não usou nenhum parâmetro relacionado ao formato para
tomar decisões. Se a solidez e excentricidade realmente possuem
influência na lesão de Lúpus, seria interessante treinar outro
classificador considerando estes parâmetros, o que pode ser avaliado
como um trabalho futuro.

Para o presente trabalho, utilizou-se de descritores de imagem e
estatísticas de histograma. Um ponto importante a ser colocado é que os
descritores foram escolhidos sem critérios definidos por profissionais
da área de saúde, o que pode trazer certa dificuldade para um resultado
e/ou análises mais precisas e aprofundadas.

Dado o cenário de que não houve um acesso a profissionais de saúde para
validação e modelo, e pensando numa possibilidade de abarcar mais
características, uma *feature engineering* ou engenharia de atributos
poderia ser delegada a uma rede neural convolucional, construída
manualmente ou a partir de *transfer learning*) a fim de maximizar as
possibilidades e potencializar a resposta no sentido de garantir uma
maior abrangência e acurácia para o modelo final.

Vale ressaltar que neste caso, a explicabilidade do modelo ficaria
comprometida dado que os modelos baseados em redes neurais profundas tem
características que não permitem um detalhamento tão grande quanto
extração manual de características de imagens baseadas em descritores
(como de forma, área, textura, por exemplo).

# Referências

[1] E. F. e. a. Borba, “Consenso de lúpus eritematoso sistêmico,” Revista Brasileira de Reumatologia, vol. 48,
no. 4, pp. 196–207, Nov 2008.

[2] H. A. e. a. Magalhães e Silva, “Lúpus eritematoso sistêmico: uma revisão atualizada da fisiopatologia ao
tratamento,” Brazilian Journal of Health Review, vol. 4, no. 6, pp. 24 074–24 084, Dez 2021.

[3] L. Rittner, “Classificação de wml baseado em etiologia,” Jun 2022.

[4] O. d. e. a. Castro e Silva Jr., “Aspectos básicos da lesão de isquemia e reperfusão e do pré-condicionamento
isquêmico,” Acta Cirúrgica Brasileira, vol. 17, no. 3, pp. 96–100, Abr 2003.

[5] D. J. A. Morsch. (2021) Isquemia cerebral: Sintomas, riscos, sequelas e diferenÇas do avc. [Online].
Available: https://telemedicinamorsch.com.br/blog/isquemia-cerebral

[6] D. M. Miranda. (2013) Doenças desmielinizantes. [Online]. Available: http://www.ineuro.com.br/para-os-pacientes/doencas-desmielinizantes/comment-page-1/#comments

[7] B. Copeland. artificial intelligence. [Online]. Available: https://www.britannica.com/technology/
artificial-intelligence/Reasoning

[8] V. Cortes, Corina Vapnik, “Support-vector networks,” Machine Learning - Kluwer Academic Publishers,
vol. 20, no. 3, pp. 273–297, 1995.

[9] R. L. e. a. Leite M, “Influence of mr image intensity normalization on texture-based classification of brain
white matter lesions.” JECN, vol. 21, no. 2, pp. 46–77, 2015.

[10] C. G. . S. M. . M. F, “Influence of mri acquisition protocols and image intensity normalization methods on
texture classification.” Magn Reson Imaging, vol. 22, pp. 81–91, 2004.

[11] M. T. Ribeiro, S. Singh, and C. Guestrin, “"why should i trust you?": Explaining the predictions of any
classifier,” Ago 2016.

[12] H. Pedrini, “Introduçao ao processamento digital de imagem mc920 / mo443.” [Online]. Available:
https://www.ic.unicamp.br/~helio/disciplinas/MO443/aula_representacao.pdf

[13] M. A. Wirth, “Shape analysis and measurement,” University of Guelph. CIS, vol. 6320, 2001.
