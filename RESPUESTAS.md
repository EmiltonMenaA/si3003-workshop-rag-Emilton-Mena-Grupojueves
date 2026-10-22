# Respuestas del workshop RAG
Escribe aquí tus respuestas con tus propias palabras. Pega tablas, resultados y enlaces a las capturas de `evidencias/`.

---

# Bloque 0 · Teoría

## T1 · Cuántos nodos viven en cada capa

<img width="422" height="260" alt="image" src="https://github.com/user-attachments/assets/252c8365-950e-4b41-94bf-9f95b01b0ab5" />


Analizo que las capas superiores permiten recorrer grandes distancias del grafo y acercarse rápidamente a la región donde está el vecino buscado. Tener pocos nodos en esas capas reduce el trabajo de navegación y ayuda a que la búsqueda sea más rápida


## T2 · ef, k y el tamaño de la cola


1. Si pides k = 10 vecinos y configuras ef = 5, ¿qué falla y por qué?

ef controla el tamaño de la lista de candidatos explorados durante la búsqueda, mientras que k indica cuántos vecinos se desean obtener. Si ef es menor que k, la búsqueda no tiene suficientes candidatos para garantizar la recuperación de los 10 vecinos solicitados. En implementaciones habituales de HNSW, se recomienda configurar ef al menos igual a k.

2. ¿Con qué valor conviene empezar y por qué?

Conviene comenzar con ef ≥ k, por ejemplo, ef = 10 si k = 10. Si la recuperación de vecinos no es suficientemente buena, se puede aumentar ef a 20, 50 o más, evaluando el equilibrio entre precisión y tiempo de respuesta.

3. ¿Cuál parámetro puedes cambiar en producción sin reconstruir el índice?
- ef o ef_search: se puede ajustar durante la búsqueda, normalmente sin reconstruir el índice.
- ef_construction: controla el esfuerzo dedicado a construir el grafo y su calidad; modificarlo para un índice ya construido normalmente requiere reconstruirlo.


## T3 · Inserción frente a búsqueda


Pregunta	Inserción	Búsqueda
¿Se sortea la capa?	Sí, se asigna aleatoriamente un nivel al vector nuevo.	No, se recorre el grafo ya construido.
Valor de ef en las capas altas	Se usa ef_construction para explorar candidatos durante la construcción.	Se usa ef o ef_search para controlar los candidatos explorados.
Parámetro de la fase principal	ef_construction durante la construcción del índice.	ef o ef_search durante la consulta.
¿Modifica el índice?	Sí, añade el vector y sus conexiones.	No, solo consulta el índice existente.

Capa: durante la inserción se asigna un nivel aleatorio al vector; durante la búsqueda se navegan los niveles ya existentes.
Valor de ef: en la inserción se controla el esfuerzo de construcción; en la búsqueda se controla cuántos candidatos se consideran.
Parámetro principal: cada operación utiliza un parámetro ajustado a su objetivo, construcción o recuperación.
Modificación del índice: insertar cambia la estructura del grafo; buscar no cambia sus conexiones.


## T4 · El invariante de FAISSVectorStore


¿Qué ocurre al recuperar los resultados?
Índice devuelto por FAISS	Documento que se consulta en la lista	Resultado
0	A	Correcto
1	C	Error silencioso: FAISS se refería a B
2	D	Error silencioso: FAISS se refería a C
3	No existe	Excepción IndexError

¿Cuáles son los tres resultados problemáticos?

- Índice 1: devuelve C en vez de B, sin advertir del error.
- Índice 2: devuelve D en vez de C, también sin advertirlo.
- Índice 3: intenta acceder a una posición inexistente y produce una excepción.
  
¿Por qué se utilizan bases vectoriales como Qdrant?

Porque gestionar correctamente los identificadores, los metadatos y las eliminaciones es fundamental para mantener la correspondencia entre los vectores y sus documentos. Una base vectorial que administra estas operaciones reduce el riesgo de recuperar información incorrecta por desajustes de índices.

## T5 · El costo de la fuerza bruta

Se necesitan aproximadamente  76.800 millones de multiplicaciones por segundo.

2. ¿Por qué el cálculo exacto deja de ser viable?
   
Aunque la búsqueda exhaustiva encuentra los vecinos más cercanos exactos según la métrica utilizada, debe comparar cada consulta con todos los vectores. El costo computacional y la latencia crecen con el tamaño de la base de datos, especialmente cuando hay muchas consultas simultáneas.

4. ¿Qué sacrifica ANN y qué gana?
ANN (Approximate Nearest Neighbors) sacrifica la garantía de encontrar siempre los vecinos exactos para reducir el número de comparaciones realizadas. A cambio, obtiene búsquedas mucho más rápidas y escalables, con una calidad de recuperación que puede ajustarse según los parámetros del índice.

---

# Bloque A · App RAG empresarial

## A1 · Ejecuta la app RAG de punta a punta

_Evidencia:_ …

_Mi respuesta / análisis:_


## A2 · Tu propio corpus de documentos

_Evidencia:_ …

_Mi respuesta / análisis:_


## A3 · Ingeniería del prompt (antes y después)

_Evidencia:_ …

_Mi respuesta / análisis:_


## A4 · Muestra el score de similitud en las fuentes

_Evidencia:_ …

_Mi respuesta / análisis:_


---

# Bloque B · RAG sobre papers

## B1 · Ejecuta el RAG de papers y evalúa con el ground truth

_Evidencia:_ …

_Mi respuesta / análisis:_


## B2 · Experimentos con chunk_size, k y score_threshold

_Evidencia:_ …

_Mi respuesta / análisis:_


## B3 · Preguntas trampa: ¿el RAG admite que no sabe?

_Evidencia:_ …

_Mi respuesta / análisis:_


## B4 · Preguntas en español sobre papers en inglés

_Evidencia:_ …

_Mi respuesta / análisis:_


## B5 · Escribe tu propio quiz con ground truth

_Evidencia:_ …

_Mi respuesta / análisis:_


---

# Reflexión

## R1 · Reflexión final con tus datos

_Evidencia:_ …

_Mi respuesta / análisis:_


---

# Retos

## C1 · HNSW contra fuerza bruta con FAISS

_Evidencia:_ …

_Mi respuesta / análisis:_


## C2 · Un tercer paper: el RAG que explica HNSW

_Evidencia:_ …

_Mi respuesta / análisis:_


## C3 · App Gradio para los papers con filtro por paper

_Evidencia:_ …

_Mi respuesta / análisis:_


## C4 · Evaluación automática de la recuperación

_Evidencia:_ …

_Mi respuesta / análisis:_


## C5 · MMR frente a similitud pura

_Evidencia:_ …

_Mi respuesta / análisis:_


## C6 · Persistencia del índice en disco

_Evidencia:_ …

_Mi respuesta / análisis:_

