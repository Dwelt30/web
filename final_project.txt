import matplotlib.pyplot as plt
from pprint import pprint
def reading (s):
    f=open(s)
    a=f.read()
    return a
def get_headers (s):
    s=s.split('\n')
    a = [s[0].split(',')]
    g=a[0]
    h=g
    for x in g:
        if x.isdigit():
            h.replace(x, float(x))
        elif (x.replace('.', ' ', 1)).isdigit():
            h.replace(x, float(x))
    return h
def get_data (s):
    s=s.split('\n')
    s=s[1:-1]
    g=[]
    for x in s:
        x=x.split(',')
        b=[]
        for a in x: 
            if a.isdigit():
                b.append(float(a))
            elif (a.replace('.', '', 1)).isdigit():
                b.append(float(a))
            else:
                b.append(a)
        g.append(b)
    return g
def make_year_dict (data):
    year={}
    for a in data:
        year[a[0]]=a[1:-1]
        year[a[0]].append(a[-1])
    return year
def combine_dict (year_dict, headers):
    a=headers[1:-1]
    b=headers[-1]
    a.append(b)
    z={}
    for x in year_dict:
        c={}
        for y in a:
            c[y]=year_dict[x][a.index(y)]
        z[x]=c
    return z
def full_dict (s):
    s=reading(s)
    a=get_headers(s)
    b=get_data(s)
    b=make_year_dict(b)
    c=combine_dict(b, a)
    c[s.split(',')[0]]=0
    return c
def choose_graph (d):
    a=list(d.keys())
    b=list(d.values())
    c=''
    if a[0]==float(a[0]):
        if float(list(b[0].values())[0])==list(b[0].values())[0]:
            if a[-1]=='Year' or ('Time' in a[-1]) or a[-1]=='Day' or a[-1]=='year' or ('time' in a[-1]) or a[-1]=='day':
                c='plot'
            else:
                c='scatter'
        else:
            c='hist'
    else:
        if float(list(b[0].values())[0])==list(b[0].values())[0]:
            c='bar'
        else:
            c='hist'
    return c
def graphing (g, j):
    d=full_dict(g)
    a=choose_graph(d)
    b=list(d.keys())
    b0=b[-1]
    b.remove(b[-1])
    c=list(d.values())
    c.remove(c[-1])
    e=list(c[0].keys())
    f=[]
    if a=='plot':
        for x in e:
            for y in c:
                f.append(y[x])
            plt.plot(f, label=x)
            plt.xlabel(b0)
    elif a=='scatter':
        for x in e:
            for y in c:
                f.append(y[x])
            plt.scatter(b, f, label=x)
            f=[]
            plt.xlabel(b0)
    elif a=='hist':
        for x in e:
            for y in c:
                f.append(d[x])
            plt.hist(f, label=x)
            plt.xlabel(b0)
    elif a=='bar':
        for x in e:
            for y in c:
                f.append(d[x])
            plt.bar(b, f, label=x)
            plt.xlabel(b0)
    plt.title(g)
    plt.legend()
    plt.savefig(j)
    plt.show()
def make_html (g, j):
    d=full_dict(g)
    a=choose_graph(d)
    b=list(d.keys())
    b0=b[-1]
    b.remove(b[-1])
    c=list(d.values())
    c.remove(c[-1])
    e=list(c[0].keys())
    y='''
    </tbody>
</table>
</body>'''
    z='''<!DOCTYPE html>
<html lang=“en”>
<title>'''+g+'''</title>
<body>
<img src='''+j+''' display: block;>
<table display: block;>
    <thead>
        <tr>
            <th>'''+b0+'''</th>'''
    for x in e:
            z+='''
            <th>'''+x+'''</th>'''
    z+='''
        </tr>
    </thead>
    <tbody>'''
    for f in b:
        z+='''
        <tr>
            <td>'''+str(f)+'''</td>'''
        for h in e:
            z+='''
            <td>'''+str(d[f][h])+'''</td>'''
        z+='''
        </tr>'''
    f=z+y
    graphing(g, j)
    print(f)
