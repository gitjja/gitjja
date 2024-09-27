class Livro:
    def __init__(self, titulo, autor, ano, categoria):
        self.titulo = titulo
        self.autor = autor
        self.ano = ano
        self.categoria = categoria
        self.disponivel = True

    def __str__(self):
        status = "Disponível" if self.disponivel else "Indisponível"
        return f"{self.titulo} por {self.autor} ({self.ano}) - {status} [Categoria: {self.categoria}]"


class Usuario:
    def __init__(self, nome):
        self.nome = nome
        self.livros_emprestados = []

    def emprestar(self, livro):
        self.livros_emprestados.append(livro)
        livro.disponivel = False

    def devolver(self, livro):
        self.livros_emprestados.remove(livro)
        livro.disponivel = True

    def __str__(self):
        return self.nome


class HistoricoEmprestimos:
    def __init__(self):
        self.historico = []

    def registrar_emprestimo(self, usuario, livro):
        self.historico.append(f"{usuario.nome} emprestou '{livro.titulo}'.")

    def registrar_devolucao(self, usuario, livro):
        self.historico.append(f"{usuario.nome} devolveu '{livro.titulo}'.")

    def listar_historico(self):
        for registro in self.historico:
            print(registro)


class Biblioteca:
    def __init__(self):
        self.livros = []
        self.usuarios = []
        self.historico = HistoricoEmprestimos()

    def adicionar_livro(self, livro):
        self.livros.append(livro)

    def adicionar_usuario(self, usuario):
        self.usuarios.append(usuario)

    def listar_livros(self):
        for livro in self.livros:
            print(livro)

    def buscar_livro(self, termo):
        resultado = [livro for livro in self.livros if termo.lower() in livro.titulo.lower() or termo.lower() in livro.autor.lower()]
        return resultado

    def emprestar_livro(self, usuario, titulo):
        livro = self.buscar_livro(titulo)
        if livro and livro[0].disponivel:
            usuario.emprestar(livro[0])
            self.historico.registrar_emprestimo(usuario, livro[0])
            print(f"{usuario} emprestou {livro[0].titulo}.")
        else:
            print("Livro não disponível ou não encontrado.")

    def devolver_livro(self, usuario, titulo):
        livro = self.buscar_livro(titulo)
        if livro and livro[0] not in usuario.livros_emprestados:
            print("Este livro não foi emprestado por você.")
        elif livro:
            usuario.devolver(livro[0])
            self.historico.registrar_devolucao(usuario, livro[0])
            print(f"{usuario} devolveu {livro[0].titulo}.")
        else:
            print("Livro não encontrado.")

    def listar_historico(self):
        print("\nHistórico de Empréstimos:")
        self.historico.listar_historico()


def menu():
    biblioteca = Biblioteca()
    
    while True:
        print("\nMenu:")
        print("1. Adicionar Livro")
        print("2. Adicionar Usuário")
        print("3. Listar Livros")
        print("4. Buscar Livro")
        print("5. Emprestar Livro")
        print("6. Devolver Livro")
        print("7. Listar Histórico de Empréstimos")
        print("8. Sair")
        
        opcao = input("Escolha uma opção: ")

        if opcao == '1':
            titulo = input("Título do livro: ")
            autor = input("Autor do livro: ")
            ano = input("Ano de publicação: ")
            categoria = input("Categoria do livro: ")
            livro = Livro(titulo, autor, ano, categoria)
            biblioteca.adicionar_livro(livro)
            print("Livro adicionado com sucesso!")

        elif opcao == '2':
            nome = input("Nome do usuário: ")
            usuario = Usuario(nome)
            biblioteca.adicionar_usuario(usuario)
            print("Usuário adicionado com sucesso!")

        elif opcao == '3':
            print("\nLivros na Biblioteca:")
            biblioteca.listar_livros()

        elif opcao == '4':
            termo = input("Digite o título ou autor do livro a buscar: ")
            resultado = biblioteca.buscar_livro(termo)
            if resultado:
                print("Livros encontrados:")
                for livro in resultado:
                    print(livro)
            else:
                print("Nenhum livro encontrado.")

        elif opcao == '5':
            usuario_nome = input("Nome do usuário: ")
            usuario = next((u for u in biblioteca.usuarios if u.nome == usuario_nome), None)
            if usuario:
                titulo = input("Título do livro a emprestar: ")
                biblioteca.emprestar_livro(usuario, titulo)
            else:
                print("Usuário não encontrado.")

        elif opcao == '6':
            usuario_nome = input("Nome do usuário: ")
            usuario = next((u for u in biblioteca.usuarios if u.nome == usuario_nome), None)
            if usuario:
                titulo = input("Título do livro a devolver: ")
                biblioteca.devolver_livro(usuario, titulo)
            else:
                print("Usuário não encontrado.")

        elif opcao == '7':
            biblioteca.listar_historico()

        elif opcao == '8':
            print("Saindo do sistema...")
            break

        else:
            print("Opção inválida! Tente novamente.")


# Iniciar o menu
if __name__ == "__main__":
    menu()
