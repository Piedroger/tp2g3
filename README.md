# tp2g3
Grupo3print()

import numpy as np
import pandas as pd

#valores ingresados de filas y columnas

m=int(input("Numero de filas: "))
n=int(input("Numero de columnas: "))

M=[]
for i in range(m):
    print("FILA NÚMERO ",i+1)
    m1=str(input("---Ingrese el nombre de la fila : "))
    M=M+[m1]


N=[]
nomC=[]
for i in range(n):
    print("Columna",i+1)
    nomC.append(str(input("Nombre de la columna: ")))


for j in range(n):
    N.append([])
    for i in range(m):
        print("- Columna: ",j+1," fila: ",i+1)
        f=int(input("Ingrese los valores de la columna (de arriba hacia abajo): "))
        N[j]=N[j]+[f]  
print(N)

data={} 
for i in range(len(N)):
    data[nomC[i]]=N[i]
print(data)

df=pd.DataFrame(data,index=M)
df

f = [1,2,3,4]
c = [1,2,3]
suma_filas = list(sum(df.iloc[i]) for i in range(len(M)))
print("suma de filas:",suma_filas)
suma_columnas = list(sum(df.iloc[:,i]) for i in range(len(nomC)))
print("suma de columnas:",suma_columnas)
total = sum(suma_filas)
print('total:',total)

###### Coeficiente de Pearson ######
#f_b = 0
#c_b = 0
#for i in range(len(suma_filas)):
    #f_b += (suma_filas[i]*f[i])/total
#for j in range(len(suma_columnas)):
    #c_b += (suma_columnas[j]*c[j])/total
#print('f_b:',f_b)
#print('f_c:',c_b)

#for i in range(len(suma_filas)):
#    for j in range(len(suma_columnas)):
#        s1 += (df.iloc[i,j]*(f[i]-f_b)*(c[j]-c_b))
#        s2 += (df.iloc[i,j]*(f[i]-f_b)**2)
#        s3 += (df.iloc[i,j]*(c[j]-c_b)**2)
#pp = s1/(s2*s3)**0.5        
#print('s1,s2,s3:',s1,s2,s3)
#print('pp:',pp)


###### Coeficiente de Spearman ######
fi = []
ci = []
for i in range(len(suma_filas)):
    l = 0
    if i > 0:
        l += sum(suma_filas[:i]) + (suma_filas[i]+1)/2
    else:
        l += (suma_filas[i]+1)/2
    fi.append(l)

for i in range(len(suma_columnas)):
    l = 0
    if i > 0:
        l += sum(suma_columnas[:i]) + (suma_columnas[i]+1)/2
    else:
        l += (suma_columnas[i]+1)/2
    ci.append(l)
print('fi,ci:',fi,ci)

f_b = 0
c_b = 0
for i in range(len(suma_filas)):
    f_b += (suma_filas[i]*fi[i])/total
for j in range(len(suma_columnas)):
    c_b += (suma_columnas[j]*ci[j])/total
    
print('f_b,c_b:',f_b,c_b)

s1 = 0
s2 = 0
s3 = 0

for i in range(len(suma_filas)):
    for j in range(len(suma_columnas)):
        s1 += (df.iloc[i,j]*(fi[i]-f_b)*(ci[j]-c_b))
        s2 += (df.iloc[i,j]*(fi[i]-f_b)**2)
        s3 += (df.iloc[i,j]*(ci[j]-c_b)**2)

print('s1,s2,s3:',s1,s2,s3)
ps = s1/(s2*s3)**0.5
print('ps:',ps)
