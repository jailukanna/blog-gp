---
title: mysql example crud python qt4 (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'crud python qt4'


Modules used in program: 
* `import mysql.connector`
* `import sys`

## python crud python qt4

Python mysql example: crud python qt4

```python
#Muda codificação para UTF-8
# -*- coding: utf-8 -*-
#importações de modulos

import sys
from PyQt4 import QtCore, QtGui
import mysql.connector

try:
    _fromUtf8 = QtCore.QString.fromUtf8
except AttributeError:
    def _fromUtf8(s):
        return s

try:
    _encoding = QtGui.QApplication.UnicodeUTF8
    def _translate(context, text, disambig):
        return QtGui.QApplication.translate(context, text, disambig, _encoding)
except AttributeError:
    def _translate(context, text, disambig):
        return QtGui.QApplication.translate(context, text, disambig)

#Classe da tela de login
class Login(QtGui.QDialog):
    def __init__(self, parent=None):
        super(Login, self).__init__(parent)
        self.setWindowTitle('InfoCad - Login')
        self.setGeometry(400,300,300,250)

        #Label Login
        self.lb_logo = QtGui.QLabel('HL INFORMÁTICA', self)
        self.lb_user = QtGui.QLabel('Usuário:', self)
        self.lb_pass = QtGui.QLabel('Senha:', self)
        self.lb_logo.setGeometry(85, 25, 150, 15)
        self.lb_user.setGeometry(40, 60, 60, 30)
        self.lb_pass.setGeometry(40, 100, 60, 30)
        self.font = QtGui.QFont()
        self.font.setBold(True)
        self.font.setWeight(75)
        self.lb_logo.setFont(self.font)


        #Line Edit Login
        self.le_user = QtGui.QLineEdit(self)
        self.le_pass = QtGui.QLineEdit(self)
        self.le_user.setGeometry(100, 60, 150, 30)
        self.le_pass.setGeometry(100, 100, 150, 30)
        self.le_pass.setEchoMode(QtGui.QLineEdit.Password)

        #Botões Login
        self.btn_enter = QtGui.QPushButton("Entrar", self)
        self.btn_sair = QtGui.QPushButton('Cancelar', self)
        self.btn_enter.move(150,190)
        self.btn_sair.move(45,190)

        #Funções Botões
        self.btn_enter.clicked.connect(self.validarlogin)
        self.btn_sair.clicked.connect(sys.exit)

    #Metodo de conexão com banco
    def ConexaoBanco(self):
        BANCO = 'bancohlinfo'
        USER = 'hlinforj'
        PASSWD = '12345'
        HOST = 'db4free.net'
        try:
            con = mysql.connector.connect(user=USER, password=PASSWD, host=HOST, database=BANCO)
        except mysql as error:
            print('Não foi possivel conectar-se ao Banco de Dados', erro)
        return con

    #Testa se usuario e senha estão corretos
    def validarlogin(self):
        try:
            conexao = self.ConexaoBanco()
            cursor = conexao.cursor()
            usuario = self.le_user.text()
            senha = self.le_pass.text()
            sql = "SELECT * FROM staff where username like'"+usuario+"' and userpass like'"+senha+"'"
            cursor.execute(sql)
            valido = cursor.fetchall()

            if len(valido) > 0:
                print('Conectado!')
                self.accept()

            else:
                QtGui.QMessageBox.warning(self, 'Erro 157!', 'Usuário ou senha incorretos!')
        except mysql as erro:
            print('Nao conseguiu conectar', erro)

    '''
    def status(self):
        user = self.le_user.text()
        passw = self.le_pass.text()

        if user == 'admin' and passw == '123':

            print('Conectado!! %s %s' % (user, passw))
            self.accept()

        else:
            QtGui.QMessageBox.warning(self, 'Erro 157!', 'Usuário ou senha incorretos!')
    '''


#Classe da tela principal
class Ui_MainWindow(object):
    def setupUi(self, MainWindow):
        MainWindow.setObjectName(_fromUtf8("MainWindow"))
        MainWindow.setEnabled(True)
        MainWindow.resize(600, 400)
        self.centralwidget = QtGui.QWidget(MainWindow)
        self.centralwidget.setObjectName(_fromUtf8("centralwidget"))
        self.horizontalLayout_2 = QtGui.QHBoxLayout(self.centralwidget)
        self.horizontalLayout_2.setObjectName(_fromUtf8("horizontalLayout_2"))
        self.verticalLayout = QtGui.QVBoxLayout()
        self.verticalLayout.setObjectName(_fromUtf8("verticalLayout"))
        self.lbPesquisar = QtGui.QLabel(self.centralwidget)
        self.lbPesquisar.setMaximumSize(QtCore.QSize(60, 20))
        self.lbPesquisar.setObjectName(_fromUtf8("lbPesquisar"))
        self.verticalLayout.addWidget(self.lbPesquisar)
        self.lePesquisar = QtGui.QLineEdit(self.centralwidget)
        self.lePesquisar.setMaximumSize(QtCore.QSize(120, 16777215))
        self.lePesquisar.setObjectName(_fromUtf8("lePesquisar"))
        self.verticalLayout.addWidget(self.lePesquisar)
        self.cbpesquisar = QtGui.QComboBox(self.centralwidget)
        self.cbpesquisar.setMaximumSize(QtCore.QSize(120, 16777215))
        self.cbpesquisar.setObjectName(_fromUtf8("cbpesquisar"))
        self.cbpesquisar.addItem(_fromUtf8(""))
        self.cbpesquisar.addItem(_fromUtf8(""))
        self.cbpesquisar.addItem(_fromUtf8(""))
        self.cbpesquisar.addItem(_fromUtf8(""))
        self.cbpesquisar.addItem(_fromUtf8(""))
        self.cbpesquisar.addItem(_fromUtf8(""))
        self.verticalLayout.addWidget(self.cbpesquisar)
        spacerItem = QtGui.QSpacerItem(20, 40, QtGui.QSizePolicy.Minimum, QtGui.QSizePolicy.Expanding)
        self.verticalLayout.addItem(spacerItem)
        self.horizontalLayout_2.addLayout(self.verticalLayout)
        self.horizontalLayout = QtGui.QHBoxLayout()
        self.horizontalLayout.setObjectName(_fromUtf8("horizontalLayout"))
        self.tabWidget = QtGui.QTabWidget(self.centralwidget)
        self.tabWidget.setEnabled(False)
        self.tabWidget.setObjectName(_fromUtf8("tabWidget"))
        self.tab_2 = QtGui.QWidget()
        self.tab_2.setObjectName(_fromUtf8("tab_2"))
        self.tabWidget.addTab(self.tab_2, _fromUtf8(""))
        self.horizontalLayout.addWidget(self.tabWidget)
        self.horizontalLayout_2.addLayout(self.horizontalLayout)
        MainWindow.setCentralWidget(self.centralwidget)
        self.menubar = QtGui.QMenuBar(MainWindow)
        self.menubar.setGeometry(QtCore.QRect(0, 0, 600, 19))
        self.menubar.setObjectName(_fromUtf8("menubar"))
        self.menuClientes = QtGui.QMenu(self.menubar)
        self.menuClientes.setObjectName(_fromUtf8("menuClientes"))
        self.menuClientes_2 = QtGui.QMenu(self.menuClientes)
        self.menuClientes_2.setObjectName(_fromUtf8("menuClientes_2"))
        self.menuOrdem_Servi_o = QtGui.QMenu(self.menuClientes)
        self.menuOrdem_Servi_o.setObjectName(_fromUtf8("menuOrdem_Servi_o"))
        self.menuProduto = QtGui.QMenu(self.menuClientes)
        self.menuProduto.setObjectName(_fromUtf8("menuProduto"))
        self.menuVendas = QtGui.QMenu(self.menubar)
        self.menuVendas.setObjectName(_fromUtf8("menuVendas"))
        self.menuSistema = QtGui.QMenu(self.menubar)
        self.menuSistema.setObjectName(_fromUtf8("menuSistema"))
        self.menuUtilitario = QtGui.QMenu(self.menubar)
        self.menuUtilitario.setObjectName(_fromUtf8("menuUtilitario"))
        self.menuAjuda = QtGui.QMenu(self.menubar)
        self.menuAjuda.setObjectName(_fromUtf8("menuAjuda"))
        self.menuSair = QtGui.QMenu(self.menubar)
        self.menuSair.setObjectName(_fromUtf8("menuSair"))
        MainWindow.setMenuBar(self.menubar)
        self.statusbar = QtGui.QStatusBar(MainWindow)
        self.statusbar.setObjectName(_fromUtf8("statusbar"))
        MainWindow.setStatusBar(self.statusbar)
        self.actionNovo = QtGui.QAction(MainWindow)
        self.actionNovo.setObjectName(_fromUtf8("actionNovo"))
        self.actionNovo_2 = QtGui.QAction(MainWindow)
        self.actionNovo_2.setObjectName(_fromUtf8("actionNovo_2"))
        self.actionPesquisar = QtGui.QAction(MainWindow)
        self.actionPesquisar.setObjectName(_fromUtf8("actionPesquisar"))
        self.actionNovo_3 = QtGui.QAction(MainWindow)
        self.actionNovo_3.setObjectName(_fromUtf8("actionNovo_3"))
        self.actionPesquisar_2 = QtGui.QAction(MainWindow)
        self.actionPesquisar_2.setObjectName(_fromUtf8("actionPesquisar_2"))
        self.actionNovo_4 = QtGui.QAction(MainWindow)
        self.actionNovo_4.setObjectName(_fromUtf8("actionNovo_4"))
        self.actionPesquisar_3 = QtGui.QAction(MainWindow)
        self.actionPesquisar_3.setObjectName(_fromUtf8("actionPesquisar_3"))
        self.actionConfigura_o = QtGui.QAction(MainWindow)
        self.actionConfigura_o.setObjectName(_fromUtf8("actionConfigura_o"))
        self.actionBackup = QtGui.QAction(MainWindow)
        self.actionBackup.setObjectName(_fromUtf8("actionBackup"))
        self.actionRestaurar_Backup = QtGui.QAction(MainWindow)
        self.actionRestaurar_Backup.setObjectName(_fromUtf8("actionRestaurar_Backup"))
        self.actionCalculadora = QtGui.QAction(MainWindow)
        self.actionCalculadora.setObjectName(_fromUtf8("actionCalculadora"))
        self.actionBloco_de_Notas = QtGui.QAction(MainWindow)
        self.actionBloco_de_Notas.setObjectName(_fromUtf8("actionBloco_de_Notas"))
        self.actionSobre = QtGui.QAction(MainWindow)
        self.actionSobre.setObjectName(_fromUtf8("actionSobre"))
        self.actionSobre_2 = QtGui.QAction(MainWindow)
        self.actionSobre_2.setObjectName(_fromUtf8("actionSobre_2"))
        self.menuClientes_2.addAction(self.actionNovo_2)
        self.menuClientes_2.addAction(self.actionPesquisar)
        self.menuOrdem_Servi_o.addAction(self.actionNovo_3)
        self.menuOrdem_Servi_o.addAction(self.actionPesquisar_2)
        self.menuProduto.addAction(self.actionNovo_4)
        self.menuProduto.addAction(self.actionPesquisar_3)
        self.menuClientes.addAction(self.menuClientes_2.menuAction())
        self.menuClientes.addAction(self.menuOrdem_Servi_o.menuAction())
        self.menuClientes.addAction(self.menuProduto.menuAction())
        self.menuSistema.addAction(self.actionConfigura_o)
        self.menuSistema.addSeparator()
        self.menuSistema.addAction(self.actionBackup)
        self.menuSistema.addAction(self.actionRestaurar_Backup)
        self.menuUtilitario.addAction(self.actionCalculadora)
        self.menuUtilitario.addAction(self.actionBloco_de_Notas)
        self.menuAjuda.addAction(self.actionSobre)
        self.menuAjuda.addAction(self.actionSobre_2)
        self.menubar.addAction(self.menuClientes.menuAction())
        self.menubar.addAction(self.menuVendas.menuAction())
        self.menubar.addAction(self.menuSistema.menuAction())
        self.menubar.addAction(self.menuUtilitario.menuAction())
        self.menubar.addAction(self.menuAjuda.menuAction())
        self.menubar.addAction(self.menuSair.menuAction())

        self.retranslateUi(MainWindow)
        self.tabWidget.setCurrentIndex(0)
        QtCore.QObject.connect(self.cbpesquisar, QtCore.SIGNAL(_fromUtf8("activated(QString)")), self.lePesquisar.clear)
        QtCore.QMetaObject.connectSlotsByName(MainWindow)

    def retranslateUi(self, MainWindow):
        MainWindow.setWindowTitle(_translate("MainWindow", "InfoCad - Sistema de Cadastro", None))
        self.lbPesquisar.setText(_translate("MainWindow", "Pesquisar:", None))
        self.cbpesquisar.setItemText(0, _translate("MainWindow", "Ordem Serviço", None))
        self.cbpesquisar.setItemText(1, _translate("MainWindow", "Nome", None))
        self.cbpesquisar.setItemText(2, _translate("MainWindow", "CPF", None))
        self.cbpesquisar.setItemText(3, _translate("MainWindow", "Marca", None))
        self.cbpesquisar.setItemText(4, _translate("MainWindow", "Modelo", None))
        self.cbpesquisar.setItemText(5, _translate("MainWindow", "Telefone", None))
        self.tabWidget.setTabText(self.tabWidget.indexOf(self.tab_2), _translate("MainWindow", "Resultado:", None))
        self.menuClientes.setTitle(_translate("MainWindow", "Cadastro", None))
        self.menuClientes_2.setTitle(_translate("MainWindow", "Cliente", None))
        self.menuOrdem_Servi_o.setTitle(_translate("MainWindow", "Ordem Serviço", None))
        self.menuProduto.setTitle(_translate("MainWindow", "Produto", None))
        self.menuVendas.setTitle(_translate("MainWindow", "Vendas", None))
        self.menuSistema.setTitle(_translate("MainWindow", "Sistema", None))
        self.menuUtilitario.setTitle(_translate("MainWindow", "Utilitario", None))
        self.menuAjuda.setTitle(_translate("MainWindow", "Ajuda", None))
        self.menuSair.setTitle(_translate("MainWindow", "Sair", None))
        self.actionNovo.setText(_translate("MainWindow", "Novo", None))
        self.actionNovo_2.setText(_translate("MainWindow", "Novo", None))
        self.actionPesquisar.setText(_translate("MainWindow", "Pesquisar", None))
        self.actionNovo_3.setText(_translate("MainWindow", "Novo", None))
        self.actionPesquisar_2.setText(_translate("MainWindow", "Pesquisar", None))
        self.actionNovo_4.setText(_translate("MainWindow", "Novo", None))
        self.actionPesquisar_3.setText(_translate("MainWindow", "Pesquisar", None))
        self.actionConfigura_o.setText(_translate("MainWindow", "Configuração", None))
        self.actionBackup.setText(_translate("MainWindow", "Backup", None))
        self.actionRestaurar_Backup.setText(_translate("MainWindow", "Restaurar Backup", None))
        self.actionCalculadora.setText(_translate("MainWindow", "Calculadora", None))
        self.actionBloco_de_Notas.setText(_translate("MainWindow", "Bloco de Notas", None))
        self.actionSobre.setText(_translate("MainWindow", "Site", None))
        self.actionSobre_2.setText(_translate("MainWindow", "Sobre", None))

#Roda o sistema
if __name__ == '__main__':

    app = QtGui.QApplication(sys.argv)
    login = Login()

    if login.exec_() == QtGui.QDialog.Accepted:
        MainWindow = QtGui.QMainWindow()
        ui = Ui_MainWindow()
        ui.setupUi(MainWindow)
        MainWindow.show()
        sys.exit(app.exec_())

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
