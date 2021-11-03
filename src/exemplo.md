Gravity sort
======



![](C:\Users\Dual Stream\Downloads\vambora\vambora\src\gravitysort.png)



Implementacao no software
---------



Apos a apresentacao, tem-se algumas conclusoes a respeito do Gravity sort:

1. **Alto desempenho** quando utilizado em algum hardware.
2. **Dificuldade** de implementacao computacional.

Tal dificuldade se da pois **deve-se criar uma maneira do computador realizar essa operacao** - que para nos, parece simples - de "queda das bolinhas".



Ábaco
---------

  Como se baseia em um ábaco, em um primeiro passo devemos compreender como traduzí-lo em termos de *software* e em um segundo momento implementá-lo para utilizar o *gravity sort* 





Portanto, a largura da estrutura define o comprimento **c** de cada fileira, ao passo que o comprimento da estrutura define a quantidade de fileiras **f** que podem ser adicionadas



Uma vez pronta a esturua podemos adicionar as contas (representadas por ⚫) deixando ou não espaços entre elas (representados por ◯). Por simplicidade, será considerado que um espaço possui comprimento contante e igual ao de uma conta, já que, dessa forma, será  fácil compreender a distância entre duas contas



Logo, uma representação plausível de um ábaco seria a seguinte:


| c = 5   f = 4 |
| :------------ |
| ◯⚫◯◯◯         |
| ◯⚫◯⚫⚫         |
| ⚫◯◯◯◯         |
| ⚫◯⚫⚫◯         |
|               |

Pensando em agora em uma implementação de *software* podemos pensar em um ábaco como uma matriz $$M_{ij}$$ sendo i=c e j=f, onde as contas seriam representadas por um valor (por exemplo, `1`) enquanto os espaços seriam representados por outro valor(por exemplo, `0`).

Logo, o ábaco anteriormente citado poderia ser representado em termos de código da seguinte forma:

```c
int c = 15
int f = 5
int abaco[c][f] = { {0, 1, 0, 0, 0},
                    {0, 1, 0, 1, 1},
                    {1, 0, 0, 0, 0},
                    {1, 0, 1, 1, 0} };
```

  Portanto o primeiro objetivo consiste em obter os valores de c e f e "construir" o ábaco para, posteriormente, distribuir as contas.

  Obter o valor de **c** é uma tarefa razoávelmente simples, uma vez que este se resume em contar a quantidade de valores a serem distribuídos no ábaco. Um abordagem razóavel é contar quantos elementos estão disponíveis dentro do array inicial:

```c
int abacus_c(int[] input, int n){
	int c = 0;
	for (int i = 0; i < n; i++) {
        c ++;
    }
}
	return n
```

Como em C esse valor é fornecido ao passar um array, a tarefa se torna ainda mais simples,  se tornando simplesmente:

```c
int abacus_c(int[] input, int n) {
    return n
}
```

Já o valor de **f** exige um pouco mais de trabalho, pois a quantidade de fileiras deve ser tal que não falte ao distribuir horizontalmente as contas. Uma abordagem razoável seria buscar pelo maior valor do array inicial, garantindo que não falte espaço para os demais valores do array:

```c
int abacus_f(int[] input, int n):
	int f = 0;
	for (int i = 0; i < n; i++) {
        if (input[i] > f) {
            f = input[i]
		}
    }
	return f;
```

Note que, em escopo geral, as funções `abacus_c` e `abacus_f` são, respectivamente, as funções `len` e `max`.

Com ambos os valores disponíveis podemos por fim, contruir o ábaco, inicializando uma matriz $$M_{cf} $$ com zeros:

```c
int c = abacus_c(input, n);
int f = abacus_f(input, n);
int abacus[c][f];
```

Partindo agora para a segunda parte, devemos distribuir as contas referentes a cada um dos valores do array inicial, de tal forma que a quantidade de contas verticalmente empilhadas corresponda ao valor do a rray inicial. A função  que descreveria esse procedimento está descrita abaixo:

```c
void distribute_beads(int[] input, int[][] *abacus, int c, int f) {
}
```

#TODO:

Pergunta: por que o valor de n não foi passado para a função?

Resposta: por que c=n

Sabemos que o primeiro loop é tal que acessa cada um dos valores do array inicial e cada um das colunas da matriz do ábaco, portanto sua implementação é 



```c
for (int i = 0; i < c; i++) {
    int val = input[i]
    for (int j = val; j > 0; j--) {
        abacus[j][i] = 1;
	}
    
}
```



