# projeto-em-python-2-dados-bancarios
Projeto de simulação bancária em Python

def exibir_menu():
    """Exibe o menu principal e retorna a opção escolhida pelo usuário."""
    return input("""
====== MENU PRINCIPAL ======

[1] Depositar
[2] Sacar
[3] Visualizar Extrato
[4] Sair

Escolha uma opção: """)


def depositar(saldo, extrato):
    """Realiza um depósito na conta do usuário."""
    try:
        valor = float(input("Digite o valor para depósito: R$ "))

        if valor > 0:
            saldo += valor
            extrato += f"Depósito: + R$ {valor:.2f}\n"
            print(f"✅ Depósito de R$ {valor:.2f} realizado com sucesso!\n")
        else:
            print("❌ Operação falhou: O valor do depósito deve ser positivo.\n")

    except ValueError:
        print("❌ Entrada inválida. Digite um valor numérico.\n")

    return saldo, extrato


def sacar(saldo, extrato, limite, saques_realizados, limite_saques):
    """Realiza um saque da conta do usuário, respeitando limites diários e de saldo."""
    try:
        valor = float(input("Digite o valor para saque: R$ "))

        excedeu_saldo = valor > saldo
        excedeu_limite = valor > limite
        excedeu_saques = saques_realizados >= limite_saques

        if valor <= 0:
            print("❌ Operação falhou: O valor do saque deve ser positivo.\n")

        elif excedeu_saldo:
            print("❌ Saldo insuficiente para realizar o saque.\n")

        elif excedeu_limite:
            print(f"❌ Valor excede o limite por saque (R$ {limite:.2f}).\n")

        elif excedeu_saques:
            print(f"❌ Limite diário de {limite_saques} saques atingido.\n")

        else:
            saldo -= valor
            extrato += f"Saque:   - R$ {valor:.2f}\n"
            saques_realizados += 1
            print(f"✅ Saque de R$ {valor:.2f} realizado com sucesso!\n")

    except ValueError:
        print("❌ Entrada inválida. Digite um valor numérico.\n")

    return saldo, extrato, saques_realizados


def exibir_extrato(saldo, extrato):
    """Mostra todas as movimentações da conta e o saldo atual."""
    print("\n========== EXTRATO ==========")
    if not extrato:
        print("Nenhuma movimentação registrada.")
    else:
        print(extrato.strip())
    print(f"\nSaldo atual: R$ {saldo:.2f}")
    print("==============================\n")


def main():
    saldo = 0.0
    limite_por_saque = 500.00
    extrato = ""
    saques_realizados = 0
    limite_saques_diario = 3

    print("Bem-vindo(a) ao Sistema Bancário Python!\n")

    while True:
        opcao = exibir_menu()

        if opcao == "1":
            saldo, extrato = depositar(saldo, extrato)

        elif opcao == "2":
            saldo, extrato, saques_realizados = sacar(
                saldo, extrato, limite_por_saque, saques_realizados, limite_saques_diario
            )

        elif opcao == "3":
            exibir_extrato(saldo, extrato)

        elif opcao == "4":
            print("✅ Saindo do sistema. Obrigado por utilizar nosso banco!\n")
            break

        else:
            print("❌ Opção inválida. Por favor, selecione uma opção válida do menu.\n")

# Inicia o programa
main()
