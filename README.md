# cursoGIT

Primero debemos de configurar el _**nombre de usuario y correo**_ de manera local en el equipo, para esto hacemos uso de los siguientes comandos:
```sh
git config --global user.name "Nombre"
git config --global user.email "Correo"
```
### Comandos GIT en OH-MY-ZSH
Agregar archivos a staggin

|Atajo | Comando Largo | Comentario |
| --   | -------- | ---- |
| ga   | git add <nombre_archivo> | Agrega el archivo al stagging
| gaa  | git add --all | Agrega todos los archivos al staggin

**Branch** (_Ramas_)
|Atajo  | Comando Largo                 | Comentario |
| --    | --------                      | ---- |
|gb     |  git branch                   | Muestra ramas locales
|gba    | git branch -a                 | Muestra **TODAS** las ramas
|gb     | git branch <nombre_rama>      | Crea rama local a partir de Master
|gbd    | git branch -d <nombre_rama>   | Elimina una rama
|gco -b | git checkout -b <nombre_rama> | Crea rama y se mueve a ella
|gsw -c | git switch -c <nombre_rama>   | Crea rama y se mueve a ella
|gsw -  | git switch -                  | Cambia a la rama anterior

**Commit**
|Atajo | Comando Largo | Comentario |
| -- | -------- | ---- |
|gcmsg | git commit -m "Mensaje" |
|gc! -m |git commit --amend -m "Mensaje" |Actualiza mensaje del commit anterior

Con el siguiente comando, el **head** va apuntar al commit anterior, restaurando los cambios
_git reset --soft HEAD^_

--Se puede modificar el archivo--


######### USAR PARA ELIMINAR ALGUNOS COMMIT, MIXED 
git reset --mixed <id_commit> //Va apuntar hacia el id_commit SIN borrar los archivos nuevos, solo los saca del staged
git reset --hard <id_commit> //Va colocar el HEAD en ese punto

 Checkout
gcm  git checkout $(git_main_branch)
gco  git checkout

grh  git reset HEAD <Nombre del archivo> //Sacar del staged y mandar a unstaged
//NECESITA ESTAR EN UNSTAGED 
gco -- <Nombre del archivo>//Regresa los archivos al commit anterior


 Log
glg  git log --stat
glgg git log --graph
glo  git log --oneline --decorate
glog git log --oneline --decorate --graph
gloga git log --oneline --decorate --graph --all

 Status
gsb  git status -s -b (Short y Branch)
gst  git status
gss  git status -s

#ELIMINAR COMMIT O APUNTAR HACIA COMMIT ANTERIOR
git reset --hard <id_commit>

#VOLVER A UN COMMIT FUTURO O ELIMINADO
git reflog
Copiamos id_commit
git reset --hard <id_commit>

##ETIQUETAS (TAGS)
Si queremos colocar a etiquetas a versiones alpha, beta, release, etc, se hace de la siguiente manera:

git tag -a <version> -m <Mensaje sobre la version" //La version puede ser tanto en numero como letras ejemplo: v1.0.1, Alpha, etc.

Esto le colocara la etiqueta al ultimo commit, en cambio si queremos etiquetar un commit pasado debemos de colocar lo siguiente

git tag -a <version> <id_commit> -m <Mensaje sobre la version"

gtv  = git tag | sort -V //Nos mostrara las etiquetas que hay en el repositorio

##MERGE
Traera los cambios y commits de la rama especificada hacia la que estamos parados
git merge <nombre_rama>
gm   = 'git merge'
gmom = 'git merge origin/$(git_main_branch)'

##STASH
Sirve para dejar un trabajo (de la rama) pendiente por algo de emergencia, se usa de la siguiente manera:
Si queremos un Stash espcifico indicar "stash@{n}" despues del comando drop, apply, pop.

gsta git stash //Esto manda los ultimos cambios guardados al stash (pueden existir varios) se crea un WIP (Work In Progress)
gsta -m "Mensaje"
Se trabaja en lo urgente, se hace el commit de lo urgente.

::LIST
Para ver la lista de los stash que existen hacemos lo siguiente:
gstl git stash list

