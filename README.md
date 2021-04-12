# ViviData

## Descripción de la metodología

Preprocesamiento de los datos: Se añaden 8 columnas adicionales, provenientes de APIs gratuitas de georeferenciación reversa y datos públicos de Bogotá. Estos son: barrio, upz, localidad, avaluo terreno catastral (m/2) , avaluo terreno comercial (m/2), valorref (m/2), uso, predial (m/2). Estos se obtienen de un API de catastro, donde se busca su ubicación por manzana en un radio de 50m. Luego, se eliminan las columnas innecesarias como id, latitud y longitud. Posteriormente, se utiliza un método de remplazo para los valores nulos, se transforman todas las variables categoricas a variables binarias. Luego, se usa un modelo de k-means para encontrar los grupos o clusters de viviendas basados en sus caracteristicas. Finalmente, para determinar el precio de venta, se utiliza el modelo de k-medias para identificar el grupo al que pertenece, y se encuentra el minimo del valor real de las viviendas de este grupo. Luego con este valor, se calcula un incremento = (min_valor_real_cluster - valor_avaluo_catastral_vivienda_a_predecir)*51%, y el precio por metro cuadrado es el valor del avaluo_catastral_vivienda_a_predecir+incremento = precio (m/2)

## Resultados

Se obtuvieron resultados favorables, entorno a las variables descritas, para la muestra seleccionada se usaron 8 clusters con la información de las zonas, adjunto los centros. El modelo retorna valores que corresponden con la realidad. Además, la fórmula planteada permite optimizar las ganancias de Habi, al adquirir una casa para su venta, ** al enviar la información en el kaggle se obtuvó el puesto de 10 con el modelo entrenado de solo la muestra de 866 , 0.4% de los datos de entrenamiento. **

## Pasos a seguir

Probar la metodología con más datos para probar la distribución de los precios, utilizar un modelo que permita mejorar e identificar el mejor número de clusters, validar la información con información real de venta.

## Retos encontrados

El entrenamiento del modelo requiere una gran cantidad de llamados a API, aunque las APIs de catastro no tienen limitaciones, la de georefenciación inversa tiene limite de 2 peticiones por segundo, esto requiere una gran cantidad de tiempo para los datos de entrenamiento, para facilitar el proceso se seleccionó una muestra aleatoria de 1000 viviendas de este conjunto. Otro reto fue la precisión de la georefenciación, especialmente en barrio, sin embargo, la API de catastro solo requiere lat y lon lo que facilitó este proceso. Finalmente, un reto grande fue la definción de las reglas de decisión final, respecto a determinación del precio luego del clustering.

## Arhivos

El notebook algorithm2, contiene el código de la solución.

APIs utilizadas:

Servicios catastro: Mapas, se toman zonas de 50m para calcular los demás valores a partir de la latitud y longitud. https://serviciosgis.catastrobogota.gov.co/arcgis/rest/services/catastro

Servicio de georefenciación reversa: geopy