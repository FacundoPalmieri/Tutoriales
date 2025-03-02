---
sidebar_position: 1
---

# 1 - GitHub


## **Instalación de Git**

1.  Descarga el instalador de Git desde [git-scm.com.](https://git-scm.com/downloads)

2.  Ejecuta el instalador y sigue las instrucciones en pantalla.

3.  Acepta las opciones predeterminadas a menos que tengas una razón específica para cambiarlas.

4.  Una vez completada la instalación, abre la terminal de Git Bash para verificar la instalación con el siguiente comando: css git --version


### *Configurar GIT por primera vez*

1. Abrir GIT BASH 

2. git –version 

3. git config --global user.name “Juan Perez” 

4. git config --global user.email “juanperez@gmail.com” 

------------------------

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



![git-branch](/img/git-branch.png)

### *Cambiar de rama*

Con el comando **git checkout nombre-de-la-rama** podremos crear una nueva rama. 

![git-checkout](/img/git-checkout.png)


### *Unificar Ramas (merge)*

Para realizar el merge debemos primero estar parados en la rama que queremos actualizar (Main) luego usaremos el comando **git merge nombre-de-la-rama-secundaria**.

#### Desde la rama principal, llamamos a la rama secundaria. 

![git-merge](/img/git-merge.png)


```jsx title="Ejemplo"
git checkout main // Para ubicarnos en la rama principal

git merge mi-rama // unificas los cambios de la rama “mi-rama” con la Principal

```


### Eliminar rama local.
```jsx title=""
git branch -d nombre-de-la-rama
```


### Listar ramas locales y remotas
```jsx title=""
git branch -a
```

  

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

### Descargarse una rama nueva creada por un colaborador.

1. Nos posicionamos desde el **bash** en la carpeta del proyecto.

2. Verificamos en que rama estamos.

3. Verificamos las ramas que tenemos localmente.

```jsx title="Ejemplo"
git branch
```

4. verificamos las ramas que tenemos remotas.

```jsx title="Ejemplo"
git branch -r
```

5. Si no existe nuestra rama remota de manera local, debemos crearla.

```jsx title="Ejemplo"
git checkout -b nombre-de-la-rama origin/nombre-de-la-rama


primero crear localmente el nombre de la rama y luego vincula y hace un pull con la rama remota
```

6. Cambiamos de rama

```jsx title="Ejemplo"
git checkout nombre-de-la-rama
```
<br/>

### Crear una nueva rama local y remota

1. Crear la nueva rama localmente:

```jsx title="Ejemplo"
git checkout -b nueva-rama-backend
```

<br/>

2. Realizar tus cambios y hacer commit:

```jsx title="Ejemplo"
git add .                 # Agrega los archivos modificados
git commit -m "Tu mensaje de commit"   # Realiza el commit
```

<br/>

3. Subir la rama a GitHub (remoto):

**git push** → Envía los cambios de una rama local a un repositorio remoto.

**origin**→ Es el nombre del repositorio remoto vinculado a tu repositorio local. Cuando clonas un repositorio desde GitHub, Git automáticamente asigna el nombre origin al repositorio de GitHub.

**nueva-rama-backend** → Es el nombre de la rama local que quieres subir al repositorio remoto.

```jsx title="Ejemplo"
git push origin nueva-rama-backend

```

<br/>

### Actualizar ramas locales si alguna remota fue mergeada.

1. Nos paramos siempre en la rama principal

```jsx title="Ejemplo"
git checkout main
```

2. Descargamos los cambios.

```jsx title="Ejemplo"
git pull origin main
```

3. Limpiar referencias a ramas eliminadas en GitHub
```jsx title="Ejemplo"
git fetch --prune
```

4. Verificar las ramas remotas.
```jsx title="Ejemplo"
git branch -r
```

5. Eliminar la rama local si aún existe

```jsx title="Ejemplo"
git branch 

git branch -d nombre-de-la-rama
```

6. Confirmar que las ramas locales y remotas coincidan.

```jsx title="Ejemplo"
git branch -a
```


## **Comandos**

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





### *Commit*

1. git commit –m “comentario” (realizas el checkpoint)

2. git log (muestra los commit hechos)  

3. git log --oneline (lista los commits)


NO USAR: 
4. git checkout +id commit (vuelve al commit seleccionado) 




### *Enlazar el repo local con GitHub*


#### 1. Ver el nombre de la rama principal.

```jsx title="Ejemplo"
git branch 
```
<br/>

#### 2. Agrega un repositorio remoto a tu proyecto local. (origin es el nombre que Git usará para referirse a ese repositorio remoto)
```jsx title="Ejemplo"
git remote add origin   https://…    (aca iria el link del repositorio)
```
<br/>

#### 3. Renombra la rama actual a main.
```jsx title="Ejemplo"
git branch -M main // REVISAR COMO SE LLAMA LA RAMA PRINCIPAL ANTES 
```
<br/>

#### 4. Sube la rama main al repositorio remoto (origin).

-u (--set-upstream) establece un rastreo entre la rama local y la remota, para que en futuros git push o git pull no tengas que especificar el origen y la rama cada vez.

```jsx title="Ejemplo"
git push -u origin main 
```