Despues para volver a unir los cambios que ya habiamos realizado hacemos lo siguiente:

::POP (BORRA EL STASH)
gstp git stash pop //Esto traera los cambios que existen entre el ultimo WIP y del HEAD del commit:

1. NO HAY CONFLICTOS: Hara el merge sin problema y BORRARA el stash.
2. SI HAY CONFLICTOS: Si hay conflictos entre archivos se mostraran, se corrigen pero hay que ELIMINAR el stash con drop.

::APPLY (NO BORRA EL STASH)
gstaa git stash apply //Trae los cambios en el ultimo stash y los une con el head de la lista de commit
gstaa "stash{n}" //Trae los cambios del stash{n} y los une con el head de la lista de commit

::DROP
Para eliminar los stash se puede hacer con lo siguiente:
gstd git stash drop //Elimina el stash{0}
gstd "stash{n}" //Elimina el stash{n}

::CLEAR
gstc git stash clear //Elimina todos los stash que existan

::SHOW
 El comando show nos muestra información acerca de los cambios
 gsts git stash show //Muestra diferencias de manera corta
 gsts -p //Muestra todas las diferencias

::Untracked Files
Poedemos hacer stash a archivos mixtos es decir con y sin seguimiento, para esto hacemos uso de:
gstu git stash --include--untracked
gstu -m "Mensaje"

#REBASE
Sirve para actualizar (moviendola) una rama derivada con respecto a otra con la diferencia de commits (en caso de que existan cambios/commits sobre nuestra linea)
**Se usa en primera instancia cuando existen commits sobre la misma linea de tiempo, en lugar de un merge.
 
grb  = git rebase
grbm = git rebase $(git_main_branch) //Hace un rebase a la rama principal

Git rebase interactivo
grbi = git rebase -i
grbi = git rebase -i HEAD~3
//Donde head apunta hacia el ultimo commit(puede ser otro commit con su hash) y el numero despues de ~ indica cuantos commits antes para que se haga el rebase.

Una vez haciendo el rebase, continuamos con los trabajos de nuestra rama, al final cuando queramos unir los cambios haremos un merge en la rama maestra
gcm --> gm <rama_de_trabajo>

Funcion principal del rebase:
*Ordenar commits
*Corregir mensajes de los commits
*Separar commits
*Unir commits

::SQUASH (Unir 2 commits)
El squash nos va a servir para poder unir 2 commits en uno solo, conservando la información, para esto hacemos primero uso del siguinte comando:
grbi head~n //Donde n indica cuantos commits atras queremos

 Se abrira una pantalla con algo similar a esto:
   ::Veremos los commits que indicamos con el head~4 
    pick 300c014 Misiones nuevas agregadas
    pick 158ba9e Se agrego a la liga: Volcán Negro
    pick 19cea51 Agregamos el archivo de las misiones completadas
    s ea53ed0 Actualizamos la informacion de las misiones completadas

   ::De acuerdo a los comandos de abajo tenemos que modificar la palabra "pick" arriba segun lo que queramos hacer, por ejemplo si elegimos squash, borramos pick y ponemos squash o solo la s 
    # Rebase acea380..ea53ed0 onto acea380 (4 commands)
    #
    # Commands:
    # p, pick <commit> = use commit
    # r, reword <commit> = use commit, but edit the commit message
    # e, edit <commit> = use commit, but stop for amending
    # s, squash <commit> = use commit, but meld into previous commit
    # f, fixup <commit> = like "squash", but discard this commit's log message
    # x, exec <command> = run command (the rest of the line) using shell
    # b, break = stop here (continue rebase later with 'git rebase --continue')
    # d, drop <commit> = remove commit
    # l, label <label> = label current HEAD with a name
    # t, reset <label> = reset HEAD to a label
    # m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
    # .       create a merge commit using the original merge commit's
    # .       message (or the oneline, if no original merge commit was
    # .       specified). Use -c <commit> to reword the commit message.
    #
    # These lines can be re-ordered; they are executed from top to bottom.

    Al finalizar damos enter, nos mandara a otra pantalla, donde vamos a tener que modificar el mensaje del commit.

