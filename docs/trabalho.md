# Estruturas de Dados II – 2026 – Semestre 2

**Professor:** Francisco Assis da Silva

## Exercício Prático em dupla: Árvore de Huffman para codificar palavras de uma frase

Você deverá fazer **2 programas separados** (para serem executados separadamente) em arquivos `.cpp` diferentes.

## Primeiro programa

O primeiro programa deverá ter:

### a) Construção da árvore de Huffman

Um algoritmo para construção da árvore de Huffman (utilize uma lista encadeada ordenada no processo de construção da árvore).

A árvore não deverá ter a palavra e sim um símbolo (numérico, um código!). Os espaços em branco da frase também devem ser considerados e terão um símbolo na árvore.

A frequência das palavras e dos espaços deverá ser contada e armazenada na lista do item **b)** antes de montar a árvore.

### b) Lista de registros

Uma lista (de registros) para armazenar:

- O símbolo;
- A palavra;
- A frequência;
- Os códigos de Huffman.

Os dados da lista, com exceção da frequência, deverão ser gravados em arquivo em disco (arquivo binário).

**Exemplo de frase para usar na construção da árvore de Huffman** (ignore a pontuação e o tamanho da caixa dos caracteres!). Utilize frases com palavras repetidas!

> Amar é sonhar, sonhar é viver, viver é curtir, curtir é amar. Cada um terá uma escolha, cada um fará a escolha, cada um escolhe a sua escolha. Levaremos um tempo para crescer, levaremos um tempo para amadurecer, levaremos um tempo para entender, levaremos um tempo para envelhecer, levaremos um tempo para morrer.

### c) Exibição

Após montar a lista (de registros) e a árvore, você deve exibir ambas na tela.

No caso da árvore, deve ser exibida no formato de árvore (vertical ou horizontal).

### d) Codificação de uma frase

Codificação de uma frase e armazenamento do resultado (uma sequência de bits sem espaços, que deverá ser gravada byte a byte!).

Para montar um byte com 8 bits você pode utilizar `union`.

**Observação:** Utilize uma frase diferente da que foi usada na construção da árvore!

## Segundo programa

O segundo programa deverá ter:

### a) Decodificação

Um algoritmo que abra os arquivos binários (registros) e o arquivo da frase codificada, faça a decodificação da frase e mostre na tela.

Para fazer a decodificação:

1. Monte uma árvore binária a partir dos códigos de Huffman armazenados na tabela (arquivo binário).
2. Faça varreduras na árvore seguindo as sequências binárias; assim você chegará nas folhas, onde estão os códigos das palavras.
3. Vá ao arquivo, busque o código e pegue a palavra ou o espaço em branco.
4. Concatene em uma string e, no final, você terá a frase decodificada.
5. Após ter a frase decodificada, mostre na tela.

## Observações gerais

- Não use `scanf()` ou `gets()`.
- Faça as chamadas de funções passando coisas fixas, ou seja, basta rodar os dois programas que eles já apresentam os resultados.
- As frases usadas para a montagem da árvore de Huffman e para codificação devem ser diferentes!
- No seu trabalho use um texto bem grande (use um arquivo texto), maior do que o exemplo, para montar a árvore e uma frase contendo algumas das palavras desse texto para codificar!