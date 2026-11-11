# 🏧 Caixa Eletrônico v2.0

Simulador de caixa eletrônico em **C** que roda no terminal. Permite acessar contas, sacar, depositar, transferir e consultar extrato, além de uma área administrativa. Os dados ficam salvos em arquivos de texto, então continuam lá quando o programa é fechado e aberto de novo.

Projeto de estudo que reúne os conteúdos das **aulas 10 a 15**: vetores, matrizes e strings, arquivos, funções e structs, ponteiros, Bubble Sort, recursividade e Quick Sort.

## Funcionalidades

**Cliente**
- Login com número da conta e senha (máximo de 3 tentativas)
- Consulta de saldo
- Saque com entrega de cédulas de R$ 100, 50, 20, 10 e 5, respeitando o estoque do caixa
- Depósito
- Transferência entre contas
- Extrato em ordem cronológica ou ordenado por valor (maior para menor)

**Administração** (senha `9999`)
- Relatório de contas ordenado por saldo
- Estoque de cédulas do caixa
- Cadastro de novas contas
- Reabastecimento de cédulas

## Requisitos

- Um compilador C, como o `gcc` (Linux, macOS, ou MinGW no Windows)

## Como compilar e executar

```bash
gcc -Wall -Wextra -o caixa caixa_eletronico.c
./caixa
```

No Windows (MinGW):

```bash
gcc -o caixa.exe caixa_eletronico.c
caixa.exe
```

Na primeira execução o programa cria sozinho os arquivos `contas.txt`, `extrato.txt` e `cedulas.txt` na pasta onde for executado.

## Contas de teste

| Conta | Senha | Titular         | Saldo inicial |
|-------|-------|-----------------|---------------|
| 1001  | 1234  | Ana Paula Souza | R$ 1000,00    |
| 1002  | 4321  | Bruno Lima      | R$ 2500,50    |
| 1003  | 1111  | Carla Mendes    | R$ 300,00     |

Senha da área administrativa: `9999`.

## Exemplo de uso

```text
=================================
      CAIXA ELETRONICO v2.0
=================================

1 - Acessar conta
2 - Area administrativa
0 - Sair
Opcao: 1
Numero da conta: 1001
Senha: 1234

Bem-vindo(a), Ana Paula Souza!

===== MENU =====
1 - Consultar saldo
2 - Sacar
3 - Depositar
4 - Transferir
5 - Extrato
0 - Sair
Opcao: 2
Saldo atual: R$ 1000.00
Valor do saque: R$ 185

Notas entregues:
1 nota(s) de R$ 100
1 nota(s) de R$ 50
1 nota(s) de R$ 20
1 nota(s) de R$ 10
1 nota(s) de R$ 5
Novo saldo: R$ 815.00
```

## Regras de negócio

- O saque precisa ser em valor inteiro e **múltiplo de 5**.
- O saque é recusado se o saldo for insuficiente ou se o caixa não tiver cédulas para formar o valor.
- Não é possível transferir para a própria conta nem para uma conta que não existe.
- Depois de 3 senhas erradas, o acesso é bloqueado naquela sessão.
- Cada conta nova exige senha de 4 dígitos e começa com saldo zero.

## Estrutura do repositório

```text
.
├── caixa_eletronico.c         # código-fonte do programa
├── tests/
│   └── smoke_test.sh          # testes automáticos
├── .github/
│   └── workflows/
│       └── ci-cd.yml          # compila e testa a cada push e Pull Request
├── .gitignore
├── LICENSE
└── README.md
```

## Testes

Os testes simulam um usuário digitando no terminal e conferem as respostas do programa (login, saque, bloqueio por senha errada, saldo insuficiente, gravação do extrato e relatório do admin).

```bash
gcc -Wall -Wextra -o caixa caixa_eletronico.c
bash tests/smoke_test.sh ./caixa
```

O mesmo teste roda automaticamente no GitHub Actions a cada push e a cada Pull Request para a `main`.

## Conceitos aplicados

| Aula | Conceito | Onde aparece no código |
|------|----------|------------------------|
| 10 | Vetores, matrizes e strings | matriz de cédulas `[2][5]` (valor e estoque), `fgets`, `strlen`, `strcspn` |
| 11 | Arquivos | `fopen`, `fprintf`, `fscanf` nos modos `"r"`, `"w"` e `"a"` |
| 12 | Funções e structs | `struct Conta`, `struct Transacao`, funções de responsabilidade única |
| 13 | Ponteiros | `struct Conta *`, operador `->`, troca por referência |
| 14 | Bubble Sort | `bubble_sort_transacoes` (extrato por valor, com contagem de comparações e trocas) |
| 15 | Recursividade e Quick Sort | `distribuir_notas` (cédulas) e `quick_sort_contas` (relatório por saldo) |

## Limitações

Este é um projeto didático, não um sistema bancário real:

- As senhas ficam em texto puro em `contas.txt`.
- Os valores em dinheiro usam `float`, que pode gerar pequenas imprecisões de centavos.
- O limite é de 50 contas e 500 transações por leitura de extrato.

## Fluxo de contribuição

Mudanças neste repositório seguem a esteira do GitHub:

1. Abrir uma **Issue** descrevendo a mudança.
2. Criar uma **branch** (por exemplo, `feature/nome-da-mudanca`).
3. Abrir um **Pull Request** com `Closes #número` na descrição.
4. Esperar o CI ficar verde e fazer o **merge**.

## Licença

Distribuído sob a licença MIT. Veja o arquivo [LICENSE](LICENSE).
