---
sidebar_position: 1
---

# 1 - SQL

## Omitir caracteres

```jsx title="Ejemplo"
SUBSTRING(string, start_position, length)
```

string: Es el valor o columna sobre la cual quieres trabajar (en este caso, Codigo).

start_position: Es la posición en la que comienza el substring. Recuerda que la indexación en SQL Server empieza desde 1 (no desde 0 como en otros lenguajes). Así que, si start_position es 5, significa que empezamos desde el 5.º carácter.

length: Es la longitud del substring, es decir, cuántos caracteres quieres tomar a partir de esa posición. Acá se toma la cadena total menos la cantidad de caracteres a omitir.


#### Ejemplo

```jsx title="Ejemplo"
SUBSTRING(Codigo, 5, LEN(Codigo) - 4)

// Codigo = '123456789'.
```
start_position = 5: Empieza desde el 5.º carácter de Codigo. Esto significa que se omiten los primeros 4 caracteres.

length = LEN(Codigo) - 4: La longitud del substring será igual a la longitud total de Codigo menos 4, lo que hace que se tomen todos los caracteres a partir del 5.º carácter hasta el final.

El resultado sería '56789' porque empieza desde el carácter 5 ('5') y toma 5 caracteres ('56789').

---------------------------------------------

## Reemplazar caracteres

```jsx title="Ejemplo"
SELECT REPLACE(CONVERT(VARCHAR, Fecha, 103), '/', '.') AS FechaModificada
FROM TuTabla;

```

Explicación:

**CONVERT(VARCHAR, Fecha, 103):** Convierte la fecha Fecha a formato DD/MM/YYYY (que es el estilo 103 en SQL Server).

**REPLACE(CONVERT(VARCHAR, Fecha, 103), '/', '.'):** Reemplaza las barras / por puntos ..


#### Ejemplo

Si Fecha = '01/01/2025', el resultado sería '01.01.2025'.

Si lo quieres hacer en un UPDATE para modificar los valores en la tabla, sería algo como:

```jsx title="Ejemplo"
UPDATE TuTabla
SET Fecha = REPLACE(CONVERT(VARCHAR, Fecha, 103), '/', '.')
WHERE Fecha IS NOT NULL;

```

------------------------------------------------------------------------

## Mostrar datos en otro formato

La función FORMAT() en SQL Server se usa para dar formato a valores de fecha y números de manera flexible. Permite personalizar la salida usando patrones específicos de formato 

```jsx title="Ejemplo"
FORMAT(valor, formato)
```

Ejemplo básico para documento tutorial

```jsx title="Ejemplo"
-- Formatear una fecha en el formato dd.MM.yyyy
SELECT FORMAT(GETDATE(), 'dd.MM.yyyy') AS FechaFormateada;

```

FechaFormateada  
--------------  
21.03.2025  
