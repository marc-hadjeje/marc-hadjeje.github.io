---
layout: post
title: Custom Translator via Azure Cognitive Service  
date: 2022-11-23
categories: [AI,Cognitives Service, Translator,Custom Dictionnary,1st post]
---

For my first post, I'm interested in AI topics that I had to set up for a customer via the usage of Translator which is part of the [Azure Cognitive services](https://azure.microsoft.com/fr-fr/products/cognitive-services/){:target="_blank"} on Azure.In a simple way it's a tool that allows you to translate your texts. It's easy to use, it help you to integrate it into your backoffice to translate your texts with a lot of sample code allowing you to accelerate your development.You can test it quite easily through the Azure portal or via APIs (in my post via [postman](https://www.postman.com/){:target="_blank"}).There is a lot of documentation on this service, however it is possible to improve the translations that Microsoft offers, using an additional portal called [custom translator](https://portal.customtranslator.azure.ai/){:target="_blank"} to refine your translation.For example, train your model of translation or by the addition of a [custom dictionary](https://learn.microsoft.com/en-us/azure/cognitive-services/translator/custom-translator/concepts/dictionaries){:target="_blank"} enabling you to add business vocabulary.
In this post we will briefly explain how to use the translation service but especially the usage of custom translator via the addition of custom dictionary.

##### 1.Creation of the service Cognitives | Translator Ressource 

![Creation translator](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/Translator_creation.jpg?raw=true)

###### Test the service with postman and call the API 

-	Use the following url by creating a new request "Post": 

**https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=xx&to=xx**

Replace the **xx** from the URL by the [language code](https://learn.microsoft.com/en-us/azure/cognitive-services/translator/language-support){:target="_blank"} of your choice,it is possible to directly detect the language to be translated by removing the **"from"** parameter:

**https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=xx**


###### Add in the header when calling the API the 3 parameters concerning your cognitive services

-	Ocp-Apim-Subscription-Key :  **this is the key to the service** 

![Param cognitive](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/key_translator.jpg?raw=true)

-	Content-Type : **this is the extraction format, for example "application/json"**
-	Ocp-Apim-Subscription-Region : **this is the region where you have deployed the service**

![Param cognitive](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/param_cognitive.jpg?raw=true)


Add the text to be translated to the body tab of postman :

```api
// API code with syntax highlighting.
{"Text":" your text"}]
}; 
```
![Text translator](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/text_translator.jpg?raw=true)

-   In my example below I am translating from English into Chinese Traditional, the API has detected the language to be translated directly and you can see the translation in the body as a result:

**https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=zh-Hant**

![Translation result](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/translation_result.jpg?raw=true)


##### 2.Create custom translation project include your "own dictionary”

After several translation tests my customer would like to add his own translation on certain words, because he considered that the translation of the service was not always the best or wanted to add specific vocabulary from his company.  

-	To create your company dictionary you need to log in to the portal : [custom translator](https://portal.customtranslator.azure.ai/workspaces){:target="_blank"} which is an additional portal to the service allowing you to improve your translation.

-	Create a new workspace and link it to your translator ressource with the same information as in the API call:

![new workspace](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/custom_translator_key.jpg?raw=true)

-   Create new translation project link to your ressource :

![new project dico](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/new_project_translator.jpg?raw=true)

-	Create a document set in order to download Dictionary set (you can choose Phrase Dictionary or Sentence Dictionary), please read [the documentation](https://learn.microsoft.com/en-us/azure/cognitive-services/Translator/custom-translator/concepts/dictionaries){:target="_blank"} to understand the difference.

![new type of dico](https://raw.githubusercontent.com/marc-hadjeje/marc-hadjeje.github.io/main/assets/images/type_of_dictionnary.jpg)

**Respect the pattern of the file name when uploading the dictionary** 

**example: filename_en.TXT**

-  You have to upload 2 files : the text to be translated and the expected translation, for example:

![new translator](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/content%20custom%20dictionnary.jpg?raw=true)

-	Train model with your dictionary document set, you will find informations with the number for sentences,It can take a couple of time to train a new model.

 ![traim model translator](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/train_model.jpg?raw=true)
 
-    Publish the model in the right region

 ![publish translator](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/publish_model2.jpg?raw=true)

Once you have published the model, you can call the api by adding the categoryid you will find in the published model :

 ![traim model translator](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/category_id.jpg?raw=true)

Now you can use the API below by adding the categoryid corresponding to your custom dictionary (with the same source and target langage in the url of your API) and you will see your custom translation !

**https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=xx&category=category-Id**


In my example below I am translating from Chinese Traditional into English and add category-id to add custom dictionary to the translation:

 ![traim model final result](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/result_custom_traduction.jpg?raw=true)