Vamos considerar o array abaixo, que deve ser organizado de forma crescente. Note que a operacao de pegar o tamanho do array ja foi realizada.

```
x[] = {5, 3, 13, 7, 4, 11, 20, 17};

int len = sizeof(x)/sizeof(x[0]);
```

O array apresenta um total de 8 itens, o que correspondem as 8 "colunas" de um abaco. Sabemos tambem que cada coluna apresenta um numero de "bolinhas", que eh representada pelo numero inteiro correspondente. Logo, o array pode ser melhor visualizado da seguinte forma:

![](C:\Users\Dual Stream\Downloads\vambora\vambora\src\array.png)

Para traduzir isso no mundo computacional, devemos avancar por partes. Primeiro, deve-se **criar a matriz que representa o abaco**, realizando a seguinte operacao:

```c
unsigned char *beads;
#define BEAD(i, j) beads[i * max + j]

for (i = 1, max = a[0]; i < len; i++)
    if (a[i] > max) max = a[i];
beads = calloc(1, max * len);
```

Definimos um array  que consta todos os itens - ou "bolinhas" - da matriz, sendo o [[for]] utilizado para checar o inteiro de maior valor do input - no caso do nosso array, 20.

Apos essa operacao, realizamos uma chamada de **calloc**, para alocar memoria no heap de acordo com o tamanho do array de "bolinhas", criando o espaco necessario para a matriz.

!!! Aviso
Tem algumas diferencas no uso de *malloc* e *calloc*. Pesquise a respeito.
!!!

Fiz ate aqui rei Arthur e compadre Tkacz. Tentem continuar falando passo a passo do codigo.

```c
#include <stdio.h>
#include <stdlib.h>

void gravity_sort(int *a, int len){
    int i, j, max, sum;
    unsigned char *beads;
    #define BEAD(i, j) beads[i * max + j]

    for (i = 1, max = a[0]; i < len; i++)
        if (a[i] > max) max = a[i];
    beads = calloc(1, max * len);
    /* mark the beads */
    for (i = 0; i < len; i++)
        for (j = 0; j < a[i]; j++)
            BEAD(i, j) = 1;
    for (j = 0; j < max; j++) {
        /* count how many beads are on each post */
        for (sum = i = 0; i < len; i++) {
            sum += BEAD(i, j);
            BEAD(i, j) = 0;
        }
        /* mark bottom sum beads */
        for (i = len - sum; i < len; i++) BEAD(i, j) = 1;
    }
    for (i = 0; i < len; i++) {
        for (j = 0; j < max && BEAD(i, j); j++);
        a[i] = j;
    }
    free(beads);
}
int main()
{
    int i, x[] = {5, 3, 13, 7, 4, 11, 20, 17};
    int len = sizeof(x)/sizeof(x[0]);

    gravity_sort(x, len);
    
    for (i = 0; i < len; i++){
        printf("%d\n", x[i]);
    }
    return 0;
}
```



A partir desse monstro acima, podemos tirar algumas conclusoes a respeito do codigo necessario para a implementacao do gravity sort na linguagem C.


Você também pode criar


1. listas;
2. ordenadas,

assim como

* listas;
* não-ordenadas

e imagens. Lembre que todas as imagens devem estar em uma subpasta img.

![](C:\Users\Dual Stream\Downloads\vambora\vambora\src\logo.png)

Para tabelas, usa-se a [notação do
MultiMarkdown](https://fletcher.github.io/MultiMarkdown-6/syntax/tables.html),
que é muito flexível. Vale a pena abrir esse link para saber todas as
possibilidades.


| coluna a | coluna b |
| -------- | -------- |
| 1        | 2        |

Ao longo de um texto, você pode usar itálico, *negrito*, {red}(vermelho) e
[[tecla]]. Também pode usar uma equação LaTeX: $f(n) \leq g(n)$. Se for muito
grande, você pode isolá-la em um parágrafo.

$$\lim_{n \rightarrow \infty} \frac{f(n)}{g(n)} \leq 1$$

Para inserir uma animação, use `md ;` seguido do nome de uma pasta onde as
imagens estão. Essa pasta também deve estar em img.

;bubble

Você também pode inserir código, inclusive especificando a linguagem.


```py
def f():
print('hello world')
```

```c
void f() {
printf("hello world\n");
}
```

Se não especificar nenhuma, o código fica com colorização de terminal.

```
hello world
```


!!! Aviso
Este é um exemplo de aviso, entre `md !!!`.
!!!


??? Exercício

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???