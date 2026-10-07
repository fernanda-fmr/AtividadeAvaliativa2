'''Desenvolva um programa em Python utilizando orientação a objetos para simular o funcionamento de um
carrinho de compras.
Crie uma classe chamada Produto, com atributos privados __nome, __preco __descrição e __quantidade, e
implemente métodos públicos para acessar e modificar esses dados de forma segura (getters e setters).
Em seguida, implemente a classe CarrinhoDeCompras, que armazena uma lista de objetos Produto e oferece
funcionalidades para adicionar produtos, remover produtos pelo nome, calcular o valor total da compra e
exibir os itens do carrinho com suas respectivas informações.
O programa principal deve apresentar um menu interativo no console, permitindo ao usuário realizar
operações como incluir e excluir produtos, visualizar o conteúdo do carrinho e consultar o total da compra.
Utilize boas práticas de encapsulamento e organização orientada a objetos para garantir a integridade dos
dados.
'''



class Produto:
    def __init__(self, nome: str, preco: float, descricao: str, quantidade: int):
        self.__nome = nome
        self.__preco = 0.0
        self.__descricao = descricao
        self.__quantidade = 0

        self.preco = preco
        self.quantidade = quantidade

    @property
    def nome(self) -> str:
        return self.__nome

    @nome.setter
    def nome(self, novo_nome: str):
        if novo_nome.strip():
            self.__nome = novo_nome.strip()
        else:
            raise ValueError("O nome do produto não pode ficar vazio.")

    @property
    def preco(self) -> float:
        return self.__preco

    @preco.setter
    def preco(self, novo_preco: float):
        if novo_preco >= 0:
            self.__preco = float(novo_preco)
        else:
            raise ValueError("O preço do produto não pode ser negativo.")

    @property
    def descricao(self) -> str:
        return self.__descricao

    @descricao.setter
    def descricao(self, nova_descricao: str):
        self.__descricao = nova_descricao.strip()

    @property
    def quantidade(self) -> int:
        return self.__quantidade

    @quantidade.setter
    def quantidade(self, nova_quantidade: int):
        if nova_quantidade >= 0:
            self.__quantidade = int(nova_quantidade)
        else:
            raise ValueError("A quantidade do produto não pode ser negativa.")


    def calcular_subtotal(self) -> float:
        return self.__preco * self.__quantidade

    def __str__(self) -> str:
        return (
            f"Produto: {self.__nome} | Preço: R$ {self.__preco:.2f} | "
            f"Qtd: {self.__quantidade} | Subtotal: R$ {self.calcular_subtotal():.2f}\n"
            f"   Descrição: {self.__descricao}"
        )


class CarrinhoDeCompras:
    def __init__(self):
        self.__itens: list[Produto] = []

    def adicionar_produto(self, produto: Produto) -> None:
        for item in self.__itens:
            if item.nome.lower() == produto.nome.lower():
                item.quantidade += produto.quantidade
                print(f"\nQuantidade do produto '{item.nome}' atualizada com sucesso!")
                return
        
        self.__itens.append(produto)
        print(f"\nProduto '{produto.nome}' adicionado ao carrinho!")

    def remover_produto(self, nome_produto: str) -> bool:
        for item in self.__itens:
            if item.nome.lower() == nome_produto.lower():
                self.__itens.remove(item)
                return True
        return False

    def calcular_total(self) -> float:
        return sum(item.calcular_subtotal() for item in self.__itens)

    def exibir_carrinho(self) -> None:
        if not self.__itens:
            print("\nO carrinho está vazio.")
            return

        print("\n" + "=" * 50)
        print(" ITENS NO CARRINHO ".center(50, "="))
        print("=" * 50)
        for i, item in enumerate(self.__itens, 1):
            print(f"[{i}] {item}")
            print("-" * 50)
        print(f"VALOR TOTAL DA COMPRA: R$ {self.calcular_total():.2f}")
        print("=" * 50)


def menu():
    carrinho = CarrinhoDeCompras()

    while True:
        print("\n--- MENU DO CARRINHO DE COMPRAS ---")
        print("1. Adicionar produto")
        print("2. Remover produto")
        print("3. Visualizar carrinho")
        print("4. Consultar total da compra")
        print("5. Sair")

        opcao = input("Escolha uma opção: ").strip()

        if opcao == "1":
            try:
                nome = input("Nome do produto: ").strip()
                preco = float(input("Preço unitário (R$): "))
                descricao = input("Descrição do produto: ").strip()
                quantidade = int(input("Quantidade: "))

                novo_produto = Produto(nome, preco, descricao, quantidade)
                carrinho.adicionar_produto(novo_produto)
            except ValueError as e:
                print(f"\nErro de entrada: {e}. Digite um número válido. ")

        elif opcao == "2":
            nome_remover = input("Digite o nome do produto a remover: ").strip()
            removido = carrinho.remover_produto(nome_remover)
            if removido:
                print(f"\nProduto '{nome_remover}' removido com sucesso!")
            else:
                print(f"\nProduto '{nome_remover}' não encontrado no carrinho.")

        elif opcao == "3":
            carrinho.exibir_carrinho()

        elif opcao == "4":
            total = carrinho.calcular_total()
            print(f"\nO valor total da compra é: R$ {total:.2f}")

        elif opcao == "5":
            print("Saindo...")
            break

        else:
            print("\nOpção inválida! Tente novamente.")


if __name__ == "__main__":
    menu()
