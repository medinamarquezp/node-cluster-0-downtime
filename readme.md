# Alta disponibilidad con clusters de NodeJS

Uno de los problemas que se produce cuando ejecutamos una sola instancia de una aplicación Node frente a varias es que, cuando esta se bloquea, debe reiniciarse y habrá **tiempo de inactividad** entre estas dos acciones, incluso si el proceso ha sido automatizado. 

Esto también aplica a cuando el servidor debe reiniciarse para implementar nuevo código. Con una instancia, habrá un tiempo de inactividad y esto afectará a la **disponibilidad del sistema**. Cuando ejecutamos varias instancias, la disponibilidad del sistema aumenta fácilmente y esto con solo unas pocas líneas de código. 

Para simular un bloqueo en nuestro servidor, realizaremos un script que fuerce la salida de un proceso dentro de un temporizador que se activa después de un período de tiempo aleatorio. Cuando un proceso termina de esta manera, se puede notificar al maestro utilizando el evento de salida del clúster y, de esta manera, podremos registrar un controlador para ese evento en el que simplemente crearemos un nuevo proceso. 

Es buena práctica controlar esta condición para asegurarnos de que el proceso realmente se bloqueó y no fue desconectado o eliminado por el proceso maestro. Por ejemplo, el proceso maestro podría decidir que estamos usando demasiados recursos en base a la carga y esto le obligaría a matar algunos procesos workers.

Si ejecutamos el código, podremos observar que, después de una cantidad aleatoria de segundos, los workers comenzarán a fallar y el proceso maestro inmediatamente comenzará a crear nuevos workers para aumentar la disponibilidad del sistema. De hecho, podemos medir la disponibilidad usando el comando **ab** (https://httpd.apache.org/docs/2.4/programs/ab.html) para ver cuántas solicitudes no pudo gestionar el servidor. Solo 6 solicitudes fallaron de 2000 en un intervalo de unos 6 segundos con 200 peticiones simultáneas. Eso es más del 99% de disponibilidad:


