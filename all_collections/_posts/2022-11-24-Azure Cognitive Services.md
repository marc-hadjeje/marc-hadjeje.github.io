---
layout: post
title: Custom Translator via Azure Cognitive Service  
date: 2022-11-23
categories: [AI,Cognitives Service, Translator,Custom Dictionnary,1st post]
---

I'm interested in AI topics via the usage of Translator which is part of the [Azure Cognitive services](https://azure.microsoft.com/fr-fr/products/cognitive-services/) on Azure, in a simple way it's a tool that allows you to translate your texts. It's easy to use, it allows you to integrate it into your backoffice with a lot of sample code allowing you to accelerate your development and you can test it quite easily through the Azure portal or via APIs (in my post via [postman](https://www.postman.com/)). There is a lot of documentation on this service, however it is possible to improve the translations that Microsoft offers, using an additional portal called [custom translator](https://portal.customtranslator.azure.ai/) to refine your translation for example train your model of translation or by the addition of a [custom dictionary](https://learn.microsoft.com/en-us/azure/cognitive-services/translator/custom-translator/concepts/dictionaries) allowing you to add the vocabularies of your company.

##### 1.Creation of the service Cognitives | Translator Ressource 

![Text translator](https://raw.githubusercontent.com/marc-hadjeje/marc-hadjeje.github.io/main/assets/images/text_translator.jpg)

###### Test the service with postman and call the API 

-	Use the following url by creating a new request "Post": 

 https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=xx&to=xx

 Or it is possible to directly detect the language to be translated by removing the "from" parameter

https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=xx

![Text translator](https://raw.githubusercontent.com/marc-hadjeje/marc-hadjeje.github.io/main/assets/images/text_translator.jpg)


###### Add in the header when calling the API the 3 parameters concerning your cognitive services

-	Ocp-Apim-Subscription-Key :  this is the key to the service 

![Text translator](https://raw.githubusercontent.com/marc-hadjeje/marc-hadjeje.github.io/main/assets/images/text_translator.jpg)

-	Content-Type : this is the extraction format, leave it as is
-	Ocp-Apim-Subscription-Region : this is the region where you have deployed the service

Add the text to be translated to the body tab of postman :

```api
// API code with syntax highlighting.
{"Text":" your text"}]
}; 
```
![Text translator](https://raw.githubusercontent.com/marc-hadjeje/marc-hadjeje.github.io/main/assets/images/text_translator.jpg)

######	you will see the translation in the body as a result

![Text translator](https://raw.githubusercontent.com/marc-hadjeje/marc-hadjeje.github.io/main/assets/images/text_translator.jpg)


###### 2st step, test postman with a translation “with the dictionary”

-	To create your company dictionary you need to log in to the portal: https://portal.customtranslator.azure.ai/workspaces
-	Create a new workspace and link it to your translator service with the same information as in the API call:

![Text translator](https://raw.githubusercontent.com/marc-hadjeje/marc-hadjeje.github.io/main/assets/images/text_translator.jpg)

######	Create new project with source language and target language and category ( it will be useful to call API with custom dictionary)


	Create document set in order to download Dictionnary set  ( you can choose Phrase Dictionnary or Sentence Dictionnary )  , please read the documentation to understand the difference 
Respect the partern of the file name when uploading the dictionary 
example: filename_en.TXT

	Train model with your dictionary document set , you will find kpis with the number for sentences ,   It can take a couple of time to train a new model.

 ![Text translator](https://raw.githubusercontent.com/marc-hadjeje/marc-hadjeje.github.io/main/assets/images/text_translator.jpg)
 
=> Publish the model in the right region

 ![Text translator](https://raw.githubusercontent.com/marc-hadjeje/marc-hadjeje.github.io/main/assets/images/text_translator.jpg)

	Once you have published the model, you can call the api by adding the categoryid you find in the published model

https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&category=<category-Id>

![Text translator](https://raw.githubusercontent.com/marc-hadjeje/marc-hadjeje.github.io/main/assets/images/text_translator.jpg)

I replaced really by “tatata” in my dictionnary

example :
 
