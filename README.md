from InquirerPy import prompt

CANCELAR = "Ok! Operação encerrada."

print("Bem-vindo ao sistema de avaliação de alunos!\n")

def materia_aluno ():
    questoes = [
        {
            'type': "list",
            'message': "Escolha a disciplina desejada : ↓ - ↑ ",
            'choices': ["Matemática", "Português","Ciencias", "Geografia", "História", "Inglês"],
            'name': 'materia'
        }
    ]
    return prompt(questoes)['materia']

materia = materia_aluno()

def tentar_novamente():

    perguntas = [
        {
            'type': "list",
            'message': "Deseja tentar novamente? ↓ - ↑ ",
            'choices': ["Sim", "Não"],
            'name': 'escolha'
        }
    ]
    resultado = prompt(perguntas)
        
    return resultado['escolha'] == "Sim"


def quantidade_aluno():
    while True:
        try:
            return int(input('Digite a quantidade de alunos: '))
        except ValueError:
            print('Erro, por favor digite um valor inteiro!')
            if not tentar_novamente():
                print('Ok! Operação Cancelada.')
                return None
                
def nome_aluno():
    while True:
        try:
            nome = input('Nome do aluno(a): ')
            if not nome.isalpha():
                raise ValueError('Digite o nome do aluno, sem números!')
            return nome
        except ValueError as ve:
            print(f'Erro: {ve}')
            if not tentar_novamente():
                print(CANCELAR)
                return None
                
def pegar_notas():
    while True:
        try:
            a = float(input('Nota 1 UND: '))
            b = float(input('Nota 2 UND: '))
            c = float(input('Nota 3 UND: '))
            return a, b, c
        except ValueError:
            print('Digite apenas números!')
            if not tentar_novamente():
                print(CANCELAR)
                return None

def calcular_media(a, b, c):
    return round((a + b + c) / 3, 2)

def imprimir_alunos(aprovados, reprovados, recuperar):
    print('\nLista de alunos aprovados:')
    for aluno in aprovados:
        print(f'{aluno[0].capitalize()} - Média: {aluno[1]:.2f}')
    print('\nLista de alunos reprovados:')
    for aluno in reprovados:
        print(f'{aluno[0].capitalize()} - Média: {aluno[1]:.2f}')
    print('\nLista de alunos em recuperação:')
    for aluno in recuperar:
        print(f'{aluno[0].capitalize()} - Média: {aluno[1]:.2f}')
            
def planilha_nota():
    qtd_alunos = quantidade_aluno()
    if qtd_alunos is None:
        return
    
    aprovados = []
    reprovados = []
    recuperar = []

    for _ in range(qtd_alunos):
        aluno = nome_aluno()    
        if aluno is None:
            return

        notas = pegar_notas()
        if notas is None:
            return

        resultado = calcular_media(*notas)

        if resultado >= 5:
            print(f'{aluno.capitalize()}, passou! Média: {resultado:.2f}')
            aprovados.append((aluno, resultado))
        elif resultado <= 2:
            print(f'{aluno.capitalize()}, perdeu. Média: {resultado:.2f}')
            reprovados.append((aluno, resultado))
        else:
            print(f'{aluno.capitalize()}, recuperação! Média: {resultado:.2f}')
            recuperar.append((aluno, resultado))
            
    print(f'\nSEGUE A PLANILHA ABAIXO:')
    print('='*20)
    print(f'Disciplina :{materia}')
    print(f'Quantidade de alunos aprovados: {len(aprovados)}')
    print(f'Quantidade de alunos reprovados: {len(reprovados)}')
    print(f'Quantidade de alunos em recuperação: {len(recuperar)}')
    imprimir_alunos(aprovados, reprovados, recuperar)

if __name__ == "__main__":
    planilha_nota()
    
#data de ultima atualização: 03/12/2024
