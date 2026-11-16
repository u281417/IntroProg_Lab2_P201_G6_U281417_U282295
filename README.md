INFORME DEL LABORATORIO 2 - PONG

Contribución:
Adrià Álvarez se encargó de la lógica y la física del juego: el movimiento de las palas y de la pelota, las colisiones con las paredes y las palas, el reinicio de la pelota y el movimiento de la pala automática. Para ello, implementó las funciones move paddle(), move_ball(), collide_ball_walls(), collide_ball_paddle(), reset_ball() y compute_ai_direction().

Martí Calvo se encargó de la configuración y de la validación de las entradas: la selección de parámetros, los colores RGB, la lectura de enteros y las preguntas de sí/no. Para ello, implementó las funciones get_config(), change_colors(), read_color(), read_int_in_range(), read_int_in_range_w_default() y ask_yes_no().

Aunque nos repartimos el trabajo por funciones, ambos conocemos la estructura general y podemos explicar el funcionamiento de todo el código.

Descripción general del diseño:
Hemos basado el diseño en funciones pequeñas y especializadas. Cada función recibe los datos necesarios mediante parámetros y devuelve los resultados, normalmente en forma de tupla cuando hay varios valores. 

Al comenzar, desde main() llamamos a get_config(), que devuelve, la altura de la pala, la velocidad de la pala, las velocidades horizontal y vertical de la pelota y el radio de la pelota. Después llamamos a change_colors(), que devuelve los colores de fondo y primer plano como tuplas RGB. Guardamos estos valores en variables locales de main() y los pasamos como argumentos a las funciones correspondientes.

Una vez inicializado Pygame, establecemos las posiciones iniciales, las velocidades y el marcador. En cada iteración del bucle principal procesamos los eventos y las teclas, calculamos la dirección de cada pala, actualizamos sus posiciones y movemos la pelota. A continuación, comprobamos los rebotes contra las paredes y las colisiones con las palas.
 
Después comprobamos si la pelota ha salido de la pantalla y si corresponde sumar un punto y reiniciar el saque. Por ejemplo, utilizamos move_ball(x, y, vx, vy) para obtener las nuevas coordenadas. Asignamos estos valores en main() y los pasamos a collide_ball_walls() y después a collide_ball_paddle(). Actualizamos las posiciones y velocidades mediante los valores devueltos, en lugar de modificar directamente el estado global.

Decisiones clave de implementación:
Colisión entre la pelota y las palas
En collide_ball_paddle() aproximamos la pelota mediante un rectángulo delimitador calculado a partir de su centro y su radio: izquierda/derecha son x - radius y x + radius; arriba/abajo son y - radius y y + radius. También representamos la pala mediante sus cuatro límites. Detectamos el choque cuando existe solapamiento horizontal y vertical entre ambos rectángulos (AABB, rectángulos alineados con los ejes).

Cuando detectamos una colisión, invertimos la velocidad horizontal. Además, si la pelota se mueve hacia la derecha, la colocamos justo a la izquierda de la pala; si se mueve hacia la izquierda, la colocamos a su derecha. La función devuelve la posición corregida, las velocidades y un hit. Con esta recolocación evitamos que la pelota siga solapada y que detectemos repetidamente el mismo contacto. No aplicamos ningún efecto adicional a la velocidad vertical.

Validación de las entradas del menú
Con read_int_in_range() repetimos la pregunta hasta recibir un entero dentro del intervalo permitido, señalando cada vez que el usuario introdujese un entero no valido que ese valor tiene que estar dentro del intervalo mostrado en la pregunta. Colocamos la conversión con int() dentro de un bloque try/except ValueError, de manera que una entrada no numérica no interrumpa el programa. 

En read_color() utilizamos esta misma función para leer los componentes rojo, verde y azul, cada uno entre 0 y 255. En change_colors() comprobamos, además, que el fondo y el primer plano no sean iguales; si coinciden, volvemos a los colores predeterminados. Por último, en ask_yes_no() eliminamos los espacios exteriores y convertimos la respuesta a minúsculas, aceptando y/yes y n/no.

Otras decisiones y diferencias respecto a los docstrings
En compute_ai_direction() calculamos el centro de la pala y utilizamos una tolerancia fija de 10 píxeles para evitar que la pala automática cambie continuamente de dirección cuando la pelota está alineada. Decidimos defininir esta tolerancia dentro de la función, en lugar de utilizarla como parámetro.

En reset_ball() utilizamos el parámetro to_right para decidir el sentido horizontal del saque. Elegimos la velocidad vertical aleatoriamente entre vy0 - 2 y vy0 + 2 (para que nunca fuese 0), con un mínimo de 1, y decidimos también su signo de forma aleatoria.

Hemos implementado la función read_int_in_range_w_default() como alternativa que permite pulsar ENTER para utilizar un valor por defecto, aunque en el menú principal utilizamos read_int_in_range().

En la colisión con la pala no hemos implementado el efecto opcional sobre vy, sino que hemos decidido mantener un rebote horizontal sencillo.

Problemas encontrados y soluciones:
Uno de los principales problemas surgió al gestionar el choque entre la pelota y las palas. A veces, la pelota llegaba a solaparse un poco con el rectángulo de la pala y, como el juego revisa los impactos en cada fotograma, ese solapamiento se interpretaba como un choque nuevo una y otra vez, haciendo que la velocidad horizontal rebotara sin parar.

Lo solucionamos dentro de collide_ball_paddle() ajustando la posición de la pelota para dejarla justo fuera de la pala en función de su trayectoria previa. De este modo, en cuanto se invierte vx, ambos objetos ya no se tocan y el siguiente fotograma continúa sin errores.
También tuvimos que proteger la entrada de datos en el menú inicial. Si el usuario escribía algo que no fuera un número (provocando un ValueError) o introducía cifras sin sentido, el programa fallaba. Lo resolvimos pidiendo el dato en bucle, capturando el error y comprobando que estuviera dentro de los márgenes válidos para que el juego nunca se cierre de golpe.

Pruebas:
Hicimos varias pruebas para validar tanto la jugabilidad como la estabilidad del código. Revisamos los topes de movimiento de las palas, los rebotes en las paredes superior e inferior, el contacto con las dos palas, el saque tras anotar un punto y el comportamiento de la pala controlada por la máquina.

Finalmente, testeamos el menú a conciencia probando textos no numéricos, números fuera de rango, respuestas de sí/no escritas con distintas combinaciones de mayúsculas y minúsculas, y la selección de colores repetidos.

Funciones opcionales:
Para darle más juego al proyecto, hemos añadido un modo para jugar contra la máquina activando la constante PLAY_AGAINST_MACHINE = True. En este modo, la pala derecha sigue automáticamente el movimiento vertical de la pelota gracias a la función compute_ai_direction().

Además, hemos hecho que el juego sea fácilmente configurable: se puede ajustar tanto la altura y velocidad de las palas como el tamaño, la velocidad y los colores (en formato RGB) de la pelota. Todo esto lo hemos integrado respetando la estructura original de la plantilla, sin alterar ni el bucle principal ni el apartado de dibujo.

Uso de herramientas de IA:
Hemos ido intercalando el uso de Chat GPT y Claude como apoyo para revisar el código frente a las instrucciones de la práctica, comentar problemas que nos sucedían con el código cuando ya no había otra alternativa y preparar un primer borrador de este informe con las ideas principales. También hemos activado el autocompletado de GitHub Copilot en Visual Studio Code para orientarnos mientras íbamos creando nuestro propio código.