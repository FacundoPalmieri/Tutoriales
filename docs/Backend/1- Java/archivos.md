---
sidebar_position: 8
---

# 8 - Archivos

El patrón que suele usarse:

1. Definir la ruta 

```jsx title="Main"

Path ruta = Paths.get("...");

```


2. Verificar si no existe y crear ruta.

```jsx title="Main"
if(!Files.exists(ruta)){
  Files.createDirectories(ruta)
}
```


3. Crear archivo dentro 

```jsx title="Main"

Path pathArchivo = ruta.resolve("nombreArchivo");

```


4. Escribir contenido en docx

```jsx title="Main"
//Para pruebas
byte[] archivoBytes = "hola".getBytes();
Files.write(archivoRuta, archivoBytes);


// Obtener los bytes del MultipartFile y escribirlos en disco
Files.write(pathArchivo, file.getBytes());

```


5. Guardar PDF

```jsx title="Main"


 // Redimensionar y guardar (300x300, calidad 0.8)
        Thumbnails.of(file.getInputStream())
                .size(300, 300)
                .outputFormat("jpg")
                .outputQuality(0.8)
                .toFile(filePath.toFile());

```

6. Obtener documento fisico 


```jsx title="Main"
 public UrlResource getDocument(AttachedFile file) throws IOException {

        Path path = Paths.get(uploadDirDocument, file.getStoredFileName());
        if (!Files.exists(path)) {
            throw new NotFoundException("exception.file.attachedFileNotFound.user", null, "exception.file.attachedFilesNotFound.log", new Object[]{file.getStoredFileName(), "FileStorageService", "getDocument"}, LogLevel.ERROR);
        }

        return new UrlResource(path.toUri());
    }
```





5. Leer contenido 
```jsx title="Main"

Files.readAllBytes(pathArchivo);

```



6. Borrar 

```jsx title="Main"

Files.delete(path);

```

7. Obtener nombre de archivo

Dependiento de cliente puede venir el nombre del archivo o la ruta completa, por eso accedemos de esta manera.

```jsx title="Main"

String originalFilename = Paths.get(file.getOriginalFilename()).getFileName().toString();

```
