# Árvore de Huffman: codificação de palavras

Trabalho prático da disciplina **Estruturas de Dados II** (2026, 2º semestre), com o professor Francisco Assis da Silva.

O projeto usa uma **árvore de Huffman** para comprimir frases. A diferença em relação ao Huffman clássico é que os símbolos não são caracteres, e sim **palavras inteiras e espaços em branco**. As palavras mais frequentes recebem os códigos binários mais curtos, e a frase codificada é gravada em disco **byte a byte**.

## Como funciona

O trabalho é dividido em **dois programas independentes**:

### 1º programa: codificação (`programa1.cpp`)

1. Lê um texto grande de um arquivo, ignorando pontuação e maiúsculas/minúsculas.
2. Conta a frequência de cada palavra e de cada espaço e dá a cada um um **símbolo numérico**.
3. Monta a árvore de Huffman usando uma **lista encadeada ordenada** pela frequência.
4. Gera o código de Huffman de cada símbolo e guarda tudo numa **lista de registros** (símbolo, palavra, frequência e código).
5. Exibe na tela a lista de registros e a árvore desenhada.
6. Grava a tabela (símbolo, palavra e código) em um **arquivo binário**.
7. Codifica uma frase diferente do texto original e grava a sequência de bits **byte a byte**.

### 2º programa: decodificação (`programa2.cpp`)

1. Lê o arquivo binário com a tabela de códigos e o arquivo com a frase codificada.
2. Reconstrói a árvore de Huffman **só a partir dos códigos**.
3. Percorre a árvore bit a bit até as folhas, recupera as palavras e os espaços e mostra a frase decodificada.

Os dois programas rodam sem nenhuma entrada do usuário: as funções são chamadas com valores fixos.

## Como compilar e executar

O projeto usa o compilador do **Dev-C++** (TDM-GCC 4.7.1) em modo **32 bits**, necessário para a biblioteca `conio2`.

**No Dev-C++:** selecione o compilador *TDM-GCC 4.7.1 32-bit* e compile normalmente (F11).

**Pela linha de comando:**

```
g++ -m32 -std=c++11 -Wall programa1.cpp -o programa1.exe -static-libgcc -lconio
g++ -m32 -std=c++11 -Wall programa2.cpp -o programa2.exe -static-libgcc -lconio
```

Execute o `programa1.exe` primeiro, porque ele gera os arquivos binários que o `programa2.exe` lê.

## Estrutura

```
arvoreHuffman/
├── programa1.cpp          # codificação
├── programa2.cpp          # decodificação
└── docs/
    ├── trabalho.md        # enunciado do trabalho
    ├── arvoreHuffman.md   # explicação da árvore de Huffman com exemplo
    ├── PROJETO.md         # ambiente, regras e andamento do projeto
    └── GUIA.md            # orientações de apoio ao desenvolvimento
```

## Regras de código

O código segue estas restrições da disciplina:

- sem `break` para sair de repetições (permitido apenas em `switch`);
- sem `continue`, `goto` ou `return;` vazio;
- sem variáveis globais;
- sem ponteiro triplo (`***`);
- sem `scanf()` ou `gets()`;
- structs declaradas no formato `struct nome { ... };` seguido de `typedef struct nome Nome;`.

## Status

🚧 Em desenvolvimento. O andamento está em [`docs/PROJETO.md`](docs/PROJETO.md).

## Dupla

- [Humberto Arruda](https://github.com/HumbertoAlcantaraArruda)
- [Nicoly Rampaso](https://github.com/nicolyrampaso)
