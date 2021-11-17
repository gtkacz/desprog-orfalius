Gravity sort
======



![](..\src\gravitysort.png)



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

  Portanto o primeiro objetivo consiste em obter os valores de **c** e **f** e "construir" o ábaco para, posteriormente, distribuir as contas.

  Obter o valor de **c** é uma tarefa razoávelmente simples, uma vez que este se resume em contar a quantidade de valores a serem distribuídos no ábaco. Um abordagem razóavel é contar quantos elementos estão disponíveis dentro do array inicial:

```c
int abacus_c(int *input, int n){
	int c = 0;
	for (int i = 0; i < n; i++) {
        c ++;
    }
	return n;
}
```

Como em C o tamanho de um array é passado para funções como parâmetro, a tarefa se torna ainda mais simples,  se tornando simplesmente:

```c
int abacus_c(int *input, int n) {
    return n;
}
```

Já o valor de **f** exige um pouco mais de trabalho, pois a quantidade de fileiras deve ser tal que não falte ao distribuir horizontalmente as contas. Uma abordagem razoável seria buscar pelo maior valor do array inicial, garantindo que não falte espaço para os demais valores:

```c
int abacus_f(int *input, int n) {
	int f = 0;
	for (int i = 0; i < n; i++) {
        if (input[i] > f) {
            f = input[i];
		}
    }
	return f;
}
```

??? Pergunta

Em um contexto mais geral, quais as funções básicas poderiam substituir as funções `abacus_c`  e `abacus_f`?

::: Gabarito

Respectivamente as funções `len` e `max`

:::

???

Com ambos os valores disponíveis podemos por fim, contruir o ábaco, inicializando uma matriz $$M_{cf}$$ com zeros:

```c
int c = abacus_c(input, n);
int f = abacus_f(input, n);
int abacus[c][f];
```



Distribuindo Contas
---------

Partindo agora para a segunda parte, devemos distribuir as contas referentes a cada um dos valores do array inicial, de tal forma que a quantidade de contas verticalmente empilhadas corresponda ao valor do a rray inicial. O cabeçalho da função que descreveria esse procedimento está descrita abaixo:

````c
void distribute_beads(int c, int f, int abacus[c][f], int *input) {

}
````

??? Pergunta

Por que na função `distribute_beads` o valor **n** referente ao tamanho do array `input` não é passado como parâmetro da função?

::: Gabarito

Por que **c** = **n**

:::

???

É razoável pensar que, para acessarmos os valores da matriz que representa o ábaco, precisaríamos de dois laços de repetição, um aninhado no outro. Para facilitar e melhor compreender a dinâmica envolvendo a distribuição de contas, vamos implementar cada um dos laços separadamente.

!!! Dica

Para facilitar a visualização da sua matriz conforme for realizando as atividades a seguir, utilize a função a abaixo:

```c
void show_matrix(int c, int f, int abacus[c][f]) {
    for (int i = 0; i < c; i++) {
        for (int j = 0; j < f; j++) {
            printf("%i", abacus[i][j]);
        }
        printf("\n");
    }
}
```

!!!

??? Atividade 1

Desenvolva a função `distribute_beads` de modo que ela acesse cada um dos valores do *array* `input` e exiba seus valores.

::: Gabarito

````c
	int val;
	for (int i = 0; i < c; i++) {
        val = input[i];
        printf("%i\n", val);
    }
````

:::

???

??? Atividade 2

Desenvolva a função `distribute_beads` de modo que ela acesse cada um dos valores do *array* `input` e exiba uma fileira de  ***** referente a cada valor.

::: Gabarito

````c
	int val;
	for (int i = 0; i < c; i++) {
        val = input[i];
        for (int j = 0; i < val; i++) {
            printf("*");
        }
        printf("\n");
    }
````

:::

???

??? Atividade 3

Desenvolva a função `distribute_beads` de modo que ela acesse cada um dos valores do *array* `input` e exiba uma fileira de  `1` referente a cada valor e preencha com `0` até atingir **f**.

::: Gabarito

````c
	int val;
	for (int i = 0; i < c; i++) {
        val = input[i];
        for (int j = 0; j < f; j++) {
            if( j < val) {
                printf("1");
            } else {
                printf("0");
            }
        }
        printf("\n");
    }
````

:::

???

??? Atividade 4

Desenvolva a função `distribute_beads` de modo que ela acesse cada um dos valores do *array* `input` e insira na matriz `abacus` uma fileira de  `1` referente a cada valor e preenchida com `0` até atingir **f**.

::: Gabarito

````c
	int val;
	for (int i = 0; i < c; i++) {
        val = input[i];
        for (int j = 0; j < f; j++) {
            if( j < val) {
                abacus[j][i] = 1;
            } else {
                abacus[j][i] = 0;
            }
        }
        printf("\n");
    }
