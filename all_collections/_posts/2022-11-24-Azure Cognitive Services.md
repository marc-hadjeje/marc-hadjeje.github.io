---
layout: post
title: Custom Translator via Azure Cognitive Service  
date: 2022-11-23
categories: [AI,Cognitives Service, Translator,Custom Dictionnary,1st post]
---

I'm interested in AI topics for a customer via the usage of Translator which is part of the [Azure Cognitive services](https://azure.microsoft.com/fr-fr/products/cognitive-services/) on Azure, in a simple way it's a tool that allows you to translate your texts. It's easy to use, it allows you to integrate it into your backoffice to translate your texts with a lot of sample code allowing you to accelerate your development and you can test it quite easily through the Azure portal or via APIs (in my post via [postman](https://www.postman.com/)). There is a lot of documentation on this service, however it is possible to improve the translations that Microsoft offers, using an additional portal called [custom translator](https://portal.customtranslator.azure.ai/) to refine your translation for example train your model of translation or by the addition of a [custom dictionary](https://learn.microsoft.com/en-us/azure/cognitive-services/translator/custom-translator/concepts/dictionaries) allowing you to add the vocabularies of your company.
In this post we will quickly explain how to use the translation service but especially the usage of custom translator via the addition of custom dictionnary.

##### 1.Creation of the service Cognitives | Translator Ressource 

![Creation translator](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/Translator_creation.jpg?raw=true)

###### Test the service with postman and call the API 

-	Use the following url by creating a new request "Post": 

**https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=xx&to=xx**

Replace the **xx** from the URL by the [language code](https://learn.microsoft.com/en-us/azure/cognitive-services/translator/language-support) of your choice,it is possible to directly detect the language to be translated by removing the **"from"** parameter:

**https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=xx**


###### Add in the header when calling the API the 3 parameters concerning your cognitive services

-	Ocp-Apim-Subscription-Key :  **this is the key to the service** 

![Param cognitive](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/key_translator.jpg?raw=true)

-	Content-Type : **this is the extraction format, leave it as is**
-	Ocp-Apim-Subscription-Region : **this is the region where you have deployed the service**

Add the text to be translated to the body tab of postman :

```api
// API code with syntax highlighting.
{"Text":" your text"}]
}; 
```
![Text translator](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/text_translator.jpg?raw=true)

-   You will see the translation in the body as a result

![Translation result](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/translation_result.jpg?raw=true)


##### 2.Test postman with a translation include your "own dictionary”

-	To create your company dictionary you need to log in to the portal : [customtranslator](https://portal.customtranslator.azure.ai/workspaces) which is an additional portal to the service allowing you to refine your translation

-	Create a new workspace and link it to your translator service with the same information as in the API call:

![new project translator](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/new_project_translator.jpg?raw=true)

######	Create new project link to your ressource 

-	Create document set in order to download Dictionnary set  ( you can choose Phrase Dictionnary or Sentence Dictionnary )  , please read [the documentation](https://learn.microsoft.com/en-us/azure/cognitive-services/Translator/custom-translator/concepts/dictionaries) to understand the difference.
Respect the partern of the file name when uploading the dictionary 

**example: filename_en.TXT**

![new translator](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/new_project_translator.jpg)

-  You have to upload 2 files the text to be translated and the expected translation

![new translator](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/new_project_translator.jpg)

-	Train model with your dictionary document set , you will find kpis with the number for sentences ,   It can take a couple of time to train a new model.

 ![traim model translator](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/train_model.jpg?raw=true)
 
=> Publish the model in the right region

 ![publish translator](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/publish_model2.jpg?raw=true)

Once you have published the model, you can call the api by adding the categoryid you find in the published model

https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&category=<category-Id>

you will find category inside model detail: 

https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&category=<category-Id>

I replaced really by “tatata” in my dictionnary

example :
 
  ![Result translator](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/result.jpg?raw=true)

