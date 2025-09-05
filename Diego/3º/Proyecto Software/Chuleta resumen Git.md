- Git: Sistema de control de versiones de proyectos.
    
- Es distribuido, por lo que no hay un repositorio centralizado, sino uno por usuario.
    
- Permite ver los cambios realizados (quién, cuándo, por qué).
    
- Permite trabajar de forma paralela y eficiente.
    
- 3 elementos:
    

- Repositorio: Aparece en el .git de la raíz y guarda toda la historia del proyecto. No se accede directamente.
    
- Directorio de trabajo: Contiene una copia de una versión del repositorio y trabajas sobre él.
    
- Stage / Index: Donde vas añadiendo los cambios para el commit posterior. Permite hacer commits más organizados.
    

- En un repositorio se guardan todos los commits.
    
- Commit: Captura del estado actual de tus ficheros y directorios, con algo de info adicional (mensaje del commit).
    
- Lo ideal es hacer commit de los cambios realmente hechos, no todo el proyecto siempre.
    
- El commit apunta a su predecesor y tiene un identificador.
    
- 2 fases commit:
    

- Añadir los cambios al stage que quieras.
    
- Hacer el commit (una vez has terminado la tarea que querías realizar).
    

- Commit guarda lo que hay en el stage (commit predecesor + cambios en stage).
    
- Estados de ficheros o directorios:
    

- Untracked: Fichero nuevo.
    
- Tracked:
    

- Commited: Estaban en el commit y no han cambiado.
    
- Modified: Estaban en el último commit y han cambiado.
    
- Staged: O estaban en el último commit y se han modificado o son nuevos, y están en el stage.
    

- Ficheros ignorados. Los guardamos en el .gitignore para que no se suban a Git (confidencialidad o archivos basura).
    
- Subir archivos que no querías: Mala pata (Git guarda historia de versiones).
    
- Git es una herramienta de línea de comandos (funciona en cualquier terminal).
    
- Comandos básicos:
    

- Crear repositorio de git (si había ficheros dentro de la carpeta están untracked):  
    git init
    
- Puedes elegir nombre del directorio donde se crea:  
    git clone <url_git_repo>
    
- Para saber qué ficheros están en qué estados, se usa git status
    
- Para poner un fichero en tracked o staged(mete esa versión del fichero, si luego cambia se mantiene la versión), se hace:  
    git add <dir_fich> 
    
- Para saber diferencia entre fichero añadido a stage y el de mi directorio de trabajo:  
    git diff  
    Difícil de leer, es mejor leerlo en editor de texto (VSC por ejemplo).  
    Se pueden poner 2 parámetros (2 ids de commits de tu repo) para compararlos.
    
- Hacer commit (lo nuevo + el commit/commits anterior/es):  
    git commit [-m “Explicación commit”]
    
- Para eliminar ficheros de Git, primero eliminarlos del stage y luego hacer commit:  
    git rm (lo borra del git y del directorio de trabajo)
    
- Después de varios commits, viene bien revisar la historia de cambios:  
    git log [--graph –all –decorate –oneline]
    

- --graph: Muestra el historial como un gráfico ASCII, representando ramas y fusiones visualmente.
    
- --all: Incluye el historial de todas las ramas, no solo de la rama actual.
    
- --decorate: Añade información sobre las referencias (ramas, etiquetas, etc.) al historial mostrado.
    
- --oneline: Reduce cada entrada del historial a una sola línea
    

- Si haces un commit y te has olvidado de meter algo en él:  
    git commit -amend (añade lo del stage al último commit, puede hacerse también para cambiar mensaje de commit)
    
- Para quitar cambio del stage:  
    git restore --stage <fich>
    
- Si cambias fichero y quieres devolverlo al estado en el que estaba en el último commit:  
    git checkout -- <fich>
    
- Si ya has mandado commit a otros y quieres revertirlo, el siguiente comando añade un nuevo commit que deshace los comandos de un commit anterior:  
    git revert <commit-id> (HEAD es el último commit, HEAD-1 el penúltimo…)
    
- Otra forma de revertir commits es directamente borrarlo de la historia (solo usar si no has enviado commits al resto):  
    git reset <id-commit> (id del commit al que quiere volver y los posteriores se ignoran)  
    Borra los cambios del stage pero no modifica el directorio de trabajo.  
      
    

- Rama (branch): Nombre que apunta a un commit (generalmente el último) (va cambiando el puntero del commit). Permite acceder a parte del árbol de commits, pero no a todos generalmente.
    
- El nombre de rama por defecto en git es main (antes era master)
    
- Comando para crear rama que apunta al commit donde estamos:  
    git branch [-d] <nombre-rama>  
    -d borra la rama nombre-rama-.
    
- Comando para cambiar de rama (mover puntero HEAD a la rama que le digamos):  
    git checkout [-b] <nombre-rama>  
    -b crea la rama (te ahorras el git branch)  
    Modifica directorio de trabajo a versión tal cual estaba en ese commit de la rama.  
    Si luego hago commits, solo mueve la rama a la que apunta HEAD, main se queda donde estaba. (Moverte a muchas ramas y hacer cambios en cada una -> Distintas ramas separadas)
    
- Comando para combinar ramas:  
    git merge  
    Puede ir bien si no hay conflictos (2 cambios distintos en las 2 ramas en un fichero).  
    Si los hay, git avisa para que lo arregles tú  
    En general, la mejor estrategia para mezclar ramas es: hacer checkout a la rama MAIN y unir.  
    Lo mejor es unir ramas de 2 en 2(si quiero unir 3, uno primero 2 y luego la uno con la 3º).  
    fast-forward: Si la rama MAIN apunta a c1 y bugZ a c2, al unirlos, como c1 es ancestro de c2, simplemente muevo el puntero MAIN a c2.  
    Una cosa que se suele hacer es, al mezclar la rama MAIN con la rama ramaX, borrar la ramaX porque ya no te sirve de nada (git branch -d ramaX).
    
- Resolucion de conflictos:
    

- Si hay conflictos, no hace merge y te avisa.
    
- git status te muestra qué ficheros no han sido mergeados.
    
- Git pone marcadores en los ficheros que dan conflicto. Los pone en tu rama actual.
    
- Resolver manualmente el conflicto y luego mergear.
    

- Trabajo colaborativo:
    

- Un usuario crea repositorio
    
- Usuarios hacen git clone <url-repo>. Descargan toda la historia de commits de ese repo de GitHub. Esto crea un repo local y el de Github será remoto y se llama origin. El nombre del repo es main, pero no es el mismo que el main del repo remoto de GitHub.
    
- El comando git pull permite descargar los cambios más nuevos del repositorio remoto en tu local. Puede dar conflictos. Resolverlos tal y como se hacía en merge
    
- El comando git push sube los cambios locales al remoto.  
    HACER SIEMPRE PULL ANTES QUE PUSH!!!
    
- Generalmente, los parametros de ambas funciones son  
    git push origin main (usualmente el puntero HEAD estará en la rama MAIN)  
    git pull origin main (lo mismo)
    