::REWORD
Con este comando podremos renombrar un commit
grbi head~n o id_commit

Cambiamos la palabra pick por reword, salimos y modificamos el mensaje.

::EDIT
(Util para separar un commmit en varios commits)
Despues podemos editar los archivos que se hicieron cambios en el ultimo commit
grbi head~1 

Entramos y cambiamos la palabra pick por e o edit, salimos y hacemos lo siguiente:
git reset head^ //Volveremos al estado antes del commit y podemos editar los archivos.

Editamos los archivos ya sea juntos o por separado, si lo hacemos por separado los agregamos con git add <Nombre del archivo>
gcsm = git commit -s -m  //Para agregar solo el archivo

Cuando terminemos de hacer los commits por archivos haremos lo siguiente para volver a Master

grbc = git rebase --continue

##RENOMBRAR ARCHIVOS FUERA DE GIT
Si cambiamos el nombre de un archivo fuera de git y hacemos un gss veremos dos archivos
 D historia/superman.historia.md //original BORRADO
?? historia/superman.md          //renombrado

Tenemos que hacer un git add -u (gau) para actualizar los archivos, despues un git add -A (gaa), si volvemos a hacer un git status veremos que ya lo marca como renombrado
R  historia/superman.historia.md -> historia/superman.md

#RECUPERAR ARCHIVOS BORRADOS DE GIT
Para recuperar un archivo ya eliminado del directorio, que haya pasado por
stagging area, usamos el siguiente comando para buscar archivos eliminados en git

git log --oneline --name--status --diff-filter=D

Despues copiamos el identificador del commit que queramos recuperar
y escribimos el siguiente comando:

git checkout <id_commit>^ <nombnre_directorio> o <nombre_archivo>

#CREAR NUEVOS ALIAS
Revisar alias globales creados
git config --global -e (editar)
git config --global -l (mostrar)

Para crear alias globales escribimos lo siguiente:
git config --global alias.<nombre del alias> "Comando que deseamos hacer alias"
ejemplo:
git config --global alias.logp "log --oneline --pretty --all --graph"
para usarlo:
git logp

----GITHUB-----
##CLONAR REMOTO
Para clonar un remoto nos dirigimos a la ubicacion donde deseamos almacenar los archivos y hacemos lo siguiente:
git clone <URL_REPO_GITHUB> //Esto traera todo el contenido y creara una carpeta con el nombre del repositorio
git clone <URL_REPO_GITHUB> <Nombre_Carpeta> //Guardara todo el contenido en la carpeta 

##AGREGAR REMOTO
gra  = git remote add <Nombre_remoto> <URL_Github> //Por lo general el remoto siempre se llama 'origin'

::CHECAR RAMAS REMOTAS
gr   = git remote //Muestra el nombre de todos los remotos
grv  = git remote -v //Muestra ramas remotas (fetch/push)

##PUSH (Mandar cambios locales al remoto)
gp   = git push
git push -u <Nombre_remoto> <nombre_rama> //Define la rama que siempre se hara push (Para solo usar git push)

::ENVIAR TODAS LAS RAMAS AL REMOTO
git push -u <NOMBRE DEL REMOTO> --all

::ENVIAR TODOS LOS TAGS A GITHUB
git push --tags

Un comando para las 2 acciones de arriba:
gpoat = 'git push origin --all && git push origin --tags'

::ELIMINAR RAMAS REMOTAS
git push <NOMBRE DEL REMOTO> :<NOMBRE DE LA RAMA> (1ra Manera)
git push -d <NOMBRE DEL REMOTO> <NOMBRE DE LA RAMA> (2da Manera)

Se puede renombrar rama principal remota desde los ajustes de github, despues se puede eliminar la rama principal, verificar que el head no apunte hacia el main con:

git remote show <NOMBRE DEL REMOTO>

# PULL 
Bajar cambios/Actualiza los archivos desde Github y hace MERGE con nuestros cambios, si hay commits adelante
gl  = git pull 

# FETCH
Solo baja los cambios, sera necesario hacer merge despues
gf  = git fetch




 