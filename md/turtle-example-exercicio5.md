---
title: turtle example exercicio5 (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'exercicio5'

Functions in program: 
* `def main(posx, posy, tamanho, n):`
* `def draw_circle(x, y, r):   # Aqui desenho o circulo, r é o argumento para raio`
* `def draw_square(x, y, t):  # Aqui desenha-se o quadrado`
* `def coordenates(x, y):  # Esse método será responsável por levantar a caneta de`

## python exercicio5

Python turtle example: exercicio5

```python
from turtle import Screen, Turtle

screen = Screen()
turtle = Turtle()

def coordenates(x, y):  # Esse método será responsável por levantar a caneta de
    turtle.penup()      # desenho da tela e coloca-la na posiçõa que queiramos desenhar.
    turtle.goto(x, y)   # Falo caneta, pois desenhar na tela com turtle é como
    turtle.pendown()    # desenhar constantemente com a caneta no papel,
                        # precisando em algum momento levantá-la para ajeita-la para outra parte do papel

def draw_square(x, y, t):  # Aqui desenha-se o quadrado
    coordenates(x, y)      # a função coordenada ajusta a posição do denseho na tela

    for x in range(4):     # È meio que padrão e simples como se desenha um quadrado
        turtle.fd(t)       # você faz um lado e vira 90 graus
        turtle.left(90)    # O argumento letra "t" é o tamanho do quadrado,
                           # x e y são a posição onde desenharemos o quadrado.

def draw_circle(x, y, r):   # Aqui desenho o circulo, r é o argumento para raio
    coordenates(x, y)       # Mas para definir tamanho em circulo deve-se enteder quer o diâmetro é
                            # o dobro do raio, então eu espero como argumento o para raio metade d0
    turtle.circle(r)        # tamanho do quadrado, para circulo e quadrado ocuparem quase o emsmo esxpaço.


# Tres aspas (''') simples é um tipo de comentário (ou linha não detectada) pelo python
'''
    E agora aqui vem o pulo do gato:
    Se temos uma noção do tamanho que queremos para nosso desenho, seja quadrado e cicrulo
    vamos inseri-los como argumento na função abixo para desenhá-los.
    Nossa única preocupação aqui é em quantas sequências internas nos queremos desenhar
    nosso desenho (Eu sei ficou confuso, mas vou explicar aqui)!
'''

'''
    Minha função abaixo desenha tantas vezes eu quero a seguinte figura:
     
      * * * * * * * * * * * * 
      *  * * * *     0      *
      *  *     *   0   0    *
      *  *     *   0   0    *
      *  * * * *     0      *
      *                     *
      *  * * * *     0      *
      *  *     *   0   0    *
      *  *     *   0   0    *
      *  * * * *     0      *
      * * * * * * * * * * * *
    
    Primeiramente só foquei em desenhar o desenho acima e para tal, obedeci as seguintes regras:
    1 - desenhar um quadrado grande, 
    2 - depois dividir seu tamnho por quatro, pois desenharei quatro figuras dentro dele.
    3 - E levar em consideração o espaço que separa cada desenho do outro
    
    O que me ajudou nisso foi entender como são localizada as coordenadas em turtle:
                    Y
                    |
                (0, 10)
                    |
                    |
                    |
    _(-10, 0)______(00)________(10, 0)__x
                    |
                    |
                    |
                (0, -10)
                
    - O centro é x = 0  e y = 0
    - A esquerda é x = valores negativos (abaixo de zero/ abaixo de um)
    - A direita é x = Valores positivos (Acima de zero)
    - O topo é y = valores positivos
    - E a parte inferior y = valores negativos
'''
'''
    Usando os conceitos que descrevir acima tem-se o código abiaxo:
'''

def main(posx, posy, tamanho, n):
    # desenhamos o primeiro quadrado, aquele que é o maior de todos
    draw_square(posx, posy, tamanho)

    '''
    então passamos a dividir as medidads por 4, 
    contudo, o espção eu preferir medir por valores menores no caso 10
    '''
    espaco = tamanho/10
    posx = posx + espaco
    posy = posy + espaco
    tamanho = tamanho/3

    '''
        Depois de dividir as medidas eu desenho os desenho internos
        com as funções de desenho acima.
        Veja que incremento espaço para algumas coordenadas nos desenhos
    '''

    draw_square(posx, posy, tamanho)
    draw_square(posy, posy + tamanho + espaco, tamanho)

    '''
        Para o circulo eu fiz o seguinte calculo para seu posicionamento:
        - Como quero desenhar a direita da posição dos quadrados,
        - calculei duas vezes o tamanho do quadrado, basta somar o tamanho duas vezes a x
    '''
    draw_circle(posx+(2*tamanho), posy, tamanho/2)
    draw_circle(posx + (2 * tamanho), posy + tamanho + espaco, tamanho / 2)
    '''
        Até aqui tudo bem, mas e se qusiermos desenhar o memso desenho diversas vezes
        sempre passando para o quadrado inferior esquerdo, constanetemente ?
        Resposta: Simples, façamos recursivamente.
        Usarei a variável n como limitador para quando parar de desenhar
    '''

    n -= 1

    if n > 0:
        main(posx, posy, tamanho, n) # chamarei a funçao internamente quantas vezes n for maior que zero


# XXXXXXXXXXXXXXXXXXX Abaixo a execução XXXXXXXXXXXXXXXX

if __name__ == '__main__':
    main(-150, -150, 300, 5) # usei valores negativos pois considerei a posição dos desenhos na tela. Esse valor para mim funciona.

screen.mainloop() # isso aqui é só para evitar erros maiores


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
