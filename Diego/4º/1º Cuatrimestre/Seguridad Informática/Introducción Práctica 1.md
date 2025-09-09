En las diapositivas, hay una funcion que revisa las variables de entorno y si contiene '() {}', lo ejecuta (se supone que la idea es definir la funcion que el padre quiere exportar).
foo = '() { comando1; }'
Esto hace que se ejecute foo() { comando1; }

El principal problema radica en que como solo tiene que reconocer los primeros caracteres, lo de despues no lo revisa, y puede ocurrir esto:
foo = '() { comando1; }; comandoExtra;'
Esto hace que se ejecute lo siguiente:
 foo() { comando1; }; comandoExtra;
 
![[Pasted image 20250909102956.png]]
 
 Por lo tanto, no se tiene un control de los posibles comandos extra que se incluyan en la variable de entorno que se ejecutarían en el hijo.
 
![[Pasted image 20250909103514.png]]

En esta segunda imagen lo que ocurre es que el system("/bin/ls -l") crea un hijo y el bash del hijo procesa las variables de entorno. Entonces, como el foo que se ha exportado como variable de entorno contiene un comando extra (/bin/sh), el bash lo detecta y ejecuta (por tanto, hay una vulnerabilidad).


Esto aplicado a CGI tambien tiene problemas:
![[Pasted image 20250909104654.png]]

Al ejecutar la funcion curl, se mandan una serie de variables de entorno al Apache donde llega la peticion URL. Entre esas variables de entorno se encuentra User-Agent, que puede ser modificado. Por lo tanto, se podrian tambien incluir ahi comandos extra que no se controlan, por lo que es vulnerable.

![[Pasted image 20250909104801.png]]

El flag -A modifica el User-Agent, por lo que puede ponerse lo que sea. Ejemplo de ataque:

![[Pasted image 20250909104914.png]]

Otro uso posible es para acceder a directorios a los que supuestamente no se tiene acceso para acceder a información (por ejemplo contraseñas).

![[Pasted image 20250909111059.png]]

Una tecnica que emplean tambien los atacantes es el Reverse Shell, que consiste en crear un shell en una maquina remota . De esta forma, se puede crear un shell remoto a la maquina a la cual haces curl (meter ese comando como extra a la variable de entorno del CGI como se ha explicado antes) y dentro crear puerto de escucha remota en el propio servidor. De esa forma, se puede acceder de forma remota al servidor al cual te conectas mediante curl y puedes hacer cualquier cosa.

![[Pasted image 20250909111321.png]]

De esta forma se crea el reverse shell (crear un shell)

![[Pasted image 20250909111520.png]]

El atacante accede maliciosamente a la URL del servidor y ejecuta un shell remoto hacia la maquina del atacante en el puerto 9090. Como el atacante estaba escuchando en el puerto 9090, llega el shell remoto desde el servidor. Por lo tanto, en la maquina del atacante tienes un shell que proviene del servidor y desde la maquina del atacante puedes ejecutar comandos que se ejecutarán en la máquina del servidor, lo cual provoca una vulnerabilidad.