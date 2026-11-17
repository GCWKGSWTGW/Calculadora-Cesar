<h1 align="center">🧮 Calculadora no Visor do Cesar</h1>
<p align="center"><b>Trabalho avaliativo do laboratório Biblioteca Cesar: operações + − × ÷ como sub-rotinas, com teclado e visor</b></p>

<p align="center">
  <img alt="Arquitetura" src="https://img.shields.io/badge/arquitetura-Cesar%2016%20bits-1F3A5F?style=for-the-badge">
  <img alt="Montador" src="https://img.shields.io/badge/montador-Daedalus-2A7F8E?style=for-the-badge">
  <img alt="Resultado" src="https://img.shields.io/badge/resultado-32%20bits-6B7280?style=for-the-badge">
  <img alt="Operações" src="https://img.shields.io/badge/opera%C3%A7%C3%B5es-%2B%20%E2%88%92%20%C3%97%20%C3%B7-success?style=for-the-badge">
</p>

<p align="center">
  <b>Gabriel Cabral Wallendzu</b> · HT3080251<br>
  Sistemas de Computação 1 de Automação · Prof. Carlos Eduardo Pagani<br>
  IFSP – Câmpus Hortolândia · 2026
</p>

---

## 📑 Sumário

[Sobre](#-sobre-o-projeto) · [Como usar](#%EF%B8%8F-como-usar) · [Organização](#-organização-do-programa) · [Como executar](#-como-executar) · [Testes](#-testes-realizados) · [Tarefas](#-divisão-das-tarefas) · [Estrutura](#-estrutura-do-repositório)

---

## 🎯 Sobre o projeto

Calculadora para o computador **Cesar**, escrita em linguagem de montagem com o **Daedalus**. A conta é digitada no teclado do simulador e aparece no visor de 36 caracteres, junto com o resultado.

> **Enunciado:** implementar uma calculadora no display do Cesar com entrada das operações `+ - * /`, que devem corresponder aos valores numéricos ASCII, e cujas funções devem ser implementadas como sub-rotina.

O operador digitado é identificado pelo **código ASCII** (`+` = 43, `-` = 45, `*` = 42, `/` = 47) e cada operação é uma **sub-rotina** (`SOMA`, `SUBTRAI`, `MULTIPLICA`, `DIVIDE`).

> 📄 O relatório completo (explicação, código comentado, prints e testes) acompanha este repositório.

---

## ⌨️ Como usar

Digite **número · operador · número · `=`**. Cada número tem até 4 dígitos (0 a 9999).

| Digitado | Visor | Observação |
|---|---|---|
| `12+34=` | `12+34=46` | Soma |
| `100-250=` | `100-250=-150` | Resultado negativo |
| `9999*9999=` | `9999*9999=99980001` | Resultado de 32 bits |
| `20/6=` | `20/6=3` | Quociente inteiro |
| `5/0=` | `5/0=ERRO` | Divisão por zero |

| Tecla | Efeito |
|---|---|
| `0` a `9` | Acrescenta um dígito (máximo 4 por número) |
| `+` `-` `*` `/` | Define a operação (depois de pelo menos um dígito) |
| `=` | Calcula e mostra o resultado |
| `C` ou `c` | Limpa o visor e recomeça, a qualquer momento |
| Outras | Ignoradas |

Depois do resultado, o programa espera uma tecla e então começa uma nova conta.

> 💡 Digite apenas o `=` e aguarde: o resultado aparece sozinho, sem precisar de Enter.

---

## 🧩 Organização do programa

### Sub-rotinas

| Sub-rotina | Função |
|---|---|
| `GETCH` | Espera uma tecla e devolve o código ASCII em `R0` |
| `ECHO` | Escreve o caractere de `R0` no visor e avança o ponteiro |
| `SOMA` · `SUBTRAI` · `MULTIPLICA` · `DIVIDE` | As quatro operações |
| `ESCREVE_RESULTADO` | Trata o status e escreve o sinal, o número ou `ERRO` |
| `ESCREVE_NUM32` | Converte um número de 32 bits para decimal (duas divisões por 10 em cada passo) |
| `STR2INT` | Converte a string de dígitos em inteiro |
| `_S_LIMPA_VISOR_RAPIDA` · `_S_MULTIPLICAR` · `_S_DIVIDIR` | Sub-rotinas da BibCesar |

### Interface das operações

| Registrador | Papel |
|---|---|
| `R0` | Entrada: 1º operando · Saída: parte alta do resultado |
| `R1` | Entrada: 2º operando · Saída: parte baixa do resultado |
| `R2` | Saída: status — `0` positivo · `1` negativo (resultado em módulo) · `2` erro (divisão por zero) |

### Fluxo de uma conta

```
NOVA_CONTA ─► LE_TECLA ─► dígito  ─► guarda no buffer e mostra no visor
                  │       operador ─► guarda o código ASCII, passa ao 2º número
                  │       '='      ─► STR2INT (x2) ─► operação ─► ESCREVE_RESULTADO
                  │       'C'      ─► NOVA_CONTA
                  └─────► outras   ─► ignoradas
```

### Mapa de memória

| Região | Endereços | Uso |
|---|---|---|
| 🟦 Programa | `0` a `1023` | Código da calculadora e da BibCesar |
| 🟩 Dados | `1536` em diante | Variáveis e buffers dos dígitos |
| 🟨 Teclado | `65498` e `65499` | Estado e código ASCII da tecla |
| 🟥 Visor | `65500` a `65535` | 36 caracteres |

---

## ▶️ Como executar

1. 🛠️ Abra o **Daedalus** e escolha **Máquina Alvo = Cesar**.
2. 📂 Abra o arquivo `.ced` da pasta [`Calculadora`](Calculadora).
3. 🔨 Clique em **montar** (gera `.mem` e `.map`).
4. 🖥️ Abra o simulador **Cesar**, carregue o programa e execute.
5. ⌨️ Digite uma conta no teclado do simulador (por exemplo `12+34=`).

---

## ✅ Testes realizados

| Conta | Visor | Status |
|---|---|---|
| `12+34=` | `12+34=46` | ✅ |
| `100-250=` | `100-250=-150` | ✅ |
| `9999*9999=` | `9999*9999=99980001` | ✅ |

---

## 📋 Divisão das tarefas

Trabalho **individual**: todas as tarefas foram realizadas por Gabriel Cabral Wallendzu.

| 📝 A fazer | 🚧 Em andamento | ✅ Concluído |
|---|---|---|
| — | — | Especificação e projeto da calculadora |
| — | — | Leitura e interpretação das teclas |
| — | — | Sub-rotinas das quatro operações |
| — | — | Exibição do resultado de 32 bits |
| — | — | Testes no simulador |
| — | — | Repositório e relatório |

---

## 🗂️ Estrutura do repositório

```
.
├── Calculadora/        código-fonte (.ced), .mem e .map
├── Relatorio_Calculadora_Cesar
└── README.md
```

| Extensão | O que é |
|---|---|
| `.ced` | Código-fonte do Daedalus para o Cesar |
| `.mem` | Imagem da memória gerada na montagem |
| `.map` | Mapa de rótulos e endereços |

---

<p align="center"><sub>Gabriel Cabral Wallendzu · HT3080251 · IFSP Hortolândia · 2026</sub></p>
