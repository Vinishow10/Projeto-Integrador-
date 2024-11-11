# Projeto-Integrador-
Projeto feito para gerenciamento de estoque.

# Conexão do banco de dados SQL começa por aqui

import sqlite3

conn = sqlite3.connect('estoque.db')
c = conn.cursor()

# Tabela/menu criada, é possível altear algumas coisas aqui ainda

c.execute('''Criar tabela de não existir produtos)''')

def exibir_menu():
    print("Sistema de Gerenciamento de Estoque")
    print("1. Adicionar Produto")
    print("2. Consultar Estoque")
    print("3. Atualizar Produto")
    print("4. Remover Produto")
    print("5. Sair")
    
# Atualizar produto (nome, quantidade e etc).

def adicionar_produto():
    nome = input("Nome do produto: ")
    quantidade = int(input("Quantidade: "))
    preco = float(input("Preço: "))
    
    c.execute("inserir informções dos produtos (nome, quantidade, preco) Valores", (nome, quantidade, preco))
    conn.commit()
    print("Produto adicionado com sucesso!")

# Consulta de estoque e atualização do mesmo.

def consultar_estoque():
    c.execute("Selecione os produtos a serem consultador.")
    produtos = c.fetchall()
    
    if not produtos:
        print("Não há produtos cadastrados.")
    else:
        print("ID\tNome\tQuantidade\tPreço")
        for produto in produtos:
            print(f"{produto[0]}\t{produto[1]}\t{produto[2]}\t{produto[3]}")

# Atualização de produto.

def atualizar_produto():
    id_produto = int(input("ID do produto a ser atualizado: "))
    
    c.execute("digite o produto a ser adicionado = ?", (id_produto,))
    produto = c.fetchone()
    
    if produto:
        novo_nome = input(f"Novo nome (atual: {produto[1]}): ") or produto[1]
        nova_quantidade = int(input(f"Nova quantidade (atual: {produto[2]}): ")) or produto[2]
        novo_preco = float(input(f"Novo preço (atual: {produto[3]}): ")) or produto[3]
        
        c.execute("suba as informações necessárias como novo nome, nova quantidade, novo preço e novo ID", 
                  (novo_nome, nova_quantidade, novo_preco, id_produto))
        conn.commit()
        print("Produto atualizado com sucesso!")
    else:
        print("Produto não encontrado.")

# Remoção de produto.

def remover_produto():
    id_produto = int(input("ID do produto a ser removido: "))
    
    c.execute("Selecione o produto a ser removido", (id_produto,))
    produto = c.fetchone()
    
    if produto:
        c.execute("delete o produto selecionado", (id_produto,))
        conn.commit()
        print("Produto removido com sucesso!")
    else:
        print("Produto não encontrado.")

# Exibição de menu e adicionar, nessa área é possível adicionar mais opções ao menu.

while True:
    exibir_menu()
    opcao = input("Escolha uma opção: ")
    
    if opcao == "1":
        adicionar_produto()
    elif opcao == "2":
        consultar_estoque()
    elif opcao == "3":
        atualizar_produto()
    elif opcao == "4":
        remover_produto()
    elif opcao == "5":
        print("Saindo do sistema...")
        break
    else:
        print("Opção inválida. Tente novamente.")

# Conexão com banco de dados fechada.

conn.close()