````

:::

???

Nada muito difícil não é mesmo? Por fim, basta aplicarmos "gravidade" à matriz `abacus` para não só termos o *array* ordenado mas também utilizando apenas uma modificação, o que resulta em complexidade $$O(1)$$.

Mas... como implementamos "gravidade" mesmo?

Imaginando a matriz que representa um ábaco



??? Pergunta

Você conseguiria pensar em uma implementação na qual garantisse a complexidade teórica $$O(1)$$ ?

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???

Sabemos que o primeiro loop é tal que acessa cada um dos valores do array inicial e cada um das colunas da matriz do ábaco, portanto sua implementação é 



```c
for (int i = 0; i < c; i++) {
    int val = input[i]
    for (int j = val; j > 0; j--) {
        abacus[j][i] = 1;
	}
    
}
```



```c
// Online C compiler to run C program online
#include <stdio.h>
#define MAX_SIZE 128

int abacus_c(int *input, int n){
	int c = 0;
	for (int i = 0; i < n; i++) {
        c ++;
    }
	return n;
}

int abacus_f(int *input, int n) {
	int f = 0;
	for (int i = 0; i < n; i++) {
        if (input[i] > f) {
            f = input[i];
		}
    }
	return f;
}

void distribute_beads(int c, int f, int abacus[c][f], int *input) {
	int val;
	for (int i = 0; i < c; i++) {
        val = input[i];
        for (int j = 0; j < f; j++) {
            if( j < val) {
                abacus[i][j] = 1;
            } else {
                abacus[i][j] = 0;
            }
        }
        printf("\n");
    }
}

void show_matrix(int a, int b, int matrix[a][a]) {
    for (int i = 0; i < a; i++) {
        for (int j = 0; j < b; j++) {
            printf("%i", matrix[a][b]);
        }
        printf("\n");
    }
}

void show_array(int n, int *array) {
    for (int i = 0; i < n; i++) {
        printf("%i ", array[n]);
    }
}

void gravity(int c, int f, int abacus[c][f]) {
    for (int j = 0; j < f; j++) {
        for (int i = 1; i < c; i++) {
            int val_previous = abacus[i-1][j];
            int val_current = abacus[i][j];
            if (val_previous == 1 && val_current == 0) {
                abacus[i-1][j] = 0;
                abacus[i][j] = 1;
                i = 0;
            }
        }
    }
}

void count(int c, int f, int abacus[c][f], int *output) {
    int val;
    for (int i = 1; i < c; i++) {
        for (int j = 0; j < f; j++) {
            if (abacus[i][j] == 1) {
                val++;
            }
            if (abacus[i][j] == 0) {
                break;
            }
        }
        
        printf("%i ", val);
        output[i] = val;
    }
}

int main() {
    int n = 7;
    int input[] = {1, 9, 32, 14, 43, 5, 24};
    int output[n];
    int c = abacus_c(input, n);
    int f = abacus_f(input, n);
    int abacus[f][c];
    
    distribute_beads(c, f, abacus, input);
    
    gravity(c, f, abacus);
    
    show_matrix(c, f, abacus);
    
    count(c, f, abacus, output);
    
    show_array(n, output);
    
    return 0;
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
Tem algumas diferenças no uso de *malloc* e *calloc*. Pesquise a respeito.
!!!

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

Complexidade de tempo
---------

As implementações analógica e de hardware podem atingir complexidade de O(n); entretanto, a implementação desse algoritmo tende a ser significativamente mais lenta no software e só pode ser usado para classificar listas de inteiros positivos.

A complexidade do tempo de execução do algoritmo varia de O(1) a O(S), sendo S a soma dos números de entrada, dependendo da perspectiva do usuário. Finalmente, depende de uma das três implementações possíveis do algoritmo.

1. Para O(1): Soltar todos os contas simultaneamente. Essa complexidade não pode ser implementada na prática.
2. Para O(√n): Em um modelo físico realista que usa a gravidade, o tempo que se leva para deixar os contas caírem é proporcional à raiz quadrada da altura máxima, que é proporcional a n.
3. Para O(n): Os contas são movidos uma linha de cada vez. É o caso das soluções analógica e de hardware.
4. Para O(S): Cada conta é movido individualmente. Este é o caso quando a classificação de contas é implementada sem um mecanismo para auxiliar na localização de espaços vazios abaixo dos mesmo. É o caso das implementações de software.

Complexidade de memória
---------

O gravity sort é detentor de recordes quanto ao desperdício de memória. Os custos de memória extra excedem os custos de armazenamento do próprio array. Sua complexidade de memória é O(n^2) em média.
