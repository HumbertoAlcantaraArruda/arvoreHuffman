# Árvore de Huffman

## 1. A ideia

A codificação de Huffman é uma forma de **compressão sem perda**. Em vez de dar o mesmo número de bits a todos os símbolos, ela dá:

- **códigos curtos** aos símbolos que aparecem **muito**;
- **códigos longos** aos símbolos que aparecem **pouco**.

O resultado é que o texto inteiro, somando os bits, fica menor.

O "símbolo" pode ser um caractere ou, como neste trabalho, uma **palavra inteira** ou um **espaço em branco**.

## 2. A propriedade que faz tudo funcionar: códigos de prefixo

Nenhum código é o começo (prefixo) de outro código. Por exemplo, se `0` é o código do espaço, nenhum outro código pode começar com `0`.

Por isso dá para gravar os bits **todos colados, sem separador**, e mesmo assim decodificar sem ambiguidade: ao ler os bits um a um, assim que a sequência lida bate com um código, ele com certeza é aquele símbolo.

A árvore garante essa propriedade de graça: **os símbolos ficam só nas folhas**, e o código de cada um é o caminho da raiz até a folha (esquerda = `0`, direita = `1`). Um caminho até uma folha nunca passa por outra folha, então nenhum código é prefixo de outro.

## 3. Exemplo completo

Frase: **"amar é sonhar sonhar é viver"**

### 3.1 Contar as frequências

Separando a frase em palavras e espaços:

`amar` · ␣ · `é` · ␣ · `sonhar` · ␣ · `sonhar` · ␣ · `é` · ␣ · `viver`

| Símbolo | Palavra  | Frequência |
|:-------:|----------|:----------:|
| 0       | amar     | 1          |
| 1       | (espaço) | 5          |
| 2       | é        | 2          |
| 3       | sonhar   | 2          |
| 4       | viver    | 1          |

Total: 11 ocorrências. O símbolo é só um número de identificação. Aqui foi dado pela ordem de aparição.

### 3.2 Montar a lista ordenada

Cada símbolo vira um **nó folha** (com a sua frequência). Esses nós são colocados numa **lista encadeada ordenada pela frequência**, da menor para a maior:

```
amar(1) → viver(1) → é(2) → sonhar(2) → espaço(5)
```

### 3.3 Juntar os dois menores, repetidamente

Enquanto a lista tiver mais de um nó:

1. Retire os **dois primeiros** nós (os de menor frequência).
2. Crie um **nó novo** cuja frequência é a soma dos dois. O primeiro retirado vira filho da esquerda e o segundo, filho da direita.
3. Insira o nó novo **de volta na lista, na posição ordenada**.

| Passo | Retirados             | Nó criado | Lista depois                         |
|:-----:|-----------------------|-----------|--------------------------------------|
| 1     | amar(1) + viver(1)    | A(2)      | é(2) → sonhar(2) → A(2) → espaço(5)  |
| 2     | é(2) + sonhar(2)      | B(4)      | A(2) → B(4) → espaço(5)              |
| 3     | A(2) + B(4)           | C(6)      | espaço(5) → C(6)                     |
| 4     | espaço(5) + C(6)      | raiz(11)  | raiz(11)                             |

Quando sobra **um único nó**, ele é a **raiz da árvore**.

> Empates: no passo 1, o nó A(2) foi inserido **depois** dos outros nós de frequência 2. Outra regra de desempate gera uma árvore diferente, mas igualmente ótima. O importante é que o decodificador use **os mesmos códigos** que o codificador usou.

### 3.4 A árvore

```
                 (11)
               0/    \1
        espaço(5)    C(6)
                   0/    \1
                 A(2)      B(4)
                0/  \1    0/  \1
            amar(1) viver(1) é(2) sonhar(2)
```

Os nós internos (A, B, C, raiz) **não têm símbolo**, só frequência. Os símbolos ficam nas folhas.

### 3.5 Extrair os códigos

Percorra a árvore da raiz até cada folha, anotando `0` ao descer pela esquerda e `1` ao descer pela direita:

