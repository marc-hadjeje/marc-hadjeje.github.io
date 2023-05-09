---
layout: post
title: Synapse Analytics DEP
date: "2023-05-09"
categories: ["Data", "Analytics", "Security"]
---
For my second post, I'm going to focus on a subject that is of major concern to many of my clients, data security inside their Data platform.
Beyond the network security aspects, what preoccupies companies are the aspects around data exfiltration which is a very considered threat by the security team.
We will address this topic through the usage of [Microsoft Azure Synapse Analytics](https://azure.microsoft.com/en-us/products/synapse-analytics/){:target="_blank"} , the Microsoft data platform.
Before briefly presenting the Synapse architecture and the possible protection capabilities, you will find below the definition of what is data exfiltration:
Data exfiltration, aka data leakage, aka data extrusion, occurs when sensitive data is exfiltrated to locations outside of an organization.
In fact, there are two types of data exfiltration threats: attacks from outside the organization, and malicious insiders stealing their organization.
In the public cloud business, data exfiltration usually stands for the second scenario: a malicious insider, who has legitimate access to sensitive data, is exfiltrating it to locations outside of the organization.

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


##### 2.Create custom translation project that includes your "own dictionary”

After several translation tests my customer would like to add his own translation on certain words, because he considered that the translation of the service was not always the best or wanted to add specific vocabulary from his company.  

-	To create your company dictionary you need to log in to into the portal : [custom translator](https://portal.customtranslator.azure.ai/workspaces){:target="_blank"} which is an additional portal to the service allowing you to improve your translation.

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

-	Train model with your dictionary document set, you will find some information with the number for sentences,It can take a couple of times to train a new model.

 ![traim model translator](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/train_model.jpg?raw=true)
 
-    Publish the model in the right region

 ![publish translator](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/publish_model2.jpg?raw=true)

Once you have published the model, you can call the api by adding the categoryid you will find in the published model :

 ![traim model translator](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/category_id.jpg?raw=true)

Now you can use the API below by adding the categoryid corresponding to your custom dictionary (with the same source and target language in the url of your API) and you will see your custom translation !

**https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=xx&category=category-Id**


In my example below I am translating from Chinese Traditional into English and add category-id to add custom dictionary to the translation:

 ![traim model final result](https://github.com/marc-hadjeje/marc-hadjeje.github.io/blob/main/assets/images/result_custom_traduction.jpg?raw=true)
