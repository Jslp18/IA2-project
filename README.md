# FacePASS: Sistema de Validación de Identidades por Reconocimiento Facial

![FacePASS Banner](https://github.com/user-attachments/assets/1b0e3f15-ee89-47a4-b99c-b1733c8d82d1)

FacePass es un proyecto que utiliza redes neuronales métricas, como redes siamesas y triplet, para resolver problemas de autenticación en eventos masivos, evitando fraudes asociados con documentos físicos como la cédula. 

El sistema emplea aprendizaje profundo para comparar imágenes faciales con una base de datos, generando representaciones únicas que permiten identificar de manera rápida y precisa si una persona está autorizada para ingresar. Este enfoque mejora la seguridad y la gestión de accesos en tiempo real, con aplicaciones en eventos deportivos, conciertos y más.

# Autores
Johan Sebastián León Peñaloza - 2202037, Cristian Stivens Villareal Parra - 2204132, Joan Sebastián Patiño Jaimes - 2202052

# Objetivo
Implementar un modelo de validación de rostros mediante redes neuronales métricas para la correcta gestión de ingreso de las personas en distintos escenarios.

# Dataset
[`FacesCRUB`](https://www.kaggle.com/datasets/rajnishe/facescrub-full)
[`FacesCRUB Reducido a 5 imagenes por persona`](https://drive.google.com/drive/folders/1JQu5U0e1ef8qd3yMLHeOHRL6McGatVSH?usp=sharing)

# Modelos
## FaceNet

FaceNet es un modelo desarrollado por Google que produce resperentacionesprofundas a partir de imágenes de rostros. Estas representaciones tienen dimensionalidad reducida y la propiedad de que rostros de la misma persona están más cerca entre sí en el espacio vectorial, mientras que rostros de personas distintas están más alejados. Util para tareas de reconocimiento y verificación de identidad, ya que se pueden comparar distancias entre embebidos para determinar si dos imágenes corresponden a la misma persona o no.

Se dispone de un modelo preentrenado de FaceNet ('https://tfhub.dev/google/tf2-preview/mobilenet_v2/classification/4') llamado Mobilenet v2 que, dada una imagen, devuelve un embebido de esta.

### Carga del dataset:

Se procede a cargar el dataset filtrado por persona, en el que a partir de un conjunto original más amplio, se han seleccionado 5 imágenes por persona, organizadas en directorios separados, en el cual del conjunto original se tomaron 5 imagenes por persona (carpeta) y en total se cuenta con una cantidad de 530 personas. Este formato facilita la generación de tripletas (ancla, positivo, negativo) para el entrenamiento.

### Preprocesamiento de imágenes:

Al conjunto de datos se le realizarón los siguientes pasos:

- Carga la imagen y se verifica si se ha cargado correctamente.
- Cambia el espacio de color de BGR a RGB.
- Redimensiona la imagen a 224x224x3.
- Normaliza los valores de píxeles a [0, 1].

### Generación de los embebebidos de las imagenes:

Una vez preprocesada la imagen, se la pasa al modelo FaceNet:

Se recorren las carpetas que contienen imágenes de diferentes personas, se generan embeddings para cada imagen y se asocian dichos embeddings con la etiqueta (nombre de la persona). El resultado es la “huella facial” de la persona. Es la base sobre la que se trabajará en el modelo siamés con triplet loss. 

### Creación del modelo siamés:

Se define una función que crea un modelo siamés. Este modelo toma una imagen como entrada y devuelve su embedding usando el modelo FaceNet. 
El "modelo siamés" se compone de tres copias idénticas de este sub-modelo con pesos compartidos, para procesar anchor, positive y negative.

### Generación de tripletas:

Se generan tripletas de entrenamiento y validación seleccionando imágenes de la misma persona para ancla y positivo, y de otra persona diferente para el negativo. Esto se hace recorriendo las carpetas de cada persona, seleccionando imágenes y emparejándolas con otras personas al azar.

### Entrenamiento con triplet loss:
Se entrena el modelo siamés usando la triplet loss, se usa como un modelo preentrenado fijo. El entrenamiento busca mejorar la separación de los embebidos de diferentes personas.

Entrada del entrenamiento: [anchor_images, positive_images, negative_images]
Sin etiquetas clásicas, ya que la función triplet loss no requiere clases explícitas, sino la relación entre ancla-positivo-negativo.

### Validación:
También se aplican las mismas tripletas para la validación, calculando la pérdida en el conjunto de validación para ver si la red generaliza.

## Siamese Contrastive approach
Modelo Siamese implementado a partir de capas convolucionales con un enfoque "Contrastive".
![siamese](https://github.com/user-attachments/assets/232eb409-c332-4376-941a-697c4017ebeb)

# Enlace del código desarrollado del proyecto
[`Triplets - modelo No.2`](https://colab.research.google.com/drive/1x7aJHDDZ63AFkns5rHhvxf_pb49LwHyo#scrollTo=jcWCNIcjIQ_V)
[`Triplets - modelo No.2`](https://colab.research.google.com/drive/1LYqGZV682OdXga6aY_WPkvkpN7ddOUk8?usp=sharing)

# Enlace del video presentación del proyecto
[`Youtube`](https://www.youtube.com/watch?v=SRJoflCAer4)

# Enlace de diapositivas de presentación del proyecto
[`Presentación`](https://www.canva.com/design/DAGVtnfq0fQ/NIcFgIZAmvOBVNsw11pKJg/edit?utm_content=DAGVtnfq0fQ&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)

