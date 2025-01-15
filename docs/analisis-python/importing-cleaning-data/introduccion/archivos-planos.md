---
title: Introducción a Importación de Datos
status: new
---

## Archivos Planos

Se abordará los temas de la importación de datos sin formato, ya sea un `.txt` o `.csv`

```py title="Archivos sin formato `.txt`"
with open("archivo.txt", "r") as file:
    print(file.read())
```
<div class="result" markdown>
Mediante la cláusula `with` podemos abrir un archivo sin formato como los `.txt`, indicando si queremos leer `'r'` o escribir `'w'`, sin tener que preocuparnos en tener que cerrar `.close()`.
</div>

```py title="Leer con `numpy`"
np.loadtxt(file, delimiter=None, skiprows=0, encoding=None, dtype=float)
```
<div class="result" markdown>
Si nuestro archivo son datos de solo tipo numérico, se puede utilizar numpy o si queremos trabajar con texto se usa `#!py dtype=str`.

Si queremos omitir el encabezado, se usa `#!py skiprows=1`. En caso que solo querramos extraer la columna 1 y 3, se usa `#!py usecols=[0, 2]`.

Si el delimitador es la tabulación, entonces se usa `#!py delimiter='\t'`
</div>

```py title="Leer con `.pandas`"
pd.read_csv(file, *, 
            delimiter=None,
            header='infer', 
            skiprows=None, 
            na_values=None, 
            encoding=None, 
            date_parser=None,
            comment=None
            )
```
<div class="result" markdown>
* `delimiter=None`: Detecta automáticamente el delimitador.
* `header='infer'`: Infiere que la primera fila es el encabezado, si no tiene se coloca `None`.
* `skiprows=None`: Se indica si queremos saltear las N primeras filas. 
* `na_values=None`: Se indica mediante una lista los valores que serán reemplazados por `NaN`.
* `encoding=utf-8`: Se indica el tipo de encoding para poder leer, pueder ser el `latin-1` y otros.
* `date_parser`=None`: Si hay una columna que es de tipo fecha, se indica aquí con el nombre de la columna.
* `comment=None`: Se indica el caracter que da entrada a los comentarios `#`.
</div>