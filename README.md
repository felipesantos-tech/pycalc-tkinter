# pycalc-tkinter

import tkinter as tki


numero1 = None
operacao = None
digitando_segundo = False

def adicionar(numero):

    global digitando_segundo

    if digitando_segundo:
        entrada.insert(tki.END, numero)
    else:
        entrada.insert(tki.END, numero)




def limpar():

    global numero1
    global operacao
    global digitando_segundo

    numero1 = None
    operacao = None
    digitando_segundo = False

    entrada.delete(0, tki.END)



def apagar():

    valor = entrada.get()

    if valor != "":
        entrada.delete(len(valor) - 1, tki.END)




def escolher_operacao(op):

    global numero1
    global operacao
    global digitando_segundo

    valor = entrada.get()

    if valor == "":
        return

    if operacao is not None:
        return

    numero1 = float(valor)

    operacao = op

    digitando_segundo = True

    entrada.insert(tki.END, op)



def somar():
    escolher_operacao("+")


def subtrair():
    escolher_operacao("-")


def multiplicar():
    escolher_operacao("×")


def dividir():
    escolher_operacao("÷")




def porcentagem():

    valor = entrada.get()

    if valor == "":
        return

    if operacao is not None:

        if operacao in valor:

            partes = valor.split(operacao)

            if len(partes) == 2 and partes[1] != "":

                numero = float(partes[1])

                resultado = numero / 100

                nova_expressao = (
                    partes[0]
                    + operacao
                    + str(resultado)
                )

                entrada.delete(0, tki.END)

                entrada.insert(
                    0,
                    nova_expressao
                )

                return

    numero = float(valor)

    resultado = numero / 100

    entrada.delete(0, tki.END)

    entrada.insert(0, resultado)



def calcular():

    global numero1
    global operacao
    global digitando_segundo

    expressao = entrada.get()

    if expressao == "":
        return

    if operacao is None:
        return

    if operacao == "×":

        partes = expressao.split("×")

    elif operacao == "÷":

        partes = expressao.split("÷")

    elif operacao == "+":

        partes = expressao.split("+")

    elif operacao == "-":

        partes = expressao.split("-")

    else:
        return

    if len(partes) != 2:
        return

    if partes[0] == "" or partes[1] == "":
        return

    numero1 = float(partes[0])

    numero2 = float(partes[1])




    if operacao == "+":

        resultado = numero1 + numero2

    elif operacao == "-":

        resultado = numero1 - numero2

    elif operacao == "×":

        resultado = numero1 * numero2

    elif operacao == "÷":

        if numero2 == 0:

            entrada.delete(0, tki.END)

            entrada.insert(0, "Erro")

            numero1 = None
            operacao = None
            digitando_segundo = False

            return

        resultado = numero1 / numero2


  

    entrada.delete(0, tki.END)

    entrada.insert(0, resultado)

    numero1 = resultado

    operacao = None

    digitando_segundo = False



janela = tki.Tk()

janela.title("Calculadora")

janela.geometry("370x570")

janela.resizable(False, False)

# Fundo da janela
janela.configure(
    bg="#151515"
)




titulo = tki.Label(
    janela,
    text="CALCULADORA",
    font=("Arial", 14, "bold"),
    bg="#151515",
    fg="#FFFFFF"
)

titulo.pack(
    pady=(18, 8)
)



entrada = tki.Entry(
    janela,
    font=("Arial", 28, "bold"),
    justify="right",
    bd=0,
    relief="flat",
    bg="#242424",
    fg="#FFFFFF",
    insertbackground="#FFFFFF"
)

entrada.pack(
    padx=18,
    pady=(5, 18),
    ipady=18,
    fill="x"
)




frame_botoes = tki.Frame(
    janela,
    bg="#151515"
)

frame_botoes.pack(
    padx=15,
    pady=5,
    fill="both",
    expand=True
)




botoes = [

    ("C", 0, 0, limpar, "limpar"),
    ("⌫", 0, 1, apagar, "limpar"),
    ("%", 0, 2, porcentagem, "operador"),
    ("÷", 0, 3, dividir, "operador"),

    ("7", 1, 0, lambda: adicionar("7"), "numero"),
    ("8", 1, 1, lambda: adicionar("8"), "numero"),
    ("9", 1, 2, lambda: adicionar("9"), "numero"),
    ("×", 1, 3, multiplicar, "operador"),

    ("4", 2, 0, lambda: adicionar("4"), "numero"),
    ("5", 2, 1, lambda: adicionar("5"), "numero"),
    ("6", 2, 2, lambda: adicionar("6"), "numero"),
    ("-", 2, 3, subtrair, "operador"),

    ("1", 3, 0, lambda: adicionar("1"), "numero"),
    ("2", 3, 1, lambda: adicionar("2"), "numero"),
    ("3", 3, 2, lambda: adicionar("3"), "numero"),
    ("+", 3, 3, somar, "operador"),

    ("0", 4, 0, lambda: adicionar("0"), "numero"),
    (".", 4, 1, lambda: adicionar("."), "numero"),
    ("=", 4, 2, calcular, "igual")

]




cores = {

    "numero": {
        "bg": "#2B2B2B",
        "fg": "#FFFFFF",
        "active": "#3A3A3A"
    },

    "operador": {
        "bg": "#FF9500",
        "fg": "#FFFFFF",
        "active": "#FFB340"
    },

    "limpar": {
        "bg": "#555555",
        "fg": "#FFFFFF",
        "active": "#707070"
    },

    "igual": {
        "bg": "#34C759",
        "fg": "#FFFFFF",
        "active": "#52D66F"
    }

}



for texto, linha, coluna, comando, tipo in botoes:

    botao = tki.Button(

        frame_botoes,

        text=texto,

        font=(
            "Arial",
            18,
            "bold"
        ),

        command=comando,

        bg=cores[tipo]["bg"],

        fg=cores[tipo]["fg"],

        activebackground=cores[tipo]["active"],

        activeforeground="#FFFFFF",

        bd=0,

        relief="flat",

        cursor="hand2"

    )

    botao.grid(

        row=linha,

        column=coluna,

        padx=5,

        pady=5,

        sticky="nsew"

    )




for i in range(5):

    frame_botoes.rowconfigure(
        i,
        weight=1
    )




for i in range(4):

    frame_botoes.columnconfigure(
        i,
        weight=1
    )




janela.mainloop()
