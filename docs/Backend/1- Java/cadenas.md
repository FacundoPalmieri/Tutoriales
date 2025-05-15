---
sidebar_position: 7
---

# 7 - Manejo de cadenas

## toCharArray()

Convierte un String en un arreglo de caracteres (char[]).

```jsx title=" "
String texto = "Hola";
char[] letras = texto.toCharArray(); 
// letras = ['H', 'o', 'l', 'a']

```

## append() (de StringBuilder)

StringBuilder es una clase especial para construir textos sin crear un objeto nuevo cada vez, como pasa con +=.


```jsx title=" "
StringBuilder sb = new StringBuilder();
sb.append('a');
sb.append('b');
System.out.println(sb.toString()); // imprime "ab"
``` 

## Character.toLowerCase(char c)

Convierte un carácter a minúscula (ideal para comparar sin importar mayúsculas/minúsculas)


```jsx title=" "
char letra = 'A';
System.out.println(Character.toLowerCase(letra)); // imprime 'a'

```

## reverse() (de StringBuilder)

Invierte el texto que hay en el StringBuilder.

```jsx title=""
StringBuilder sb = new StringBuilder("hola");
sb.reverse();
System.out.println(sb.toString()); // imprime "aloh"

```