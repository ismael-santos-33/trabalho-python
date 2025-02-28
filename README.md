
from datetime import datetime

class SistemaGerenciamentoEventos:
    def __init__(self):
        self.eventos = []  # Lista para armazenar os eventos
        self.inscricoes = {}  # Dicionário para armazenar inscrições por evento

    def adicionar_evento(self, nome, data, descricao, max_participantes):
        try:
            data_formatada = datetime.strptime(data, "%d/%m/%Y").strftime("%d/%m/%Y")
        except ValueError:
            print("Formato de data inválido. Use DD/MM/AAAA.")
            return

        evento = {
            "nome": nome,
            "data": data_formatada,
            "descricao": descricao,
            "max_participantes": max_participantes,
            "inscritos": 0
        }
        self.eventos.append(evento)
        self.inscricoes[nome] = []
        print(f"Evento '{nome}' adicionado com sucesso!")

    def atualizar_evento(self, nome, **kwargs):
        for evento in self.eventos:
            if evento["nome"] == nome:
                for chave, valor in kwargs.items():
                    if chave == "data":
                        if valor:
                            try:
                                valor = datetime.strptime(valor, "%d/%m/%Y").strftime("%d/%m/%Y")
                            except ValueError:
                                print("Formato de data inválido. Use DD/MM/AAAA.")
                                continue
                    if chave == "max_participantes":
                        try:
                            valor = int(valor)
                        except ValueError:
                            print("O número máximo de participantes deve ser um número inteiro.")
                            continue
                    if chave in evento:
                        evento[chave] = valor
                print(f"Evento '{nome}' atualizado com sucesso!")
                return
        print(f"Evento '{nome}' não encontrado.")

    def visualizar_eventos(self):
        if not self.eventos:
            print("Nenhum evento disponível.")
            return
        print("Eventos Disponíveis:")
        for evento in self.eventos:
            vagas_restantes = evento["max_participantes"] - evento["inscritos"]
            print(f"- {evento['nome']} (Data: {evento['data']}, Vagas restantes: {vagas_restantes})")

    def inscrever_participante(self, nome_evento, nome_participante):
        for evento in self.eventos:
            if evento["nome"] == nome_evento:
                if evento["inscritos"] < evento["max_participantes"]:
                    self.inscricoes[nome_evento].append(nome_participante)
                    evento["inscritos"] += 1
                    print(f"{nome_participante} inscrito com sucesso no evento '{nome_evento}'.")
                else:
                    print(f"O evento '{nome_evento}' já está com as vagas preenchidas.")
                return
        print(f"Evento '{nome_evento}' não encontrado.")

    def visualizar_inscricoes(self, nome_evento):
        if nome_evento in self.inscricoes:
            participantes = self.inscricoes[nome_evento]
            if participantes:
                print(f"Inscritos no evento '{nome_evento}':")
                for participante in participantes:
                    print(f"- {participante}")
            else:
                print(f"Nenhum inscrito no evento '{nome_evento}'.")
        else:
            print(f"Evento '{nome_evento}' não encontrado.")

    def excluir_participante(self, nome_evento, nome_participante):
        if nome_evento in self.inscricoes:
            if nome_participante in self.inscricoes[nome_evento]:
                self.inscricoes[nome_evento].remove(nome_participante)
                for evento in self.eventos:
                    if evento["nome"] == nome_evento:
                        evento["inscritos"] -= 1
                        print(f"{nome_participante} foi removido com sucesso do evento '{nome_evento}'.")
                        return
            else:
                print(f"Participante '{nome_participante}' não encontrado no evento '{nome_evento}'.")
        else:
            print(f"Evento '{nome_evento}' não encontrado.")

    def excluir_evento(self, nome):
        for evento in self.eventos:
            if evento["nome"] == nome:
                self.eventos.remove(evento)
                del self.inscricoes[nome]
                print(f"Evento '{nome}' excluído com sucesso.")
                return
        print(f"Evento '{nome}' não encontrado.")

    def menu_interativo(self):
        while True:
            print("\nMenu do Sistema de Gerenciamento de Eventos")
            print("1. Adicionar Evento")
            print("2. Atualizar Evento")
            print("3. Visualizar Eventos")
            print("4. Inscrever Participante")
            print("5. Visualizar Inscrições")
            print("6. Excluir Evento")
            print("7. Excluir Participante")
            print("8. Sair")

            escolha = input("Escolha uma opção: ")

            if escolha == "1":
                nome = input("Nome do evento: ")
                data = input("Data do evento (DD/MM/AAAA): ")
                descricao = input("Descrição do evento: ")
                max_participantes = int(input("Número máximo de participantes: "))
                self.adicionar_evento(nome, data, descricao, max_participantes)

            elif escolha == "2":
                nome = input("Nome do evento a ser atualizado: ")
                data = input("Nova data (DD/MM/AAAA) [pressione Enter para manter]: ")
                descricao = input("Nova descrição (pressione Enter para manter): ")
                max_participantes = input("Novo número máximo de participantes (pressione Enter para manter): ")

                atualizacoes = {}
                if data:
                    atualizacoes["data"] = data
                if descricao:
                    atualizacoes["descricao"] = descricao
                if max_participantes:
                    atualizacoes["max_participantes"] = max_participantes

                self.atualizar_evento(nome, **atualizacoes)

            elif escolha == "3":
                self.visualizar_eventos()

            elif escolha == "4":
                nome_evento = input("Nome do evento: ")
                nome_participante = input("Nome do participante: ")
                self.inscrever_participante(nome_evento, nome_participante)

            elif escolha == "5":
                nome_evento = input("Nome do evento: ")
                self.visualizar_inscricoes(nome_evento)

            elif escolha == "6":
                nome = input("Nome do evento a ser excluído: ")
                self.excluir_evento(nome)

            elif escolha == "7":
                nome_evento = input("Nome do evento: ")
                nome_participante = input("Nome do participante a ser excluído: ")
                self.excluir_participante(nome_evento, nome_participante)

            elif escolha == "8":
                print("Saindo do sistema. Até logo!")
                break

            else:
                print("Opção inválida. Tente novamente.")

if __name__ == "__main__":
    sistema = SistemaGerenciamentoEventos()
    sistema.menu_interativo()
  
