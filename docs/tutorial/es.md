# Tutorial: Crea tu aplicación __Heritage in...__

## Pre-requisitos:

1. Cliente GIT ([Descarga](https://git-scm.com/downloads))
2. Node.js (v14 o superior) + npm. ([Cómo instalar Node.js y npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm))
3. Open Refine ([Descarga e información](https://openrefine.org/))
4. Editor de código fuente. El que más te guste (e.g., [Visual Studio Code](https://code.visualstudio.com/), [Notepad++](https://notepad-plus-plus.org/downloads/), …)

Puedes probar que tienes todo instalado desde la línea de comandos:

````bash
node -v 

> v19.4.0   
````

````bash
git -v

> git version 2.39.0
````

````bash
npm -v

> 9.2.0
````

Para probar Open Refine, ejecuta la app y accede a la aplicación desde el navegador Web en la dirección http://127.0.0.1:3333/  


## Paso 1: Obtén el código fuente y las plantillas 

Clona el [repositorio principal](https://github.com/ow2-quick-app-initiative/poi-quick-app-implementations) en tu cuenta en Github (puedes usar la plataforma que prefieras, por supuesto). Para ello, [necesitas una cuenta](https://github.com/login).

Una vez has introducido tu contraseña en Github, vete al repositorio con los ejemplos ([poi-quick-app-implementations](https://github.com/ow2-quick-app-initiative/poi-quick-app-implementations)) y pulsa el botón de usar plantilla (_Use this template_). Crea un __nuevo repositorio__ en tu espacio personal y ya tendrás acceso a todos los ejemplos y plantillas. 
- Puedes darle el nombre que quieras o simplemente usa el mismo, `poi-quick-app-implementations`.
- Selecciona incluir todas las ramas del repositorio principal.

Vete a las opciones del proyecto que acabas de crear y activa la publicación del contenido (usando _Pages_):

Página principal del repositorio > _Settings_ > _Pages_ > Publicar desde la rama `gh-pages`, y el directorio `/docs`.

Para que las acciones de integración continua funcionen correctamente debes configurarlas incluyendo un token para la publicación.

- Vete a tu perfil > _Settings_ > __Developer settings_ > _Personal access tokens_ > _Generate new token_ 
- Asigna un nombre que te sirva para identificar el token creado (p.e., _Mi Heritage in app_).
- Asigna las opciones de `repo`.
- Copia el token recién creado en el portapapeles 

Ahora lo vamos a incluir como un secreto del repositorio para el proyecto.

Desde la página principal de nuestro repositorio:

- _Settings_ > _Secrets and variables_ > _Actions_ > _New repository secret_ 
- Nombre: `ACCESS_TOKEN`
- Secret: `ghp_rK453....` <- lo que tienes en el portapapeles
- Add Secret - Comprueba en la siguiente pantalla que aparece un secreto con el nombre `ACCESS_TOKEN`.

### Clona el proyecto en tu equipo

Para comenzar a trabajar en tu equipo, necesitas clonar el repositorio que acabas de crear en tu cuenta. 

Vete a la sección principal del repositorio, en el botón desplegable etiquetado como código (_<> code_) verás la URL que identifica tu repositorio. Será algo así como `https://github.com/tu-usuario/poi-quick-app.git` (reemplaza `tu_usuario` por el nombre asociado a tu cuenta).

Clona el repositorio en tu equipo. 

Desde la linea de comandos:

````bash
git clone https://github.com/tu_usuario/poi-quick-app-implementations.git 
````

En el directorio `poi-quick-app-implementations/docs/` que se acaba de crear tienes acceso a ejemplos y plantillas para la __base de datos__ y el código base por defecto de la app. 

Las plantillas que modificarás están en `poi-quick-app-implementations/docs/sample`. 

Renombra el directorio `sample` con un nombre intuitivo y simple que describa tu proyecto, o el lugar al que aplica. Trabajarás sobre ese directorio para crear la nueva aplicación.

## Paso 2: Base de datos

Este proyecto permite representar puntos de interés de cualquier temática, por lo que necesitamos crear un conjunto de datos adecuado con la información que podemos representar en la app.

La lista de puntos de interés está descrita en formato JSON (en `poi-quick-app-implementations/docs/sample/data.json`). Cada punto de interés es un objeto que tiene los siguientes miembros:

````json
{
    "id": "uniqueID",
    "lat": "50.8789629",
    "lon": "4.7013242",
    "type": "estatua",
    "name": "Estatua Don Pelayo",
    "images": [
        "https://ow2-quick-app-initiative.github.io/poi-quick-app/sample/images/uniqueID_1.jpg",
        "https://ow2-quick-app-initiative.github.io/poi-quick-app/sample/images/uniqueID_2.jpg"
    ],
    "description": "Descripción del lugar o cosa",
    "more": "Más información si lo deseas (opcional)",
    "urls": [
        "https://es.wikipedia.org/wiki/LoQueSea",
        "https://example.org/mypoi"
    ]
}
```` 

Por lo tanto, necesitas una lista de registros con los siguientes attributos:

- `id` (texto) con un identificador único dentro de la lista de puntos.
- `lat` , `lon` (texto) con las coordenadas de GPS en formato WGS84.
- `type`(texto) el tipo de entidad que representa el punto (p.e., `monumento`, `restaurante`, `jardín`). Usa la clasificación que quieras.
- `name` (texto) el título del punto de interés. Corto y conciso.
- `description` (texto) un texto descriptivo sobre el punto de interés. Un párrafo. 
- `more` (texto) más texto descriptivo, si quieres (déjalo en blanco si no).
- `images` (array de textos) con las URLs de la imagen o imágenes relacionadas. Array vacío si no tiene.
- `urls`  (array de textos) con las URLs complementarias del punto de interés.

Si lo prefieres, puedes usar una plantilla en formato CSV (`poi-quick-app-implementations/docs/sample/template_database.csv`).

### Refina tu conjunto de datos

Seleciona la temática y busca un conjunto de datos abierto que te sirva como base (asegúrate de que tiene una licencia que te permita reusarlo y cumple con ella). Echa un vistazo a los portales de datos abiertos o a la Wikipedia.

Ejemplo: https://bruxellesdata.opendatasoft.com/explore/dataset/bruxelles_parcours_bd/information/

Puedes crear un ejemplo desde cero (incluye al menos 10 elementos o puntos de interés).

Si el conjunto de datos está en formato tabular, puedes usar OpenRefine para una rápida limpieza y transformación de los datos. 

Open Refine app, por defecto en: http://127.0.0.1:3333/

- Crea _facets_ para el filtrado de los datos y detectar __duplicados__, __celdas vacías__, datos __erróneos__, ...
- Asegúrate de tener identificadores y que estos sean únicos.
- Asegúrate de tener coordenadas separadas en `lat` y `lon`. 
- Asegúrate de tener nombre y descripción.
- Puedes agrupar los elementos similares para establecer los tipos (`type`).

[Carga del conjunto de datos en Open Refine (ver [funciones](https://openrefine.org/docs/manual/grelfunctions))]
- Renombro y elimino algunas columnas para facilitar la legibilidad (opcional).
- Facet para detectar filas con datos erróneos 
  - Por ejemplo, compruebo si las imágenes empiezan por `http` (`value.startsWith('http')`)
- Divido coordenadas en lat y lon (split in two columns)
  - Renombro como `lat` y `lon`
- Facet para comprobar si los identificadores son únicos
- Creo una descripción basada en el autor (`Add column based on column` > `"Mural por " + value`).
- Facet para comprobar que todas las filas tienen descripción
- Genero un identificador (nueva columna con el valor `'00' + row.index`)
- Convierto el año en el campo `more` (renombrando la columna)
- Creo un tipo (nueva columna `type` con el valor `'mural'`).

Se exporta en formato JSON, usando una plantilla como la siguiente:

```
    {
      "id" : {{jsonize(cells["id"].value)}},
      "type" : {{jsonize(cells["type"].value)}},
      "lon" : "{{jsonize(cells["lon"].value)}}",
      "lat" : "{{jsonize(cells["lat"].value)}}",
      "name" : {{jsonize(cells["name"].value)}},
      "description" : {{jsonize(cells["description"].value)}},
      "more" : {{ if(cells["more"].value!=null, jsonize(cells["more"].value), '""') }},
      "images" : [{{ if(cells["image"].value!=null, jsonize(cells["image"].value), '""') }}],
      "attributions": ["Source: Brussels Open Data", "Otras atribuciones"],
      "wikidata": {{ if(cells["wikidata"].value!=null, jsonize(cells["wikidata"].value), '""') }},
      "urls": [{{if(cells["urls"].value!=null, jsonize(cells["urls"].value), '') }}]
    }
```


El resultado se parecerá al siguiente código:

````json
{
  "rows" : [
    {
      "id" : "000",
      "type" : "mural",
      "lon" : "4.34942722321",
      "lat" : "50.8467465143",
      "name" : "Broussaille - Ragebol",
      "description" : "Mural por Frank Pé",
      "more" : "En el año 1999",
      "images" : ["https://opendata.bruxelles.be/api/explore/v2.1/catalog/datasets/bruxelles_parcours_bd/files/b17daccbf026ff22035464e3d0f12b51"],
      "attributions": ["Source: Brussels Open Data", "Otras atribuciones"],
      "wikidata": "",
      "urls": []
    },
}
````

El resultado es un array de puntos de interés para cargar directamente en el documento `data.json` (base de datos), como se indica en el siguiente paso.

## Paso 3: Configuración de la app

En el directorio con las plantillas y código (`poi-quick-app-web`) encontrarás:
- `docs/sample` con las plantillas de ejemplo para tu app.  
- `quick-app/` con el código base de ejemplo de una MiniApp.


1. Renombra el directorio `sample` con el nombre de tu proyecto (p.e., `gijon-monumentos`)
2. Abre `data.json` en tu editor de código. Eso es la base de datos de tu app.
3. Edita los metadatos de tu aplicación:

````json
{
    "meta": {
        "app_id": "org.example.gijon.monumentos",
        "app_title": "Monumentos de Gijón",
        "version": 1,
        "updated": "2023-03-09",
        "source_url": "https://ow2-quick-app-initiative.github.io/poi-quick-app-implementations/monumentos-gijon/data.json",
        "matomo_base_url": "https://matomo.pbest.me/matomo.php?idsite=1&rec=1",        
        "marketplace_url": ""
    }
}
````

4. Personaliza la app:

```json
{
    "content": {
        "es": { 
            "app": {
                "theme": {
                    "brand": "#B11623",
                    "complementary": "#FAFAFA"
                },
                "repository_url": "https://github.com/ow2-quick-app-initiative/poi-quick-app/tree/main/docs/es/gijon",
                "text_info": "This app is created by locals and experts. As you can see, it's also free of charge. Please let us know if you want to contribute with your experience, enrich the content, correct inaccuracies, or submit pictures.",
                "text_acknowledge": "This quick app is an open-source project powered by open data and enriched by the community. Most of the information was extracted from Wikidata, OpenStreetMaps, and other public sources. You can find inaccuracies, so we apologize in advance.",
                "text_feedback": "Please let us know if you want to contribute with your experience, enrich the content, correct inaccuracies, or submit pictures. This app is created by locals and experts.",
                "feedback_url": "https://ow2-quick-app-initiative.github.io/poi-quick-app/sample/#get-involved",
                "issue_url": "https://github.com/ow2-quick-app-initiative/poi-quick-app/issues/new?labels=country/city&template=update_request.md&title=Update+Request"
            },
```

El array correspondiente a la clave `pois`, es donde podrás pegar la lista de los puntos de interés.

Puedes comprobar que el formato es correcto, validando `data.json` contra el [esquema JSON](https://ow2-quick-app-initiative.github.io/poi-quick-app/schema.json). 


## Paso 4: Publicación en la web

Puedes usar Github y sus opciones de publicación para exponer la base de datos y la configuración que acabas de crear. 

````bash 
cd poi-quick-app-implementations
git add .
git commit -m 'Primera versión de mi light app'
git push origin main
````

Puedes comprobar si las acciones de publicación se han ejecutado correctamente.

Si todo va bien podrás ver la base de datos que has creado desde un navegador web en una dirección como 
`https://tu-usuario.github.io/poi-quick-app-implementations/sample/data.json` 


## Paso 5: Carga la configuración y base de datos en la app 

### Descarga la aplicación web 

Desde la linea de comandos:

````bash
git clone https://github.com/ow2-quick-app-initiative/poi-quick-app-web.git 
````

- en el directorio recién creado, `poi-quick-app-web`, tienes la (meta-)applicación-web que te permitirá visualizar el resultado de tu app. 

````bash
cd poi-quick-app-web 
npm install
npm run dev  
````

Abre tu navegador y accede a la app: http://127.0.0.1:3000/

Incluye el documento de configuración que has creado como un parámetro en la URL:

http://127.0.0.1:3000/_/?url=https://ow2-quick-app-initiative.github.io/poi-quick-app-implementations/de/eckernforde/data.json



