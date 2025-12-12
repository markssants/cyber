# 🔒 Desafio de Autenticação: Bloqueio de Conta por Tentativas Inválidas

## 🚀 Visão Geral do Desafio

Em sistemas de segurança digital, é crucial prevenir ataques de força bruta. Este desafio implementa um mecanismo de segurança simples: **bloquear uma conta se houver 3 ou mais tentativas de login falhas consecutivas**.

O objetivo é processar uma lista cronológica de resultados de login e determinar se a conta deve ser bloqueada.

---

## 🎯 Regra de Bloqueio

Uma conta deve ser bloqueada se e somente se houver **três (3) ou mais** ocorrências da string **"falha"** em sequência.

* Uma tentativa de **"sucesso"** quebra a sequência de falhas e **reseta** o contador.

---

## 💻 Implementação em Python

O código utiliza um laço de repetição (`for`) para iterar sobre as tentativas e uma variável (`falhas_consecutivas`) para rastrear a contagem atual.

### 📝 Código

```python
import sys

def verificar_bloqueio(entrada):
    """
    Verifica se uma conta deve ser bloqueada com base em tentativas de login consecutivas.
    """
    # Divide a string de entrada em uma lista de tentativas, garantindo minúsculas e sem espaços
    tentativas = [item.strip().lower() for item in entrada.split(',')]

    # Inicializa o contador de falhas consecutivas
    falhas_consecutivas = 0
    
    # Flag para rastrear o estado de bloqueio
    bloqueada = False

    # Percorre cada tentativa
    for tentativa in tentativas:
        if tentativa == "falha":
            # Incrementa o contador de falhas
            falhas_consecutivas += 1
            
            # Verifica a condição de bloqueio (3 ou mais)
            if falhas_consecutivas >= 3:
                print("Conta Bloqueada")
                bloqueada = True
                break  # Para a verificação imediatamente após o bloqueio
        else:
            # Se for "sucesso", reseta o contador de falhas consecutivas
            falhas_consecutivas = 0  

    # Se o loop terminar sem que a flag de bloqueio tenha sido ativada
    if not bloqueada:
        print("Acesso Normal")

# --- Entrada ---
try:
    # Leitura da entrada (por exemplo: "sucesso, falha, falha, falha")
    entrada = sys.stdin.read().strip()
    if entrada:
        verificar_bloqueio(entrada)
    else:
        # Se nenhuma entrada for fornecida
        print("Acesso Normal")
        
except EOFError:
    # Trata o caso de entrada vazia ou fim de arquivo
    print("Acesso Normal")

🧠 Explicação da Lógica
Pré-processamento da Entrada:

A string de entrada ("sucesso, falha, falha") é transformada em uma lista de strings (['sucesso', 'falha', 'falha']) para facilitar a iteração.

Variáveis de Controle:

falhas_consecutivas: Contador que armazena quantas falhas ocorreram na sequência atual. É resetado para 0 a cada sucesso.

bloqueada: Uma flag booleana (True/False) que indica se a condição de bloqueio foi atingida.

Iteração e Condicionais:

Para cada "falha" encontrada, o contador falhas_consecutivas é aumentado em 1.

Se, após o incremento, o contador for >= 3, o programa imprime "Conta Bloqueada", define a flag bloqueada como True e sai do loop imediatamente (break).

Para qualquer outro resultado (ou seja, "sucesso"), o contador falhas_consecutivas é zerado.

Saída Final:

Após o loop, o programa verifica o estado da flag bloqueada.

Se bloqueada for False (o loop terminou sem atingir 3 falhas seguidas), a saída é "Acesso Normal".

🧪 Exemplos de TesteEntradaFalhas Consecutivas (Max)Saída Esperadasucesso, falha, falha, falha3 (atingido)Conta Bloqueadafalha, falha, sucesso, falha2Acesso Normalfalha, falha, falha, sucesso3 (atingido)Conta Bloqueadasucesso, sucesso, falha, sucesso1Acesso Normalfalha, falha, falha, falha, falha5Conta Bloqueada
