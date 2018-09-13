# aulaclass Livro:
    import datetime
    def __init__(self, titulo, autor, editora, edicao, ano_publicacao, isbn):
        self.titulo = titulo
        self.autor = autor
        self.editora = editora
        self.edicao = edicao
        self.ano_publicacao = ano_publicacao
        self.isbn = isbn
        self.status = True
        self.data_devolucao = 0
        self.data_hoje = datetime.date.today()
        self.data_devolvendo = 0
        self.valor_multa = 0


    def esta_disponivel(self): #Verifica se o livro esta disponivel
        return self.status

    def consultar_data_devolucao(self): #Verifica a data de devolução
        return self.data_devolucao

    def dia_devolucao(self): #Transforma a data atual em ordinal e soma com os 5 dias uteis(sete dias) e retorna a data de devolucao
        self.data_hoje.toordinal()
        self.data_devolucao = datetime.date.fromordinal(self.data_hoje.toordinal() + 7)
        return self.data_devolucao

    def emprestar(self): #Empresta o livro verificando se ele está disponivel
        if self.esta_disponivel():
            self.data_devolucao = self.dia_devolucao()
            self.nota_emprestimo()
            self.status = False
        else:
            print("O status do livro é incompatível")

    def devolver(self, data_devolvendo): #Devolve o livro passando uma data de devolução

        self.status = True
        self.verificar_data_devolucao(data_devolvendo)
        self.nota_devolucao()

    def verificar_data_devolucao(self, data_devolvendo): #Verifica se a data de devolução é igual a data dos 5 dias úteis
        self.data_devolvendo = data_devolvendo
        diferença = self.data_devolvendo - self.dia_devolucao()

        if diferença.days != self.dia_devolucao(): #Se as datas forem diferentes ele paga a multa
            if diferença.days > 0:
                valor = diferença.days * 5
                self.valor_multa = valor
            else:
                self.valor_multa = 0

    def nota_emprestimo(self): #Imprime as informações do emprestimo
        print(f'Livro {self.titulo} foi emprestado')
        print(f'Data de empréstimo: {self.data_hoje}')
        print(f'Data de devolução: {self.consultar_data_devolucao()}')

    def nota_devolucao(self): #Imprime as informações da devolução
        print(f'O livro {self.titulo} foi devolvido.')
        print(f'Data prevista de devolução: {self.consultar_data_devolucao()}')
        print(f'Data de devolução: {self.data_devolvendo}')
        print('Valor da multa: ${:.2f}'.format(self.valor_multa))


#Sistema da biblioteca

import datetime
livro = Livro('AAAA', 'BBB', 'CCC', 'DDD', 2018, '00000x87')

livro.emprestar()
print(livro.esta_disponivel())
print()
print(livro.consultar_data_devolucao())
livro.devolver(datetime.date(2018, 9, 25))
print(livro.esta_disponivel())
