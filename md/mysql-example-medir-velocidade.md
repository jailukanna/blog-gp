---
title: mysql example medir-velocidade (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'medir-velocidade'


Modules used in program: 
* `import mysql.connector`
* `import datetime`

## python medir-velocidade

Python mysql example: medir-velocidade

```python
import datetime
import mysql.connector
from collections import defaultdict



cnx = mysql.connector.connect(user='lindaines', database='sms', password='K5zlu7N2PKj5', host='64.90.56.66')
cursor = cnx.cursor()

listaDeCampanhas = []
listaDeVelocidadeCampanhaCliente = []
listaDeMedicoes = []

#pegando as campanhas que estao enviando
queryBuscaCampanhasEnviando = ("""
                select id from Campanha where status = 'enviando'
        """)

#gravando o Id das campanhas em uma lista
cursor.execute(queryBuscaCampanhasEnviando)
campanhas = cursor.fetchall()
for id in campanhas:
    idCampanha = (id[0])
    listaDeCampanhas.append(idCampanha)

#pegando a velocidade, a Campanha e o Cliente
for idCampanha in listaDeCampanhas:
    queryBuscaVelocidadeCampanha= ("""
                    select velocidade, campanhaId, Cliente.id from RelatorioVelocidadeCampanha
    				   inner join Campanha on RelatorioVelocidadeCampanha.campanhaId = Campanha.id
    				   inner join Usuario on Campanha.usuario_id = Usuario.id
    				   inner join Cliente on Usuario.cliente_id = Cliente.id
                       where RelatorioVelocidadeCampanha.campanhaId = %d
                       and dataMedicao = (
                           select max(RelatorioVelocidadeCampanha.dataMedicao) from RelatorioVelocidadeCampanha where campanhaId = %d
                        )
        """ %(idCampanha,idCampanha))
    cursor.execute(queryBuscaVelocidadeCampanha)
    result = cursor.fetchall()
    for item in result:
        listaDeVelocidadeCampanhaCliente.append(item)

for item in listaDeVelocidadeCampanhaCliente:
    idCliente = (item[2])
    velocidade = (item[0])
    medicao = {'idCliente' : idCliente, 'velocidade' : velocidade}
    listaDeMedicoes.append(medicao)

somaVelocidades = defaultdict(float)
for item in listaDeMedicoes:
    somaVelocidades[item['idCliente']] += item['velocidade']

for item in somaVelocidades:
    print((item[idCliente]))
    print((item[velocidade]))


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
