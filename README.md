# Proyecto-Reconocimiento-Facial
En este repositorio muestro la creación de un programa que reconozca mi rostro.

Alumno: Mario Alexis Ramírez Bautista
##La estrategia general
En primer lugar, se investigó sobre la forma en la que se pueden implementar redes neuronales para reconocimiento de características en imágenes, para ese caso se llegó a la conclusión (después de revisar varios foros), de que se trata de un proyecto en donde debe haber una clasificación binaria (pues del total de fotos, al final o soy yo, o no lo soy), para darle más calidad al modelo, seleccioné de una base de datos de Kaggle, en su mayoría imágenes de rostros masculinos.
Desde hace varios días me había estado tomando muchas fotos de la cara para tener muchas imágenes porque no suelo tener muchas imágenes mías, en total logré juntar 255 imágenes donde aparece mi rostro y elegí también 255 imágenes de otros rostros para que sea una base de datos balanceada (50% vs 50%). Posteriormente lo que hice fue seleccionar un modelo de red neuronal convolucional previamente entrenado (vgg19) para clasificar imágenes del conjunto de imagenet y modificar la ultima capa del modelo para que me haga la distinción que yo requiero. En este caso se utilizó el método llamado ”Feature Extraction” que es una modalidad de Transfer Learning mediante el cual se aprovechan los pesos de la red original y además la arquitectura de la misma. 
##Problemas encontrados
Los hay problemas que se encontraron a lo largo del desarrollo del proyecto fueron varios desde el inicio hasta el final. algunos de los primeros problemas que encontré fue precisamente saber o entender cómo iba a funcionar esta red que lograra distinguir mi rostro dentro de una imagen, Incluso al inicio del proyecto no sabía si ya había una base de Datos que tuviera muchos rostros pues desde mi perspectiva en ese momento considere que iba a requerir miles y miles de imágenes de rostros tanto de otras personas como mío y pensé que yo mismo podría hacer esa base de Datos descargando imágenes de Facebook, este problema se solucionó cuando comentaron en clases que ya había bases de datos en donde podíamos descargarlos. Otro problema importante fue las pocas imágenes de mi rostro que tenía así que me dediqué a tomarme varias a lo largo de varios días pues al principio del proyecto aunque no tenía claras muchas cosas sabía que la red iba a necesitar imágenes de mi rostro y también imágenes que no fueran mi rostro. Algo que me llevó mucho tiempo entender fue la forma en la que iba yo a implementar una red neuronal convolucional pero en documentación en internet encontré que existe un enfoque precisamente para cuando se quiere Hacer una clasificación de imágenes cuando se tienen pocos Datos en ese momento y debido a mi situación de tener muy pocas imágenes mías decidí que iba a seguir ese camino. empecé así a investigar sobre el Transfer Learning y cuando iba entendiendo el procedimiento empecé a creer que iba a ser algo más sencillo de lo que me había planteado sin embargo en el momento de la ejecución salieron muchos problemas pues no sabía cómo cargar los datos como etiquetarlos y entonces tuve que ponerme a investigar acerca de librerías como Scikit-learn. Al final del proyecto había problemas debido a que pude hacer que un programa corriera únicamente con pocas imágenes (25) pero cuando le di todas las imágenes (510) el programa ya no corría pues se saturaba la memoria incluso con 60 imágenes para resolver este problema me planteé la posibilidad de disminuir los Datos o bien hacer un pre procesamiento de los mismos para poder disminuir el tamaño y la resolución tanto de las imágenes de mi rostro como las que no eran de mi rostro, después de este problema tuve otro derivado ya que las imágenes habían quedado en una dimensión extra a la que mi modelo de Transfer Learning funcionaba, este problema ya me había aparecido anteriormente y tuve que hacer un ajuste a las imágenes para eliminar una dimensión a los arreglos de numpy (antes mis imágenes). Y el último problema que tengo ahora es que mi red neuronal no me da los resultados que yo habría deseado.
##Intentos
Los intentos que hice en la última parte del proyecto cuando ya logré que mi red trabajara con todas las imágenes fueron varios y me dieron resultados similares hubo situaciones en las que modificar la Learning rate dio resultados muy variados como es el caso de la prueba número cuatro de este repositorio en donde no mejoró para nada la exactitud y bajaba lentamente la función de costo. Por otro lado, hubo intentos en donde ni siquiera la función de costo ni la exactitud mejoraron como es el caso de la prueba número 5 de este repositorio. Realice varios ajustes tanto de optimizador como de learning rate, e incluso función de costo, como no obtuve buenos resultados decidí cambiar el tipo de imagen en el pre procesamiento pues había elegido una de 8 bits en blanco y negro, seleccione una di 8 bits en una paleta de colores, volví a hacer el entrenamiento, sin embargo, en este caso obtuve resultados similares. Seguí trabajando en la modificación de los hiper parámetros e incluso cambié los modelos pre entrenados por otros para ver si esto mejoraba los resultados (probé con vgg19, resnet e inception), obteniendo valores por debajo de los que obtuve en el modelo vgg16.
##Red neuronal
La red consiste en el modelo pre entrenado vgg16 que tiene 13 capas convolucionales y 3 de agrupación, es decir 16 capas con parámetros ajustables, de la red vgg16, se cortó la última parte (la capa de 1000 neuronas de salida) y se colocó una capa de salida precisamente para distinguir las imágenes que contengan mi rostro. Se seleccionaron varias learning rate, con mayores a 1, se obtenían pérdidas de arriba de 20, con valores de Lr de 0.01 ó 0.001, mejoraba la función de costo, el optimizador que dio los “mejores resultados” fue el Adam (probé también RMSprop, y SGD). Las imágenes se recortaron en diferentes tamaños, dentro de lo que se logró fue encontrar un tamaño adecuado para que no fueran demasiado pequeñas pero fueran lo suficientemente grandes para que se pudieran cargar en memoria (con batch de 12), este tamaño fue de 285x285 pixeles. La métrica que se utilizó fue la accuracy (exactitud) y la función de costo fue la binary_crossentropy, utilicé 100 épocas aunque pensé que en algunos casos como no había mejoría, tal vez debí elegir menos. El entrenamiento se hizo con 408 imágenes mitad de imágenes mías y mitad imágenes de rostros que no eran míos.  Y 102 imágenes para el conjunto de prueba. Se creó también una gráfica para visualizar el entrenamiento de la red en donde se puede ver la función de costo y la exactitud graficadas contra el número de épocas de entrenamiento.

##Conclusiones
Concluyendo sobre los resultados obtenidos en este proyecto, por un lado considero que se puede hacer más para mejorar el rendimiento y por otro lado los resultados malos que obtuve se los atribuyo a que el conjunto de imágenes de mi rostro son muy variados (casi no me parezco) pues en algunos casos tengo cabello muy corto y lacio y en otras cabello largo y rizado, en algunas tengo lentes y en otras no incluso puse muchas imágenes en donde aparezco con una máscara en la mitad de la cara y esto lo hice para que fuera más valioso mi trabajo, sin embargo, creo que resultó el efecto contrario. 
