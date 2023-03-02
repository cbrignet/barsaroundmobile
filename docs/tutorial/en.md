# Tutorial: Develop __Heritage in...__ app

## Pre Requirements:

1. GIT client ([Download](https://git-scm.com/downloads))
2. Node.js (v14 or higher) + npm. ([How to install Node.js and npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm))
3. Open Refine ([Download and Info](https://openrefine.org/))
4. Source code editor. The one you prefer (e.g., [Visual Studio Code](https://code.visualstudio.com/), [Notepad++](https://notepad-plus-plus.org/downloads/), …)

You can test that you have everything installed from the command line:

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

To test if Open Refine is installed, run the app and open http://127.0.0.1:3333/ on your web browser.


## Step 1: Get the source code and templates

Clone the [main repository](https://github.com/ow2-quick-app-initiative/poi-quick-app-implementations) to your Github account (you can use whichever platform you prefer, of course). To do this, [you need an account](https://github.com/login).

Once you have entered your password on Github, go to the repository with the examples ([poi-quick-app-implementations](https://github.com/ow2-quick-app-initiative/poi-quick-app-implementations)) and press the button to use template (_Use this template_). Create a __new repository__ in your personal space and you will have access to all the examples and templates.

- You can give it whatever name you want or just use the same name, `poi-quick-app-implementations`.
- Select to include all branches of the main repository.

Go to the options of the project you just created and activate the publication of the content (using _Pages_):

Repository home page > _Settings_ > _Pages_ > Publish from the `gh-pages` branch, and the `/docs` directory.

For continuous integration actions to work correctly you must configure them by including a token for the post.

- Go to your profile > _Settings_ > __Developer settings_ > _Personal access tokens_ > _Generate new token_
- Assign a name that will help you to identify the created token (eg, _My Heritage in app_).
- Assign the `repo` options.
- Copy the newly created token to the clipboard

Now we are going to include it as a repository secret for the project.

From the main page of our repository:

- _Settings_ > _Secrets and variables_ > _Actions_ > _New repository secret_
- Name: `ACCESS_TOKEN`
- Secret: `ghp_rK453....` <- what you have on the clipboard
- Add Secret - Check on the next screen that a secret with the name `ACCESS_TOKEN` appears.


## Clone the project on your computer

To start working on your team, you need to clone the repository you just created to your account.

Go to the main section of the repository, in the dropdown button labeled as code (_<> code_) you will see the URL that identifies your repository. It will be something like `https://github.com/your-user/poi-quick-app.git` (replace `your_user` with the name associated with your account).

Clone the repository on your computer.

From the command line:

````bash
git clone https://github.com/your_user/poi-quick-app-implementations.git
````

In the newly created `poi-quick-app-implementations/docs/` directory you have access to examples and templates for the __database__ and default app codebase.

The templates you will modify are in `poi-quick-app-implementations/docs/sample`.

Rename the `sample` directory with a simple and intuitive name that describes your project, or the place it applies to. You will work on that directory to create the new application.

## Step 2: Database

This project allows us to represent points of interest of any subject, so we need to create a suitable data set with the information that we can represent in the app.

The POI list is described in JSON format (in `poi-quick-app-implementations/docs/sample/data.json`). Each POI is an object that has the following members:

````json
{
    "id": "uniqueID",
    "lat": "50.8789629",
    "lon": "4.7013242",
    "type": "statue",
    "name": "Don Pelayo Statue",
    "images": [
        "https://ow2-quick-app-initiative.github.io/poi-quick-app/sample/images/uniqueID_1.jpg",
        "https://ow2-quick-app-initiative.github.io/poi-quick-app/sample/images/uniqueID_2.jpg"
    ],
    "description": "Description",
    "more": "More info (optional)",
    "urls": [
        "https://es.wikipedia.org/wiki/whaterver",
        "https://example.org/mypoi"
    ]
}
```` 

Therefore, you need a list of records with the following attributes:

- `id` (text) with a unique identifier within the list of points.
- `lat` , `lon` (text) with GPS coordinates in WGS84 format.
- `type`(text) the type of entity that the point represents (e.g., `monument`, `restaurant`, `garden`). Use whatever classification you want.
- `name` (text) the title of the point of interest. Short and concise.
- `description` (text) a descriptive text about the point of interest. A paragraph.
- `more` (text) plus descriptive text, if you want (leave blank if not).
- `images` (text array) with the URLs of the image or related images. Empty array if you don't have .
- `urls` (array of texts) with the additional URLs of the point of interest.

If you prefer, you can use a template in CSV format (`poi-quick-app-implementations/docs/sample/template_database.csv`).

### Refine your data set

Select the theme and look for an open dataset to serve as a base (make sure it has a license that allows you to reuse it and complies with it). Take a look at open data portals or Wikipedia.

Example: https://bruxellesdata.opendatasoft.com/explore/dataset/bruxelles_parcours_bd/information/

You can create an example from scratch (include at least 10 elements or points of interest).

If the data set is in tabular format, you can use OpenRefine for quick data cleansing and transformation.

Open Refine app, defaults to: http://127.0.0.1:3333/

- Create _facets_ for data filtering and detect __duplicates__, __empty cells__, __erroneous data__, ...
- Make sure you have identifiers and that they are unique.
- Make sure you have separate `lat` and `lon` coordinates.
- Make sure you have a name and description.
- You can group similar elements to establish the types (`type`).

[Loading the dataset into Open Refine (see [functions](https://openrefine.org/docs/manual/grelfunctions))]
- I rename and remove some columns for easier readability (optional).
- Facet to detect rows with wrong data
  - For example, I check if images start with `http` (`value.startsWith('http')`)
- I divide coordinates into lat and lon (split in two columns)
  - Rename as `lat` and `lon`
- Facet to check if ids are unique
- Create a description based on the author (`Add column based on column` > `"Mural by " + value`).
- Facet to check that all rows have description
- I generate an identifier (new column with the value `'00' + row.index`)
- I convert the year in the `more` field (by renaming the column)
- I create a type (new `type` column with the value `'mural'`).

It is exported in JSON format, using a template like the following:

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

The result will look like the following code:

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


The result is an array of points of interest to be loaded directly into the `data.json` document (database), as indicated in the next step.

## Step 3: Configuring the app

In the directory with the templates and code (`poi-quick-app-web`) you will find:
- `docs/sample` with the sample templates for your app.
- `quick-app/` with the example code base of a MiniApp.


1. Rename the `sample` directory with the name of your project (e.g., `gijon-monumentos`)
2. Open `data.json` in your code editor. That's your app's database.
3. Edit your app's metadata:


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

4. Customize the app:


````json
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
````

The array corresponding to the `pois` key is where you can paste the list of points of interest.

You can check that the format is correct by validating `data.json` against the [JSON schema](https://ow2-quick-app-initiative.github.io/poi-quick-app/schema.json).


## Step 4: Web Publishing

You can use Github and its publish options to expose the database and configuration you just created.

````bash 
cd poi-quick-app-implementations
git add .
git commit -m 'First version of my light app'
git push origin main
````

You can check if the post actions have been executed successfully.

If all goes well you will be able to see the database you have created from a web browser at an address like
`https://your-user.github.io/poi-quick-app-implementations/sample/data.json`


## Step 5: Load the configuration and database in the app

### Download the web app

From the command line:

````bash
git clone https://github.com/ow2-quick-app-initiative/poi-quick-app-web.git 
````

- in the newly created directory, `poi-quick-app-web`, you have the (meta-)web-application that will allow you to display the result of your app.

````bash
cd poi-quick-app-web 
npm install
npm run dev  
````

Open your browser and access the app: http://127.0.0.1:3000/

Include the configuration document you created as a parameter in the URL:

http://127.0.0.1:3000/_/?url=https://ow2-quick-app-initiative.github.io/poi-quick-app-implementations/de/eckernforde/data.json