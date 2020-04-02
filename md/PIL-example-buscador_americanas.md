---
title: PIL example buscador americanas (snippet)
date: 2020-03-02
tags: ["python"]
---
Python PIL example 'buscador americanas'


Modules used in program: 
* `import webbrowser`
* `import requests`

## python buscador americanas

Python PIL example: buscador americanas

```python
import requests
from bs4 import BeautifulSoup
from tkinter import Tk
from tkinter import Label
from tkinter import Button
from PIL import Image, ImageTk
import webbrowser

produto = input("Informe o Produto:\n")

#1
url = 'https://www.americanas.com.br'
url_base = 'https://www.americanas.com.br/busca/{}'
#2
reposta = requests.get(url_base.format(produto))
#3
try:
    reposta.raise_for_status()
except Exception as exececao:
    print("Houve um problema:\n{}\nVerifique a URL {}".format(exececao, url_base))
#4
soup = BeautifulSoup(reposta.text, 'html.parser')

marcacoes_nome = 'h1'
atributo_nome = {'class_': 'card-product-name'}
marcacoes_preco = 'div'
atributo_preco = {'class_': 'card-product-price'}
marcacoes_link = 'a'
atributo_link = {'class_': 'card-product-url'}
marcacoes_foto = 'img'
atributo_foto = {'class_': 'card-product-picture'}

resultados_nome = soup.find_all(marcacoes_nome, **atributo_nome)
resultados_preco = soup.find_all(marcacoes_preco, **atributo_preco)
resultados_link = soup.find_all(marcacoes_link, **atributo_link)
resultados_foto = soup.find_all(marcacoes_foto, **atributo_foto)

nomes = []
precos = []
links = []
fotos = []
menor = -1
count = 0
indice_menor = 0

for n, p, l, f in zip(resultados_nome, resultados_preco, resultados_link, resultados_foto):
    produto_nome = n.get_text('h1')
    nomes.append(produto_nome)

    produto_preco = float(p.get_text('').replace('R$', '').replace('.', '').replace(',', '.').strip())
    precos.append(produto_preco)

    produto_link = l.get('href')
    link_completo = url + produto_link
    links.append(link_completo)

    produto_foto = f.get('src')
    fotos.append(produto_foto)

    if produto_preco < menor or menor == -1:
        menor = produto_preco
        indice_menor = count
    count += 1

final = []
final.append(nomes[indice_menor])
final.append(precos[indice_menor])
final.append(links[indice_menor])
final.append(fotos[indice_menor])
print(final)

janela_principal = Tk()
janela_principal.geometry('600x500')
janela_principal.title("Buscador de Preços 'americanas.com.br'")

label_produto = Label(janela_principal, text=final[0])
label_preco = Label(janela_principal, text=final[1])
label_produto.pack(side='top')
label_preco.pack(side='bottom')

with open('americanas.jpg', 'wb') as f:
    f.write(requests.get(final[3]).content)

imagem_pil = Image.open('americanas.jpg')
imagem_tk = ImageTk.PhotoImage(imagem_pil)

label_foto = Label(janela_principal, image=imagem_tk)
label_foto.pack(padx=130, pady=130)

botao_link = Button(janela_principal)
botao_link['text'] = 'Ir ao Site'
botao_link['command'] = lambda: webbrowser.open(final[2])
botao_link.pack()

janela_principal.mainloop()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
