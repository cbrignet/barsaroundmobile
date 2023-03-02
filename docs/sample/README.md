# _Heritage in…_ Your City

_This is part of the [Heritage_in Quick App Project](https://ow2-quick-app-initiative.github.io/poi-quick-app/#Theproject)._

Quick links: 
- [Repository with the methodology and docs (poi-quick-app)](https://github.com/ow2-quick-app-initiative/poi-quick-app)
- [Repository of implementations and templates (poi-quick-app-implementations)](https://github.com/ow2-quick-app-initiative/poi-quick-app-implementations)
- [Repository with the Web app code (poi-quick-app-web)](https://github.com/ow2-quick-app-initiative/poi-quick-app-web)

__Change this text according to your needs__

The _Heritage in…_ Quick App promotes cultural heritage collections in small and medium cities through open data and crowdsourcing. 

> It's free, open-source and collaborative 

## Privacy

These apps are based on open data and automatic processing of the data. The community's content is enriched and curated, so it's available to anyone who wants to get involved. Local experts are welcome to refine the definitions, names, and pictures and add new points of interest to the app.

The app doesn't collect any personal data, so relax. We won't sell anything.

Perhaps the content is inaccurate, so please [let us know](https://github.com/ow2-quick-app-initiative/poi-quick-app-implementations/issues/new) if you've spotted anything that might be enhanced. 

## Get involved

> Do you want to contribute to the content?

The content for each application instance (one per city/town) is stored in a particular directory in this repository, so you can look at the existing information.

Every project has two resource types:

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

The database is in a JSON file named `data.json` in the root directory of the project. In `sample/data.json` you have an empty file you can use to start the project.

Please be sure that this document has the correct format (syntax and content). You can test it using any JSON schema validation tool against the JSON schema you can find in the repository ([schema.json](https://ow2-quick-app-initiative.github.io/poi-quick-app/schema.json)). 

This JSON document contains two main parts, represented by the main keys of the root object:

1. `meta`: metadata about the project
2. `content`: multilingual content and setup of the application (based on localized information)

#### Metadata of the project

Example of a project named `fr/paris` for the City of Paris: 

``` json
{
    "meta": {
        "app_title": "Paris Street Heritage",
        "version": 1,
        "updated": "2022-03-26",
        "source_url": "https://ow2-quick-app-initiative.github.io/poi-quick-app/fr/paris/data.json",
        "matomo_base_url": "https://matomo.pbest.me/matomo.php?idsite=1&rec=1",                
        "marketplace_url": "https://appgallery.huawei.com/app/FinalURL"
    },
    "content": {
        "en": {},
        "fr": {},
        "es": {}
    }    
}
```

- `app_title` (`string`) The title to identify the application.
- `version` (`integer`) The integer that represents the version of this document (this will be used by the app to find the latest version).
- `updated` (`date`) The date when this document was updated. 
- `source_url` (`URL`) The URI of this document on the Web.  
- `matomo_base_url` (`URL`) The base URI for a Matomo analytics instance (empty if you don't want it).
- `marketplace_url` (`URI`) The URI to the application if it is listed in a marketplace (e.g, URL to the AppGallery)

#### Content of the project

Continuing with the example of City of Paris, we can localize in any language, using the [ISO 639-1](https://www.loc.gov/standards/iso639-2/php/English_list.php) codes (assigning a two-letter code for the language). You can also specify the concrete region (using a `-` character and the concrete [ISO 3166-1](https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes) (Alpha-2 code). 

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
                    "id": "eiffeltower",
                    "lat": "48.865157817905896",
                    "lon": "2.2938185078918156",
                    "type": "tower",
                    "name": "Eiffel Tower",
                    "images": [
                        "https://ow2-quick-app-initiative.github.io/poi-quick-app/fr/paris/images/eiffeltower_1.jpg",
                        "https://ow2-quick-app-initiative.github.io/poi-quick-app/fr/paris/images/eiffeltower_2.jpg"
                    ],
                    "description": "The Tour Eiffel (French) is a iron lattice tower, named after the engineer Gustave Eiffel, whose company designed and built the tower",
                    "more": "Locally nicknamed La dame de fer (French for Iron Lady), it was constructed from 1887 to 1889 as the centerpiece of the 1889 World's Fair and was initially criticized by some of France's leading artists and intellectuals for its design, but it has become a global cultural icon of France and one of the most recognizable structures in the world",
                    "attributions": [
                        "John Smith", 
                        "Jane Doe"
                    ], 
                    "urls": [
                        "https://en.wikipedia.org/wiki/Eiffel_Tower"
                    ]
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

You also can create an issue template to have homogeneous format (see [.github/ISSUE_TEMPLATE/update_request.md](../.github/ISSUE_TEMPLATE/update_request.md)) and use it for new issues. 

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

Just fork the repository and start sending your contributions. The code of the quick app is in the [`/quick-app`](../quick-app) folder of the repository. 

Feel free to [raise issues](https://github.com/ow2-quick-app-initiative/poi-quick-app-implementations/issues/new) on the code.

