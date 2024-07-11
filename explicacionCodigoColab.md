# colab_lms.py

## Explicación General

En resumen, este código se utiliza para ejecutar casos de prueba y mostrar los resultados de forma organizada. Utiliza archivos de texto ubicados en GitHub para obtener los casos y sus resultados esperados, y luego ejecuta el código proporcionado en los casos para comparar los resultados obtenidos con los resultados esperados.

## Explicación Detallada:

- `def get_case(preg)`:
    - Se encarga de obtener un diccionario de casos a partir de un archivo de texto ubicado en GitHub. Para hacer esto, realiza una solicitud HTTP al archivo `cases_urls.txt` y lee cada línea del archivo. El archivo `cases_urls.txt` contiene los path o en este caso en Github los url a los .txt donde estan los casos de cada pregunta.  
       
          p1,https://raw.githubusercontent.com/rilopez3/colab_lms/main/em_20222_solemne_2/cases_p1.txt
          p2,https://raw.githubusercontent.com/rilopez3/colab_lms/main/em_20222_solemne_2/cases_p2.txt
          p3,https://raw.githubusercontent.com/rilopez3/colab_lms/main/em_20222_solemne_2/cases_p3.txt
          p4,https://raw.githubusercontent.com/rilopez3/colab_lms/main/em_20222_solemne_2/cases_p4.txt
          p5,https://raw.githubusercontent.com/rilopez3/colab_lms/main/em_20222_solemne_2/cases_p5.txt
    - Finalmente, la función verifica si preg existe como clave en el diccionario cases y devuelve el contenido asociado a esa clave. Si preg no está en cases, devuelve None.

