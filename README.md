PRÁCTICA 2: PONG

Miembros: Adrià Álvarez y Martí Calvo

Introduccion:
En esta práctica hemos completado la plantilla de un juego de Ping Pong en Python utilizando Pygame. Hemos implementado las funciones que faltaban y hemos hecho que se comuniquen entre ellas mediante parámetros y valores de retorno. Hemos mantenido los nombres y los parámetros que ya aparecían en la plantilla sin utilizar clases, listas ni diccionarios.

El juego tiene dos palas y una pelota. El jugador de la izquierda mueve su pala con las teclas A y Z. En nuestra versión, la pala de la derecha la controla el ordenador, porque hemos dejado activada la opción PLAY_AGAINST_MACHINE. Los puntos se van sumando cuando la pelota sale por uno de los lados y la partida acaba cuando un jugador llega a la puntuación indicada en POINTS_END.

Configuración:
La función get_config() pregunta si queremos cambiar algunos valores del juego, como la altura, la velocidad de las palas, la velocidad de la pelota o su radio. Si respondemos que no, utiliza los valores que ya vienen definidos de por si. Si respondemos que sí, pide cada valor con read_int_in_range(), para evitar que el usuario utilice números fuera del intervalo permitido. Al final devuelve los cinco valores en el orden que espera main().
 
change_colors() hace algo parecido, pero con los colores. Si no queremos cambiarlos, devuelve los colores por defecto. Si queremos personalizarlos, llama a read_color() una vez para el fondo y otra para los elementos del juego como las palas o la pelota. Hemos añadido una comprobación para que los dos colores no sean iguales, porque en ese caso sería difícil distinguir la pelota y las palas del fondo. 

read_color(prompt) recibe el nombre del color que se está configurando y pide sus componentes rojo, verde y azul. Cada componente se lee con read_int_in_range() y debe estar entre 0 y 255. La función devuelve los tres números juntos en una tupla.

Funciones para validar lo que escribe la persona que juega:
read_int_in_range(prompt, low, high) repite la pregunta hasta que recibe un número entero dentro del intervalo. Convertimos la entrada con int() dentro de un bloque try-except. Si se escriben letras o un decimal, se produce un ValueError y sale un mensaje y se repite la pregunta. Si el número es entero, pero queda fuera del intervalo, también se muestra un aviso.

ask_yes_no(prompt) se utiliza para las preguntas de sí o no. Antes de comprobar la respuesta, usamos strip() para quitar los espacios al principio y al final, y lower() para pasar el texto a minúsculas. Así también se puede poner “y, yes, n o no", incluso si se escriben en mayúsculas. Si la respuesta no es válida, la función vuelve a preguntar.

read_int_in_range_w_default() es una función opcional que hemos hecho como una alternativa a la lectura normal de enteros. Tiene la misma validación de rango, pero si pulsas el ENTER se utiliza el valor predeterminado. Si el usuario escribe un valor incorrecto, se repite la pregunta.

Movimiento de las palas y la pelota:
move_paddle() recibe la posición actual de una pala y la dirección en la que debe moverse. Cuando la dirección es -1, resta la velocidad a la coordenada vertical; cuando es +1, la suma. Después comprueba que la pala no se salga de la pantalla. Para ello, el valor mínimo de “y” es 0 y el máximo es la altura de la pantalla menos la altura de la pala. La función devuelve la nueva posición.

move_ball() suma “vx” a la coordenada “x” y “vy” a la coordenada “y”. Devuelve las dos nuevas coordenadas.
collide_ball_walls() comprueba si la pelota toca la parte superior o inferior de la ventana. Como la posición “x” e “y” de la pelota corresponde al propio centro de la pelota, tenemos en cuenta el radio para calcular sus extremos. Si toca una pared, corregimos la coordenada “y” para que no quede fuera y cambiamos el signo de “vy”.

Colisiones de las palas:
collide_ball_paddle() consiste en tratar la pelota como si fuera un cuadrado pequeño y comprobar si ese cuadrado se solapa con el rectángulo de la pala. Primero calculamos los bordes izquierdo, derecho, superior e inferior de la pelota usando su centro y su radio. Después hacemos lo mismo con la pala usando su posición, anchura y altura.

Si los rectángulos se solapan, la función considera que ha habido un choque. En ese caso coloca la pelota fuera de la pala y cambia el signo de “vx” para que rebote horizontalmente. También devuelve hit como True para que main() sepa que debe actualizar las velocidades. Si no hay choque, devuelve hit como False y mantiene los valores de movimiento. 

Reinicio de la pelota y pala automática:
reset_ball() coloca la pelota en el centro de la pantalla. El parámetro to_right indica hacia qué lado debe salir: si es True, la velocidad horizontal es positiva; si es False, es negativa. También se elige una velocidad vertical aleatoria cercana a vy0 y se cambia su signo al azar.

compute_ai_direction() es la otra función opcional que hemos implementado. Calcula el centro de la pala derecha y lo compara con la altura de la pelota. Si la pelota está bastante por encima, devuelve -1 para que la pala suba; si está por debajo, devuelve +1 para que baje. Si la pelota está cerca del centro, devuelve 0 y la pala se queda quieta. Hemos utilizado un margen de 10 píxeles para que no cambie de dirección constantemente cuando la pelota y la pala están casi alineadas.

Organización de la partida:
La función main() conecta todas las partes. Primero llama a las funciones de configuración y color, y después inicia Pygame y crea la ventana. Coloca en la posición inicial las palas y la pelota y pone los dos marcadores a cero. 

Dentro del bucle principal se revisa si el usuario ha cerrado la ventana y se leen las teclas. La pala izquierda recibe la dirección indicada por el teclado. Como hemos activado el modo contra la máquina, la dirección de la pala derecha se calcula llamando a compute_ai_direction(). A continuación se actualizan las posiciones, se comprueban los rebotes con las paredes y se comprueba si la pelota choca con alguna pala. 

Si la pelota sale por la izquierda, suma un punto el jugador de la derecha; si sale por la derecha, suma un punto el jugador de la izquierda. Después de una pausa, la pelota vuelve al centro. Cuando uno de los marcadores llega a POINTS_END, se muestra el mensaje de final de partida y el bucle termina.

Declaración IA:
Hemos ido intercalando el uso de Chat GPT y Claude como apoyo para revisar el código frente a las instrucciones de la práctica, comentar problemas que nos sucedían con el código cuando ya no había otra alternativa y preparar un primer borrador de este informe con las ideas principales. También hemos activado el autocompletado de GitHub Copilot en Visual Studio Code para orientarnos mientras íbamos creando nuestro propio código.

Conclusión:
Con esta práctica hemos aprendido a utilizar de manera más seria el Visual Studio Code y el GitHub, hemos trabajado cómo dividir un programa en funciones pequeñas, pasarles los datos que necesitan y utilizar los valores que devuelven. También hemos practicado funciones y hemos aprendido a separar el movimiento, las colisiones y la configuración ha ayudado a que el código sea más fácil de seguir y de revisar. 