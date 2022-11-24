---
layout: post
title: Translator dans Azure Cognitive Service  
date: 2022-11-23
categories: [AI,Cognitives Service, Translator,Custom Dictionnary]
---

##### Creation of the service Cognitives Ressource

###### Test postman with translation _without the dictionary

	Use the following url by creating a new request "Post":  https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=xx&to=xx
xx being the iso codes of the languages to be translated

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

 
 
=> Publish the model in the right region

 


 

	Once you have published the model, you can call the api by adding the categoryid you find in the published model

https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&category=<category-Id>

I replaced really by “tatata” in my dictionnary

example :
 

