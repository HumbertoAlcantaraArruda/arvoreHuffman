# Projeto: Árvore de Huffman

Registro de tudo o que foi definido sobre o projeto até agora: objetivo, ambiente, regras e andamento.

## 1. Objetivo

Exercício prático em dupla da disciplina **Estruturas de Dados II (2026, 2º semestre)**: usar uma árvore de Huffman para codificar **palavras** de uma frase. O enunciado completo está em [`trabalho.md`](trabalho.md).

São **dois programas separados**, em arquivos `.cpp` diferentes:

| Programa      | Arquivo         | Responsabilidade                                                                 |
|---------------|-----------------|----------------------------------------------------------------------------------|
| 1º (codifica) | `programa1.cpp` | Conta frequências, monta a lista de registros e a árvore, exibe ambas, grava a tabela em binário e codifica uma frase byte a byte |
| 2º (decodifica) | `programa2.cpp` | Lê a tabela binária e a frase codificada, reconstrói a árvore pelos códigos e mostra a frase decodificada |

### Requisitos principais

- A árvore guarda um **símbolo numérico**, não a palavra. Os **espaços em branco** também são símbolos.
- A construção da árvore usa uma **lista encadeada ordenada** pela frequência.
- A frequência é contada **antes** de montar a árvore e guardada na lista de registros.
- **Lista de registros:** símbolo, palavra, frequência e código de Huffman. Tudo menos a frequência é gravado em **arquivo binário**.
- Ignorar **pontuação** e **maiúsculas/minúsculas**.
- Exibir a lista e a árvore na tela, a árvore em formato de árvore (vertical ou horizontal).
- A frase codificada é uma sequência de bits sem espaços, gravada **byte a byte** (o enunciado sugere `union` para montar o byte).
- O texto que monta a árvore deve ser **grande e vir de um arquivo texto**. A frase codificada deve ser **diferente** dele, usando algumas de suas palavras.
- **Não usar `scanf()` nem `gets()`.** Os programas chamam as funções com valores fixos: basta executá-los para ver os resultados.

## 2. Ambiente

### Compilador

- Compilador do Dev-C++: **TDM-GCC 4.7.1**, em `C:\devcpp\Dev-Cpp\MinGW64\bin`.
- O Dev-C++ está configurado com o perfil **"TDM-GCC 4.7.1 32-bit Debug"**. O VS Code foi configurado igual.
- Comando equivalente:

  ```
  g++ -m32 -g -std=c++11 -Wall arquivo.cpp -o arquivo.exe -static-libgcc -lconio
  ```

- O **`-m32` é obrigatório**: a biblioteca `libconio.a` (do `conio2.h`) só existe em versão 32 bits.
- GCC 4.7.1 suporta no máximo **C++11 / C11**.
- `conio2.h` disponível: `textcolor`, `gotoxy`, `clrscr` etc.

### VS Code

- Extensão **C/C++ (ms-vscode.cpptools)** instalada.
- Pasta `.vscode/` (fora do Git, ignorada no `.gitignore`):
  - `c_cpp_properties.json`: IntelliSense com o `g++` do Dev-C++, `-m32`, modo `windows-gcc-x86`, C11/C++11.
  - `tasks.json`: compilação padrão (**Ctrl+Shift+B**) do arquivo aberto com os argumentos acima.
  - `launch.json`: depuração (**F5**) com o `gdb32.exe` do Dev-C++, em console externo.
- O `settings.json` do usuário também aponta o `g++` do Dev-C++ como compilador padrão da extensão.

## 3. Regras de código

Obrigatórias em todo o código do projeto:

- **Sem `break` em repetições:** a saída do laço vem da própria condição. O `break` é permitido **dentro de `switch`**.
- **Sem `continue;`.**
- **Sem `goto;`.**
- **Sem `return;` vazio:** não sair antes do fim de uma função `void`. O `return valor;` é permitido.
- **Sem variáveis globais:** os dados são passados por parâmetro.
- **Sem ponteiro triplo** (`***`).
- **Structs sempre neste formato:**

  ```c
  struct estrutura {
      //...
  };
  typedef struct estrutura Estrutura;
  ```

## 4. Como o Claude atua

Definido em [`GUIA.md`](GUIA.md). Em resumo: o **aluno escreve o código** e o Claude **orienta**. O Claude explica conceitos, faz perguntas, sugere o próximo passo e revisa o código apontando arquivo, linha e motivo, sem escrever código nem corrigir os arquivos. Só mostra código se o aluno pedir explicitamente.

## 5. Commits

- **O único autor é o aluno.** Nenhum coautor ou atribuição ao Claude (`Co-Authored-By`, `Generated with Claude Code` etc.).
- Mensagens em **português brasileiro**, no padrão **Conventional Commits**: `<tipo>(<escopo>): <descrição>`.
- Tipos: `feat`, `fix`, `refactor`, `docs`, `style`, `test`, `perf`, `build`, `ci`, `chore`.
- Verbo no presente (implementa, corrige, adiciona…), descrição em minúsculas, sem ponto final, sem mensagens genéricas.
- O Claude analisa o `git diff`/`git status`, sugere a mensagem e **só faz o commit depois da aprovação**.

## 6. Documentação

| Arquivo                                  | Conteúdo                                                          |
|------------------------------------------|-------------------------------------------------------------------|
| [`trabalho.md`](trabalho.md)             | Enunciado do trabalho                                             |
| [`arvoreHuffman.md`](arvoreHuffman.md)   | Como funciona a árvore de Huffman, com um exemplo completo        |
| [`GUIA.md`](GUIA.md)                     | Como o Claude deve atuar (orientador, sem escrever código)        |
| `PROJETO.md`                             | Este arquivo                                                      |

## 7. Andamento

### Situação atual

- `programa1.cpp`: só um teste do ambiente (`conio2` com `textcolor` + `printf`). A lógica do trabalho ainda não começou.
- `programa2.cpp`: vazio.

### Roteiro sugerido

- [ ] **Passo 0:** resolver no papel um exemplo pequeno ("amar é sonhar sonhar é viver"). Já está resolvido em `arvoreHuffman.md`.
- [ ] **Passo 1:** definir a struct do registro (símbolo, palavra, frequência, código) e a lista de registros. **Próximo passo.** Perguntas a responder:
  - Qual o tipo de cada campo?
  - Como guardar um código de tamanho variável sabendo que ele vai para um arquivo binário?
  - A lista vai ser um vetor ou uma lista encadeada?
- [ ] Separar o texto em palavras e espaços (ignorando pontuação e caixa) e contar as frequências.
- [ ] Montar a lista encadeada ordenada de nós e construir a árvore.
- [ ] Extrair os códigos e preencher a lista de registros.
- [ ] Exibir a lista e a árvore.
- [ ] Gravar a tabela em arquivo binário.
- [ ] Codificar uma frase diferente e gravá-la byte a byte.
- [ ] 2º programa: ler os arquivos, reconstruir a árvore pelos códigos e decodificar.

### Questões em aberto

- **Fim da mensagem codificada:** o último byte é completado com bits extras. Se o código de algum símbolo for só `0`s (no exemplo, o espaço é `0`), completar com zeros gera símbolos a mais na decodificação. É preciso decidir como o 2º programa vai saber onde a mensagem termina.
- **Acentos:** o texto tem palavras acentuadas ("é", "terá"…). É preciso ver a codificação do arquivo de texto (UTF-8 ou ANSI) e como ignorar a caixa nessas letras.
