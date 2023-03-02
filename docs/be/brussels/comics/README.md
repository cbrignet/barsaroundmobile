# Brussels Comic Book Route - Quick App

This project is based on the [City of Brussels Open Data](https://opendata.brussels.be/page/home/) initiative. It uses the data from the official dataset, this doesn´t mean that it´s an official project.

> It's free, open-source and collaborative.

<img width="250" src="https://ow2-quick-app-initiative.github.io/poi-quick-app-implementations/be/brussels/comics/images/screenshots.gif" alt="Screenshots of the application">


## Privacy

The main data was collected using the public dataset of the open data initiative and automatic processing of the data. The content was enriched with [Wikidata](https://www.wikidata.org/wiki/Wikidata:Main_Page) information and curated using the developer´s intuition, so it could contain errors and inaccuracies (apologies in advance). Local experts are welcome to refine the definitions, names, and pictures and add new points of interest to the app.

The app doesn't collect any personal data, so relax. There are calls to an analytics system, just to check if the app is used or not, but the app only send randomly generated information to avoid personal identification.

Perhaps the content is inaccurate, so please [let us know](https://github.com/ow2-quick-app-initiative/poi-quick-app-implementations/issues/new) if you've spotted anything that might be enhanced.

The quick app may perform a call to a [Matomo](https://en.wikipedia.org/wiki/Matomo_(software)) instance to measure its performance, but no personal data is shared. You can just [check the code](https://github.com/ow2-quick-app-initiative/poi-quick-app/blob/0e30a81f203796156ecb29b30437fb18f9f83309/quick-app/src/app.ux#L222) that generates a random identifier.  

## Get involved

> Do you want to contribute to the content?

The project has two resource types:

- *images* (`./images/xxxxx.jpg`): light pictures in square format. If possible 1x1 ratio for homogenous look and feel; the lighter the better (50Kb per image would be fine).
- *database* (`./data.json`): JSON file with the app's configuration (name, colors, privacy texts, etc.) and the points of interest you want to show in the app. 

You can download it in your computer, modify the texts, or add a new element based on your knowledge. You can upload it directly (better a Pull Request if you are familiar with GitHub), or [raise an issue](https://github.com/ow2-quick-app-initiative/poi-quick-app-implementations/issues/new) to suggest the changes (please be explicit).

Note that there is a moderation process, so it may take some hours. Please, leave a note with the suggestion, so the editor may validate and confirm your changes.

### How to submit new images

The images are in the `./images` directory. 

You can find a mistake, or you want to modify and upload a new version. 

If you have new images to add, just upload your new ones in the folder. 

Please, use the identifier of the point of interest you are referring to (see attribute `id` of the [Point of Interest](#points-of-interest)).


### How to update the database?

The database is in a JSON file named `data.json` in the root directory of the project. In `bxl/data.json` you have an empty file you can use to start the project.

Please be sure that this document has the correct format (syntax and content). You can test it using any JSON schema validation tool against the JSON schema you can find in the repository ([schema.json](https://ow2-quick-app-initiative.github.io/poi-quick-app/schema.json)). 

This JSON document contains two main parts, represented by the main keys of the root object:

1. `meta`: metadata about the project
2. `content`: multilingual content and setup of the application (based on localized information)

#### Content of the project

We can localize the content in any language, using the [ISO 639-1](https://www.loc.gov/standards/iso639-2/php/English_list.php) codes (assigning a two-letter code for the language). You can also specify the concrete region (using a `-` character and the concrete [ISO 3166-1](https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes) (Alpha-2 code). 

``` json
{
    "meta": {},
    "content": {
        "en_GB": {},
        "en_US": {},
        "fr": {},
        "es": {}
    }
}
```

The project must contain at least one language tag. 

Every key of the `content` object is a language tag that must contain JSON objects with the same keys but with different translations and content localized according to the language.    

The content language tags are objects with the following structure:

``` json
{
    "meta": {},
    "content": {
        "LANG": {
            "app": {},      // App configuration (color, texts)
            "pois": []      // Points of Interest
        }
    }
}
```

- `app` is an object that contains general information to configure the application for this language.
- `pois` is an array of objects with the points of interest or items of the database. 


``` json
{
    "meta": {},
    "content": {
        "en": {
            "app": {
                "theme": {
                    "brand": "#B11623",             // Main color of the theme
                    "complementary": "#FAFAFA"      // Secondary color of the theme
                },
                "repository_url": "https://github.com/ow2-quick-app-initiative/poi-quick-app-implementations/tree/main/docs/fr/paris",
                "text_info": "This project was created by...",
                "text_acknowledge": "We would like to thanks...",
                "text_feedback": "Please let us know if you want to contribute...",
                "feedback_url": "https://ow2-quick-app-initiative.github.io/poi-quick-app/fr/paris/#contributors",
                "issue_url": "https://github.com/ow2-quick-app-initiative/poi-quick-app-implementations/issues/new?labels=fr/paris"
            },
            "pois": [
                {
                     "id" : "Q863286",
                     "name" : "Billy the Cat",
                     "description" : "Comic book by Colman  and  Desberg in 2000",
                     "lat" : 50.8534930829,
                     "lon" : 4.34524297714,
                     "images" : ["https://opendata.bruxelles.be/api/v2/catalog/datasets/comic-book-route/files/0c23baaead4ec7e0a9febe61e0633047"],
                     "type" : "comic book",
                     "urls" : [ "https://www.comics.org/series/9582/"],
                     "freebase" : "/m/079ch0",
                     "wikimedia" : "Q863286"
                },
                {
                    // Other PoI...
                }
            ]
        }
    }
}
```

##### App configuration

The app can be configured with localized texts, URLs and themes, depending on the user's locale. Each language tag in the `content` member has an object to configure and customize the application (`app` key). You can customize it through the following members:      

- `theme` (`object`) The main look-and-feel colors:  
  - `brand` (`string`) The primary color in HEX format (e.g., `#B11623`)
  - `complementary` (`string`) The secondary color in HEX format (e.g., `#FAFAFA`)
- `repository_url` (`url`) The URL to the repository where the project is hosted (where this database is maintain).
- `text_info` (`string`) Text to explain the project. It will be shown in the _about_ section.
- `text_acknowledge` (`string`) Text with acknowledgements about the project. It will be shown in the _about_ section.
- `text_feedback` (`string`) Text with information explaining how to contribute with the project. It will be shown in the _about_ section.
- `feedback_url` (`url`) A URL linking to a page where the contributors can get involved. It will be shown in the _about_ section.
- `issue_url` (`url`) A URL with customized parameters in the querystring that will be used to report the individual issues on specific items. It will be linked on the individual items. See [How to configure and handle public update proposals](#how-to-configure-and-handle-public-update-proposals) for more details.


##### Points of Interest

The PoIs are the main items of the database, described in the `poi` attribute as an array of objects with the following members: 

- `id` (`string`) Unique identifier for the item (e.g., `eiffeltower`).
- `name` (`string`) Short text with the title of the point of interest (e.g., `Eiffel Tower`).
- `lat` (`string`) Latitude component of the item coordinates in WGS84 format (e.g. `48.8651`).
- `lon` (`string`) Longitude component of the item coordinates in WGS84 format (e.g. `2.2909`).
- `type` (`string`) Short text with the type of the item, according to your own taxonomy (e.g. `sculpture`, `painting`,...).
- `images` (`array` of `url`) One or more absolute URLs with pictures about the item. These images could be external but it is recommended to host them under the same repository as the database. See [How to submit new images](#how-to-submit-new-images) for more details.
- `description` (`string`) One paragraph text with the concise description of the item.
- `more` (`string`) Extended text with more details of the item.
- `urls` (`array` of `url`) One or more absolute URLs to external related resources (e.g., to Wikipedia, official sources, etc.).
- `attributions` (`array` of `string`). List of texts with the names of the contributors to the content or images of this item, if any.


## How to configure and handle public update proposals 

By default, any user's feedback is handled through GitHub issues. In order to facilitate the moderation, issues will be classified using tags to indicate the project whose belong to. The title of the issues should contain the same name or identifier of the point of interest to avoid misunderstandings.   

You also can create an issue template to have homogeneous format (see [.github/ISSUE_TEMPLATE/update_request.md](https://github.com/ow2-quick-app-initiative/poi-quick-app-implementations/blob/main/.github/ISSUE_TEMPLATE/update_request.md)) and use it for new issues. 

In the [app configuration](#app-configuration) we configure the template URL for the issues for the project. If you want to handle issues by language, you can specify different tags or templates according to your needs. 

For instance, 

``` json
{
    //...
    "issue_url": "https://github.com/ow2-quick-app-initiative/poi-quick-app-implementations/issues/new?labels=sample&template=update_request.md&title=Update+request+of+"
    //...
}
```

Note that the application will append the name of the point of interest at the end of the URL, so the GitHub issue will contain the full name of the point of interest in the title. 


## Developers

> Do you want to contribute to the code?

Just fork the repository and start sending your contributions. The code of the quick app is in the [`/quick-app`](https://github.com/ow2-quick-app-initiative/poi-quick-app-implementations/blob/main/quick-app) folder of the repository. 

Feel free to [raise issues](https://github.com/ow2-quick-app-initiative/poi-quick-app-implementations/issues/new) on the code.