- `def run_case(subname, tipo, code, expected)`:
    - Se encarga de ejecutar un caso de prueba. Recibe cuatro argumentos: subname (nombre del subcaso), tipo (tipo de caso: "Publico" o "Secreto"), code (código a ejecutar) y`expected (resultado esperado). 
    - Basicamente se encarga de imprimir el texto de acuerdo a si es tipo Publico o Secreto y comparar el resultado del alumno con el resultado esperado de acuerdo a cada tipo de pregunta.
    - Si el tipo es "Secreto", se calcula el resumen hash MD5 del valor de res y se guarda en res.  
    Se muestra en pantalla el estado del caso ("OK" si res es igual a expected, "Incorrecto" en caso contrario).
    - Si el tipo es "Publico", se muestra una línea separadora, se imprime "RECIBIDO" y se muestra el valor de res.  
    Se actualiza estado a 1 si res es igual a expected, de lo contrario se mantiene en 0.

- `def read_cases(fileurl)`:
    - Si fileurl no es nulo, se intenta abrir la URL y leer su contenido.  

          #FPregunta 1
          #PCase 1
          %CODE
          print(crear_tropas(5,10))
          %OUTPUT
          [10, 10, 10, 10, 10]
          #SCase 1
          %CODE
          print(crear_tropas(11,11))
          %OUTPUT
          b13efad9a9c7e232261c4535ce1d87b4

    - Se itera sobre cada línea del contenido del archivo.
    - Si la línea comienza con `'#F'`, se extrae el nombre del caso y se asigna a `nombre`.
    - Si la línea comienza con `'#P'`, se agrega el caso actual a cases si existe y se crea un nuevo caso `público` en `case`.
    - Si la línea comienza con `'#S'`, se agrega el caso actual a cases si existe y se crea un nuevo caso `secreto` en `case`.
    - Si la línea es igual a `%CODE`, se `restablece temp` a una cadena vacía.
    - Si la línea es igual a `%OUTPUT`, se agrega el contenido actual de `temp a case` y se `restablece temp` a una cadena vacía.
    - En caso contrario, se agrega la línea a `temp`.
    - Si al finalizar el bucle hay un `caso pendiente` en case, se agrega a `cases`.
    - Finalmente, se devuelve nombre (nombre del caso) y cases (lista de casos).

- `def run_test(preg)`:
    - Recibe un argumento preg que se utiliza para obtener los casos a ejecutar llamando a la función `read_cases` con el resultado de `get_case(preg)`
    - Si el tipo es "Publico", se llama a run_case con los argumentos correspondientes y se incrementan los contadores pub y t_pub.
    - Si el tipo es "Secreto", se llama a run_case con los argumentos correspondientes y se incrementan los contadores sec y t_sec.
    - Se construyen las cadenas total_pub y total_sec con el recuento de casos ejecutados.


# colab_lms_eval.py

## Explicación General:

En resumen, este código está relacionado con la evaluación automática de casos de prueba y la generación de informes en formato CSV. Utiliza archivos de texto para almacenar los casos y los resultados esperados, y ejecuta el código proporcionado en los casos para comparar los resultados obtenidos con los resultados esperados. También proporciona funciones para obtener variables y limpiarlas, y para formatear datos en formato CSV.

## Explicación Detallada:

- `def get_case(preg)`:
    - Lo mismo de la funcion del archivo anterior.

- `def clean_exec()`:
    - Se utiliza para limpiar las variables de evaluación. Utiliza la función `evaluation_variables()` para obtener una lista de las variables que se deben limpiar.

- `def eval_preg(preg)`:
    - Se encarga de evaluar una pregunta específica. Recibe un argumento preg y utiliza las funciones `get_case(preg)` y `read_cases()` para obtener los casos asociados a esa pregunta.
    - Parecido a `run_test(preg)` de arriba, pero se evalua cada caso utilizando la función `eval_case()`.

- `def evaluation_variables()`:
    - La lista a utilizar en `clean_exec()`.

- `def get_variables()`:
    - Construye un código de Python que devuelve un diccionario `data` con las variables a obtener. Utiliza la función `evaluation_variables()` para obtener la lista de variables, y luego construye una cadena var_code que contiene el código Python para crear el diccionario.

- `def eval_case(subname, tipo, code, expected)`:
    - Parecido a `run_case()` del archivo anterior.
    - Se ejecuta el código utilizando la función exec.  
    Se obtiene el valor de la salida capturada en new_stdout y se guarda en la variable res.  
    Se restablece la salida estándar original redirigiéndola de nuevo a old_stdout.  
    Si el tipo es "Secreto", se calcula el resumen hash MD5 del valor de res y se guarda en res.  
    Se actualiza estado a 1 si res es igual a expected, de lo contrario se mantiene en 0.
    - Si se produce alguna excepción durante la ejecución del código, se guarda el mensaje de error en comentario.  
    Finalmente, se devuelve el valor de estado y comentario

- `def get_preg()`:
    - Se utiliza para obtener la lista de preguntas a partir del archivo `cases_urls.txt`. (Guarda solo el "nombre" y no el path de las preguntas)

- `def csv_format(data:list)`:
    - Recibe una lista de datos y devuelve una cadena de texto en formato CSV. Utiliza la función.

# eval_responses.py

## Explicación General

En resumen, este código se encarga de evaluar el código de los estudiantes en archivos `.ipynb`, generar resultados y guardarlos en un archivo `CSV`. Utiliza el módulo glob para buscar los archivos, carga y ejecuta los archivos `colab_lms.py` y `colab_lms_eval.py` que contienen funciones y variables necesarias, limpia las variables globales, ejecuta el código del estudiante, evalúa las respuestas y guarda los resultados en un archivo CSV. El código también proporciona opciones de depuración para imprimir mensajes adicionales durante la ejecución.

## Explicación Detallada:

- `debug`: Esta variable se utiliza más adelante en el código para habilitar o deshabilitar la impresión de mensajes de depuración.
- `protected`: Nombres de variables y funciones que están protegidas y no se deben limpiar ni modificar.
- Se carga y ejecuta el código del archivo `colab_lms.py` y el del archivo `colab_lms_eval.py`.
- Se abre un archivo CSV, el nombre del archivo se crea concatenando la cadena 'res/results ' con la fecha y hora actual y luego se agrega la extensión .csv.
- Se crea una lista `header` que contiene los encabezados de las columnas para el archivo CSV. Esta lista se compone de los elementos: 'path', 'filename' y los valores devueltos por la función `evaluation_variables()`, que son las variables que deben evaluarse. Luego, se utiliza la función `csv_format(header)(de colab_lms_eval.py)` para convertir la lista en una cadena CSV y se imprime en el archivo `res_file`
- Se recorren todos los archivos con extensión `.ipynb` en el directorio actual y sus subdirectorios. Esto se hace utilizando la función `glob.glob('**/*.ipynb', recursive=True)`, que devuelve una lista de nombres de archivos que coinciden con el patrón `'**/*.ipynb'`.
- Se abre cada archivo `.ipynb` y se carga su contenido en la variable nb utilizando la función `load(fp)` del módulo json.
- Se inicializa la cadena `student_code` que se utilizará para almacenar el código del estudiante. Luego, se itera sobre las celdas del archivo `.ipynb` y se busca las celdas de tipo 'code'. Si una celda no contiene el comentario `'NO MODIFICAR ESTA CELDA'`, se agrega su contenido a la cadena student_code.
- Se divide el nombre del archivo utilizando el carácter de barra invertida ('\\') como separador y se obtiene el último elemento de la lista resultante. Esto se asigna a la variable `filename`.
- Se ejecuta `clean_exec() (de colab_lms_eval.py)`.
- Se obtiene la lista de todas las variables globales utilizando `list(globals().keys())` y se guarda en la variable globales.  
Se itera sobre cada variable en globales y se comprueba si el nombre de la variable no comienza con un guion bajo y no está en la lista protected. Si se cumple esta condición, la variable se establece en None utilizando la función `exec()`.
- A continuación, se ejecuta el código del estudiante utilizando `exec(student_code)`. Esto ejecuta el código almacenado en la cadena `student_code`.
- Se ejecuta `get_variables()` para obtener un código de Python que devuelve un diccionario `data` con las variables a obtener.
- Se itera sobre las preguntas obtenidas mediante `get_preg()`. Para cada pregunta, se ejecuta `eval_preg(i)` para obtener los resultados de la evaluación. Los resultados se agregan a la lista `detail_res` junto con el nombre de la pregunta, que se guarda en `data['results']`.
- Se actualizan los totales de puntos públicos y secretos (pub, t_pub, sec, t_sec) utilizando los valores de `data['results']`.
- La lista resume se completa agregando los totales de puntos a la lista.  
Se convierte la lista `resume` en una cadena CSV utilizando `csv_format(resume)` y se imprime en el archivo res_file utilizando `print(csv_format(resume), file=res_file)`.