| Símbolo | Palavra  | Frequência | Código |
|:-------:|----------|:----------:|:------:|
| 1       | (espaço) | 5          | `0`    |
| 0       | amar     | 1          | `100`  |
| 4       | viver    | 1          | `101`  |
| 2       | é        | 2          | `110`  |
| 3       | sonhar   | 2          | `111`  |

Repare: o espaço, que é o mais frequente, ganhou o código mais curto.

## 4. Codificar

Troque cada palavra e espaço pelo seu código e cole tudo:

```
amar ␣  é  ␣ sonhar ␣ sonhar ␣  é  ␣ viver
100  0 110 0  111   0  111   0 110 0  101
```

Sequência de bits: `10001100111011101100101` (23 bits).

**Comparação:** com código de tamanho fixo, 5 símbolos precisariam de 3 bits cada, ou seja, 11 × 3 = **33 bits**. Huffman usou **23 bits**.

### Gravando byte a byte

O arquivo só grava bytes inteiros (8 bits), então a sequência é fatiada de 8 em 8:

```
10001100 | 11101110 | 1100101_
```

O último byte ficou incompleto e precisa ser completado com bits extras.

> **Atenção:** se você completar com `0`, o decodificador vai ler esse `0` como mais um **espaço** (porque `0` é o código do espaço). Pense em como o programa de decodificação vai saber **onde a mensagem termina de verdade**.

Sobre montar o byte: uma `union` permite enxergar **a mesma memória de dois jeitos**, como um byte inteiro (para gravar no arquivo) ou como 8 bits separados (para preencher um a um). É por isso que o enunciado sugere `union`.

## 5. Decodificar

Comece na raiz e leia os bits um a um:

- bit `0` → desça para a esquerda;
- bit `1` → desça para a direita;
- chegou numa **folha** → emita o símbolo dela e **volte para a raiz**.

Decodificando o começo, `1 0 0 0 1 1 0 …`:

| Bits lidos | Caminho              | Folha?           |
|------------|----------------------|------------------|
| `1`        | raiz → C             | não              |
| `10`       | C → A                | não              |
| `100`      | A → amar             | **sim**: "amar"  |
| `0`        | raiz → espaço        | **sim**: " "     |
| `1`        | raiz → C             | não              |
| `11`       | C → B                | não              |
| `110`      | B → é                | **sim**: "é"     |

…e assim por diante até o fim dos bits válidos.

### Reconstruindo a árvore só a partir dos códigos

No segundo programa não há frequências: só a tabela com símbolo, palavra e código. Mesmo assim, dá para remontar a árvore:

1. Comece com uma raiz vazia.
2. Para cada código da tabela, ande a partir da raiz seguindo os bits (`0` esquerda, `1` direita). Sempre que o filho não existir, **crie o nó**.
3. No último bit, o nó alcançado é uma **folha**: guarde nela o símbolo.

A árvore resultante tem exatamente o mesmo formato da original, porque o formato da árvore é definido pelos códigos.

## 6. Por que funciona (intuição)

Juntar sempre os dois menos frequentes faz com que eles fiquem **mais fundos** na árvore, isto é, com códigos mais longos. Os mais frequentes são juntados por último e ficam **perto da raiz**, com códigos curtos.

O total de bits é a soma de `frequência × tamanho do código` de cada símbolo:

```
espaço: 5 × 1 =  5
amar:   1 × 3 =  3
viver:  1 × 3 =  3
é:      2 × 3 =  6
sonhar: 2 × 3 =  6
                ----
                 23 bits
```

Huffman provou que esse procedimento guloso produz o **menor total possível** para códigos de prefixo.

## 7. Ligando com o trabalho

| Conceito                             | Item do enunciado (`trabalho.md`)  |
|--------------------------------------|------------------------------------|
| Contar frequências, símbolo, palavra | 1º programa, item b                |
| Lista encadeada ordenada + junções   | 1º programa, item a                |
| Extrair códigos e gravar a tabela    | 1º programa, item b                |
| Mostrar a lista e a árvore           | 1º programa, item c                |
| Codificar e gravar byte a byte       | 1º programa, item d                |
| Reconstruir a árvore e decodificar   | 2º programa                        |
