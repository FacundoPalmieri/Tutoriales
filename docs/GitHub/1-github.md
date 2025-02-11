---
sidebar_position: 1
---

# 1 - GitHub


## **Instalación de Git**

1.  Descarga el instalador de Git desde [git-scm.com.](https://git-scm.com/downloads)

2.  Ejecuta el instalador y sigue las instrucciones en pantalla.

3.  Acepta las opciones predeterminadas a menos que tengas una razón específica para cambiarlas.

4.  Una vez completada la instalación, abre la terminal de Git Bash para verificar la instalación con el siguiente comando: css git --version


### *Configuración inicial y primer commit*

1. Ingresamos desde Visual Studio Code a nuestro proyecto.
:::info
Ingresar a la carpeta del proyecto, no a la carpeta que contiene la carpeta del proyecto.
:::

2. Dentro del proyecto, abrimos la terminal y seleccionamos el bash desde la misma.

![bash](/img/git-bash.png)


3. Inicializamos Git con el comando :  **git init**

![init](/img/git-init.png)



4. Creamos la carpeta **.gitignore**, a fin de especificar que archivos/carpetas no queremos que se suban.

![git-ignore](/img/git-ignore.png)

:::tip
**Carpetas:** Se coloca "Nombre/" -  Ej: Wireframe/

**Documento:** Se coloca nombre + "extensión" - Ej: doc.txt

**Extensión:** *."extensión" -  Ej:  *.txt
:::

![git-ignore2](/img/git-ignore2.png)



5. Verificamos que reconozca que existan archivos sin agregar con el siguiente comando: **git status**

![git-status](/img/git-status.png)

6. Agregamos los archivos a Git con el siguiente comando: **git add .**


![git-add](/img/git-add.png)

7. Realizamos nuestra versión del proyecto con el siguiente comando **git commmit -m"mensaje descriptivo "**

![git-commit](/img/git-commit.png)


8. Creamos nuestro repositorio remoto desde: **https://github.com/**

9. Vinculamos el repositorio local con el remoto y subimos la versión al servidor

![git-vincular-repo](/img/vincular-repo.png)

![git-vincular-repo2](/img/vincular-repo2.png)

10. Verificación desde el servidor.


## **Branches**

Las ramas (o branches) en Git son una característica fundamental que permite trabajar en diferentes versiones de un proyecto de manera aislada, sin afectar el trabajo principal o la "rama principal". Esta funcionalidad es clave para el desarrollo de software colaborativo y para realizar cambios sin interferir con la versión estable del proyecto.

### *¿Qué es una rama?*

Una rama en Git es una copia de la línea de tiempo del proyecto en un punto específico, lo que permite realizar cambios de forma independiente. Git mantiene el registro de cada cambio realizado en una rama, y puedes trabajar en varias ramas simultáneamente.

**Rama principal:** La rama principal, generalmente llamada main o master, es la versión estable del proyecto. Todos los cambios importantes deben ser fusionados (merge) en esta rama cuando se considera que están listos para ser liberados.

**Ramas de características:** Se crean para desarrollar nuevas funcionalidades o realizar cambios sin afectar la rama principal. Estas ramas son temporales y generalmente se eliminan después de fusionarlas con la rama principal.


### *¿Por qué usar ramas?*

**Trabajo aislado:** Se puede trabajar en una característica o corrección de errores sin que afecte el trabajo de otros desarrolladores.

**Desarrollo paralelo:** Varios desarrolladores pueden trabajar en diferentes ramas sin interferir entre sí.

**Pruebas y experimentación:** Puedes probar nuevas ideas sin comprometer el código estable en la rama principal.

**Mejor control de versiones:** Cada rama mantiene su propio historial de cambios, lo que facilita la gestión de versiones.

### *Cambiar nombre a una rama*

git branch -m nuevo-nombre

### *Crear ramas*

Con el comando **git branch nombre-de-la-rama** podremos crear una nueva rama. Luego podremos verificar si la misma fue creada con **git branch -l**


![git-branch](/img/git-branch.png)

### *Cambiar de rama*

Con el comando **git checkout nombre-de-la-rama** podremos crear una nueva rama. 

![git-checkout](/img/git-checkout.png)


### *Unificar Ramas**

Para realizar el merge debemos primero estar parados en la rama que queremos actualizar (Main) luego usaremos el comando **git merge nombre-de-la-rama-secundaria**.

#### Desde la rama principal, llamamos a la rama secundaria. 

![git-merge](/img/git-merge.png)

### *Flujo común de trabajo con ramas*

1. Se crea una nueva rama para trabajar en una característica o corrección.

```jsx title=""
git checkout -b nueva-funcionalidad

```

2. Se trabaja en esa rama, realizando cambios, commits y pruebas.

```jsx title=""
git add .
git commit -m "Agregada nueva funcionalidad"
```


3. Una vez que el trabajo en la rama está listo, se fusiona (merge) de nuevo a la rama principal

Cambiar a la rama principal y fusionar los cambios:

```jsx title=""
git checkout main
git merge nueva-funcionalidad

```


4. Subir los cambios al repositorio remoto

```jsx title=""
git push origin main
```



## **Comandos**

### *Configurar GIT por primera vez*

1. Abrir GIT BASH 

2. git –version 

3. git config --global user.name “Juan Perez” 

4. git config --global user.email “juanperez@gmail.com” 


### *Comandos para chequear información*

1. git config --list 

2. git config user.name .

3. git config user.email 



### *Eliminar un commit REMOTO pero mantener la versión LOCAL*

1.  git reset --soft HEAD~1

2.  git push origin main --force

### *Otros*

4. git help config (Abrirá ayuda de Git)

5. git diff index.html (antes de guardar con el git add, te muestra los cambios o la diferencias entre el antes y después en el documento)

6. git restore index.html (si los cambios mostrados arriba no quiero guardarlos y prefiero el estado del commit anterior)


### *Crear repositorio*

1. Click derecho sobre la carpeta, “Git bash here” (o trabajar la terminal desde el visual)

2. git init (working directory) - Inicializas git en el proyecto

3. git status (aparecerán los archivos en rojo) - Aparecen los cambios guardados o no del proyecto

4. git add index.html (staging area) - Agregas archivo por archivo

5. git add . (agregá todos los archivos)

6. git status (aparecerá los archivos guardados en verde) 

7. git commit -m “creación del index” (generas el checkpoint)


### *ramas*

1. git branch mi-rama (Creas la rama “mi-rama”)

2. git branch -l (muestra las ramas existentes) 

3. git checkout mi-rama (se mueve de rama, de master hacia “mi-rama”) 

4. git checkout -b nombre-de-la-rama (Crear y cambiar a una nueva rama en un solo paso)

5. git branch -d mi-rama (elimina la rama “mi-rama”) 


### *Commit*

1. git commit –m “comentario” (realizas el checkpoint)

2. git log (muestra los commit hechos)  

3. git log --oneline (lista los commits)


NO USAR: 
4. git checkout +id commit (vuelve al commit seleccionado) 


### *Merge*

1. git checkout master (para ubicarnos en la rama master o main) 

2. git merge mi-rama   (unificas los cambios de la rama “mi-rama” con la “master”)


### *Enlazar el repo local con GitHub*
 
git remote add origin   https://…          👈🏻 (aca iria el link del repositorio)

git branch -M main                         👈🏻 (aca revisen si su rama principal se llama “main” o “master” y sustituir para que coincida y no les cree otra, todo esto antes de tirar ese comando)


git push -u origin main                     👈🏻(al igual que arriba chequear que coincida con su repo local el main o el master (puede variar))
