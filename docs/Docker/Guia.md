---
sidebar_position: 1
---

# 1 - Docker

## ¿Qué es Docker?

Docker es una tecnología que te permite armar, administrar y desplegar/deployar aplicaciones de manera mucho más fácil mediante el uso de contenedores. 


## ¿Qué es un contenedor?

Un contenedor de Docker es como un paquete completo y aislado que incluye una aplicación junto con todas las cosas necesarias para que funcione sin problemas.


## ¿Qué es una imagen?

La imágen es el resultado de la compilación del dockerFile.

Una imagen de Docker es una plantilla que contiene todas las dependencias y configuraciones necesarias para ejecutar una aplicación/servicio en un contenedor. 


La imagen contiene el código que se ejecutará y cualquier otra configuración necesaria.

Un contenedor de Docker es una imagen instanciada (en ejecución). Las imágenes de Docker se crean a partir de un tipo de archivo especial llamado Dockerfile. Las imágenes se almacenan en un registro de imágenes (como Docker Hub) y se pueden compartir y reutilizar fácilmente.

## ¿Archivo Dockerfile?
Es un archivo de texto plano que contiene un conjunto de instrucciones que se utilizan para construir una imagen de Docker. 

El Dockerfile especifica cómo se debe configurar el entorno dentro de la imagen, qué aplicaciones y bibliotecas deben estar presentes y cómo se deben realizar tareas específicas, como copiar archivos, configurar variables de entorno y definir comandos para ejecutar cuando se inicie un contenedor basado en esa imagen.

## ¿Archivo docker-compose.yml?
Docker Compose es una herramienta que se utiliza para definir y ejecutar aplicaciones Docker multi-contenedor. El archivo docker-compose.yml es un archivo de configuración en formato YAML que especifica cómo se deben ejecutar varios contenedores Docker juntos como una aplicación. Permite definir la configuración de cada servicio, las redes, los volúmenes y otras opciones necesarias para ejecutar la aplicación de manera coherente.