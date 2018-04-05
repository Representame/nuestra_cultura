# Git

¿No sabes que es Git? Te invitamos a leer el siguiente articulo de [Wikipedia](https://es.wikipedia.org/wiki/Git).

Te podemos contar que Git fue creado para aumentar la confianza y eficiencia en el mantenimiento de versiones de aplicaciones, esto considerando la cantidad de archivos de código fuente que tienen, por lo tanto, Git proporciona un conjunto de herramientas para manejar un trabajo en equipo de manera rapida e inteligente.

Algunas de las características más importantes de Git son:

* Rapidez en la gestión de ramas, debido a que Git nos dice que un cambio será fusionado mucho más frecuentemente de lo que se escribe originalmente.

* Gestión distribuida; Los cambios se importan como ramas adicionales y pueden ser fusionados de la misma manera como se hace en la rama local.

* Gestión eficiente de proyectos grandes.

* Realmacenamiento periódico en paquetes.


## Ordenes básicas

* Iniciar un repositorio vacío en una carpeta específica.
```shell
$git init
```
* Añadir un archivo especifico.
```shell
$git add “nombre_de_archivo”
```
* Añadir todos los archivos del directorio
```shell
$git add .
```
* Confirmar los cambios realizados. El “mensaje” generalmente se usa para asociar al commit una breve descripción de los cambios realizados.
```shell
$git commit –am “mensaje”
```
* Revertir el commit identificado por "hash_commit"
```shell
$git revert “hash_commit"
```
* Subir la rama(branch) “nombre_rama” al servidor remoto.
```shell
$git push origin “nombre rama”
```
* Mostrar el estado actual de la rama(branch), como los cambios que hay sin hacer commit.
```shell
$git status
```

### Acá puedes ver nuestro [git-flow](/agreements/gitflow.md)
