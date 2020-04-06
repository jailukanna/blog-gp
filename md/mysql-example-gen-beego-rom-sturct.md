---
title: mysql example gen-beego-rom-sturct (snippet)
date: 2020-03-03
tags: ["python"]
---
Python mysql example 'gen-beego-rom-sturct'

Functions in program: 
* `def main():`
* `def gen_model(host,port,user,password,dbname,table, orm):`
* `def convertType(typ):`
* `def execute(db, sql):`
* `def query(db, sql, dataType=ROW_OF_KEY):`
* `def dbOpen(host, port, user, password, dbname):`
* `def escape(var):`
* `def now():`

Modules used in program: 
* `import _mysql`
* `import getopt`
* `import time`
* `import logging`
* `import sys`

## python gen-beego-rom-sturct

Python mysql example: gen-beego-rom-sturct

```python
#!/env/bin python
#coding:utf8
'''
使用 desc table; 解释MySQL的表结构。生成 golang- beego框架ORM的结构体定义
see doc: http://beego.me/docs/mvc/model/models.md
'''
import sys
import logging
import time
import getopt
import _mysql

def now():
    return time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()) 

def escape(var):
    '''这里连接数据库都是使用utf8的。'''
    if var is None:
        return ''
    if isinstance(var, unicode):
        var = var.encode('utf8')
    if not isinstance(var, str):
        var = str(var)
    return _mysql.escape_string(var)

def dbOpen(host, port, user, password, dbname):
    conn = _mysql.connect(
            db=dbname,
            host=host,
            user=user,
            passwd=password,
            port=port)
    conn.query("set names utf8;")
    return conn

ROW_OF_IDX=0
ROW_OF_KEY=1
DEBUG=0
def query(db, sql, dataType=ROW_OF_KEY):
    global DEBUG
    if DEBUG and not sql.startswith('SELECT'):
        print('in debug,just print:', sql)
        return

    rows = ()
    try:
        db.query(sql)
        res = db.store_result()
        if res:
            rows = res.fetch_row(res.num_rows(), dataType)
    except Exception, e:
        print("[%s]\t[%s]" % (e, sql))
        raise e
    return rows

def execute(db, sql):
    if DEBUG:
        logging.warning("Debuging, just print(SQL:%s", sql))
        return -110
    try:
        db.query(sql)
        res = db.affected_rows()
        if res < 0 or res == 0xFFFFFFFFFFFFFFFF:
            # ps : 0xFFFFFFFFFFFFFFFF (64位的-1) 
            # 这个值与驱动、系统、硬件CPU位数都可能有关
            logging.error('MySQL execute error n=[%d], sql=%s', res, sql)
        return res
    except Exception, e:
        logging.error("err=[%s]\tsql=[%s]", e, sql)
        if e[0] == 1062:
             return 0
        raise e
    return -120

def convertType(typ):
    typ = typ.lower()
    if typ.find('int') >= 0:
        return "int64", '%d'
    elif typ.find('char') >= 0 or typ.find("text") >= 0 or typ.find("enum") >= 0:
        return "string", "'%s'"
    elif typ.find("decimal") >= 0:
        return "float64",'%f'
    elif typ.find('datetime') >= 0:
        return "time.Time", "'%s'"
    elif typ.find("bool") >= 0:
        return "bool", "'%s'"
    else:
        return typ, "'%v'"

def gen_model(host,port,user,password,dbname,table, orm):
    db = dbOpen(host, port, user, password, dbname)

    desc = query(db, "desc %s;" % escape(table), ROW_OF_KEY)
    indent = ' ' * 4
    imports =[]
    const = [indent +'_tablename = "%s"' % table]
    vars =[]
    struct = ["type %s struct{" % table.title()]
    fields = []
    formats = []

    pk = None

    field_define =[]
    field_tags = []
    field_comments = []
    for row in desc:
        field = row['Field']
        typ, fmt = convertType(row['Type'])
        if typ == 'time' and (time not in imports):
            imports.append("time")

        tags = []
        if not orm:
            tags.append('db:"%s"' % field)

        else:
            tag = ["column(%s)" % field]
                
            if row['Null'] == "YES":
                tag.append("null")
            if row['Type'].startswith('decimal'):
                tag.append("digits(10);decimals(2)")
            if row['Type'].startswith('datetime'):
                if field.find("created")>=0:
                    tag.append("auto_now_add")
                if field.find("update")>=0:
                    tag.append("auto_now")
                tag.append("type(datetime)")
            if row['Key'].upper().find("PRI") >= 0:
                tag.append('pk')

            tags.append('orm:"%s"' % ";".join(tag))

        tags.append('json:"%s"' % field)

        field_define.append('%s %s' % (field.title(), typ))
        field_tags.append(tags)
        field_comments.append(row['Type'])

        if not row['Extra'].find("auto_increment") >= 0:
            fields.append("`%s`" % field)
            formats.append(fmt)
    
    cols_indent = {}
    for tags in field_tags:
        for i, t in enumerate(tags[:-1]):
            cols_indent[i] = max(len(t)+1, cols_indent.get(i,0))
        cols_indent[len(tags)] = 0

    for i, tags in enumerate(field_tags):
        struct.append("    %s `%s` // %s" %(field_define[i],
            ''.join([t + (" " * (cols_indent[j] - len(t))) for j, t in enumerate(tags)]).strip(),
            field_comments[i]))

    struct.append("}")

    vars.append((indent + '_fiels_map = []string{%s}') % ', '.join( [f for f in fields]))
    
    if not orm:
        const.append((indent + '_values_fmt = "%s"') % ','.join(formats))
        insert = ('_INSERT = fmt.Sprintf("INSERT INTO `%s`(%s) VALUES %s", '
            '_tablename, strings.Join(_fiels_map,","), _values_fmt)')
            
        imports.insert(0, indent+'"strings"')

        vars.append(indent+insert)
        if pk:
            delete = '_DELETE = fmt.Sprintf("DELETE FROM `%s` WHERE %s" ,_tablename,"'+  pk +'")'
            vars.append(indent + delete)

    if orm:
        imports.append(indent + '"github.com/astaxie/beego/orm"') # for ORM http://beego.me/docs/mvc/model/orm.md
    else:
        imports.append(indent + '"my.company/lib/core/mysql"') # for ORM
    print("package %s\n\n" % table)
    if imports:
        print("import (\n%s\n)\n\n" % '\n'.join(imports))

    print("const(\n%s\n)\n\n" % ('\n'.join(const)))
    if vars:
        print("var(\n%s\n)\n\n" % ('\n'.join(vars)))

    print('\n'.join(struct))

    if orm:
        print('\nfunc (self *%s) GetTableName() string {\n%sreturn  _tablename\n}' % (table.title(), indent))
        print('\nfunc (self *%s) TableEngine() string {\n%sreturn  "INNODB"\n}' % (table.title(), indent))
        print('\nfunc init() {\n    // 需要在init中注册定义的model\n    orm.RegisterModel(new(%s))\n}' % table.title())


def main():
    def usage():
        print("--help: print(this message")
        print("-h --host, MySQL host")
        print("-P --port, MySQL port")
        print("-u --user, MySQL user")
        print("-p --password, MySQL password")
        print("-D --database, MySQL Database")
        print("-t --table name")
        print("-o --orm, used OMR define struct.")
    try:
        opts, args = getopt.getopt(sys.argv[1:], "Hh:P:u:p:D:t:ol:", 
            ["--help","redis=","host=","port=","user=","password=",
            "database=",'table=',"--orm"])
    except getopt.GetoptError:
        print(usage())
        return

    host = 'localhost'
    user = "root"
    port = 3306
    password = ''
    dbname = "mysql"
    table = "user"
    orm = True
    for o, a in opts:
        if o in ("-H","--help"):
            usage()
            sys.exit()
        elif o in ("-o","--orm"):
            orm = True
        elif o in ("-h","--host"):
            host=a
        elif o in ("-P","--port"):
            port=int(a)
        elif o in ("-u","--user"):
            user=a
        elif o in("-p","--password"):
            password=a
        elif o in ("-D","--database"):
            dbname=a
        elif o in ('-t', '--table'):
            table=a

    logging.info("mysql=[%s:%s@%s:%s/%s?table=%s]",
        user, password, host, port, dbname, table)
    gen_model(host,port,user,password,dbname,table,orm)


if __name__ == '__main__':
    main()


```

## Python links

- Learn Python: https://pythonbasics.org/
- Python Tutorial: https://pythonprogramminglanguage.com
