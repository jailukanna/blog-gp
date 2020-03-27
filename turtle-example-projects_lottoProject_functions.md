---
title: turtle example projects lottoProject functions (snippet)
date: 2020-02-09
tags: ["python"]
---
Python turtle example 'projects lottoProject functions'


Modules used in program: 
* `import sys;`
* `import time;`
* `import random;`

## python projects lottoProject functions

Python turtle example: projects lottoProject functions

```python
# Copyright (c) Grabner Stefan Johannes, grabner.stefan15@gmail.com
# Documentation: github.com/dementisimus


import random;
import time;
import sys;

class Functions:

    tips = [];
    lsaff = [];

    def gen(self, list):
        i = 0;
        while i < 7:
            if len(list) < 6:
                ra = random.randint(1, 45);
                if self.contains(list, ra) == False:
                    list.append(ra);
                    i += 1;
                else:
                    self.gen(list);
            else:
                break;
        return list;

    def contains(self, container, con):
        co = False;
        for i in range(0, len(container)):
            if container[i] == con:
                co = True;
        return co;

    def check(self, list, tips):
        rtips = [];
        for i in range(0, len(list)):
            if list[i] in tips:
                rtips.append(list[i]);
        return rtips;

    def loadPgBar(self):
        print("\nEs werden nun sechs zufällige Zahlen zwischen 1 und 45 gezogen...");
        tb = 40;

        sys.stdout.write("[%s]" % (" " * tb));
        sys.stdout.flush();
        sys.stdout.write("\b" * (tb+1));

        for i in range(tb):
            time.sleep(0.1);
            sys.stdout.write("=");
            sys.stdout.flush();

        sys.stdout.write("]\n");

    def startSession(self):
        print("|    |    _____/  |__/  |_  ____");
        print("|    |   /  _ \   __\   __\/  _ \/");
        print("|    |__(  <_> )  |  |  | (  <_> )");
        print("|_______ \____/|__|  |__|  \____/");
        print("        \/                        ");

        sl = 3;
        time.sleep(sl);
        print("\nGuten Tag und herzlich willkommen bei Lotto 6 aus 45!");
        time.sleep(sl);
        print("Sie dürfen nun sechs verschiedene Zahlen zwischen 1 und 45 eingeben.");
        time.sleep(sl);
        print("Danach werden sechs zufällige Zahlen gezogen.");
        time.sleep(sl);
        print("Sie gewinnen, wenn Sie eine Zahl erraten haben.");
        time.sleep(sl);
        print("Wir wünschen Ihnen viel Spaß!\n");

        time.sleep(2);

    def getArr(self, list):
        li = "";
        lim = "";
        for i in range(0, len(list)):
            if i != (len(list)-1):
                li += str(list[i]) + ", ";
            else:
                li += str(list[i]);
        lim = li.rstrip(',');
        return lim;

    def ti(self, skipIntro):
        if(skipIntro == False):
            self.startSession();
        try:
            i = 0;
            while i < 7:
                if len(self.tips) < 6:
                    inp = int(input("Geben Sie Ihren "+str(len(self.tips)+1)+". Tipp ein: "));
                    if self.contains(self.tips, inp) == False:
                        if inp <= 45 and inp >= 1:
                            self.tips.append(inp);
                            i += 1;
                        else:
                            print("Geben Sie bitte eine Zahl zwischen 1 und 45 ein.");
                    else:
                        print("Geben Sie bitte einen Tipp, den Sie noch nicht eingegeben haben, ein!");
                else:
                    break;
            gens = self.gen(self.lsaff);
            er = self.check(gens, self.tips);
            self.loadPgBar();
            print("\nDie gezogenen Zahlen lauten " + self.getArr(gens) + ".");
            if len(er) == 1:
                print("Sie haben eine Zahl erraten. Die Zahl lautet " + str(er[0]) + ".");
            else:
                ergs = "";
                za = 0;
                for i in range(0, len(er)):
                    ergs += str(er[i]) + " ";
                    za += 1;
                add = "";
                if za != 0:
                    add = " Die Zahlen lauten " + ergs;
                else:
                    za = "keine";
                print("Sie haben " + str(za) + " Zahlen erraten." + add);
            input("\nDrücken Sie Enter, um das Programm zu beenden.");
        except ValueError:
            print("Geben Sie bitte eine Zahl zwischen 1 und 45 ein.");
            self.ti(True);
    def load(self):
        self.ti(False);

```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
