---
layout: post
title: Synapse security 
date: 2023-05-10
categories: [DATA,Synapse, Security]
---
For my second post, I'm going to focus on a subject that is of major concern to many of my clients, data security inside their Data platform.
Beyond the network security aspects, what preoccupies companies are the aspects around data exfiltration which is a very considered threat by the security team.
We will address this topic through the usage of Microsoft Synapse, the Microsoft data platform.
Before briefly presenting the Synapse architecture and the possible protection capabilities, you will find below the definition of what is data exfiltration:
Data exfiltration, aka data leakage, aka data extrusion, occurs when sensitive data is exfiltrated to locations outside of an organization.
In fact, there are two types of data exfiltration threats: attacks from outside the organization, and malicious insiders stealing their organization.
In the public cloud business, data exfiltration usually stands for the second scenario: a malicious insider, who has legitimate access to sensitive data, is exfiltrating it to locations outside of the organization.

# Presentation de synapse 

Azure Synapse is an enterprise analytics service that accelerates time to insight across data warehouses and big data systems. Azure Synapse brings together the best of SQL technologies used in enterprise data warehousing, Spark technologies used for big data, Data Explorer for log and time series analytics, Pipelines for data integration and ETL/ELT, and deep integration with other Azure services such as Power BI, CosmosDB, and AzureML.

**1st step, test postman with translation _without the dictionary_**

	Use the following url by creating a new request "Post":  https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=xx&to=xx
xx being the iso codes of the languages to be translated

	Add in the header when calling the API the 3 parameters concerning your cognitive services

 

	Ocp-Apim-Subscription-Key :  this is the key to the service 

 


	Content-Type : this is the extraction format, leave it as is
	Ocp-Apim-Subscription-Region : this is the region where you have deployed the service

	Add the text to be translated to the body tab of postman : {"Text":" your text"}]

 

	you will see the translation in the body as a result
 

2st step, test postman with a translation “with the dictionary”

	To create your company dictionary you need to log in to the portal: https://portal.customtranslator.azure.ai/workspaces
	Create a new workspace and link it to your translator service with the same information as in the API call:

 

	Create new project with source language and target language and category ( it will be useful to call API with custom dictionary)


 


	Create document set in order to download Dictionnary set  ( you can choose Phrase Dictionnary or Sentence Dictionnary )  , please read the documentation to understand the difference 
Respect the partern of the file name when uploading the dictionary 
example: filename_en.TXT

	Train model with your dictionary document set , you will find kpis with the number for sentences ,   It can take a couple of time to train a new model.

 
 
=> Publish the model in the right region

 


 

	Once you have published the model, you can call the api by adding the categoryid you find in the published model

https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&category=<category-Id>

I replaced really by “tatata” in my dictionnary

example :
 


[Link to another page](./another-page.html).

There should be whitespace between paragraphs.

There should be whitespace between paragraphs. We recommend including a README, or a file with information about your project.

#### Image

Images can be clicked to enlarge :)

![Dummy Image 1](https://picsum.photos/1366/768)
![Dummy Image 2](https://picsum.photos/1200/400)

# Header 1

This is a normal paragraph following a header. GitHub is a code hosting platform for version control and collaboration. It lets you and others work together on projects from anywhere.

## Header 2

> This is a blockquote following a header.
>
> When something is important enough, you do it even if the odds are not in your favor.

### Header 3

> This is another blockquote following header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require("./lang/" + l);
  return true;
};
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### Header 4

- This is an unordered list following a header.
- This is an unordered list following a header.
- This is an unordered list following a header.

##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
| :----------- | :---------------- | :---- |
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

---

### Here is an unordered list:

- Item foo
- Item bar
- Item baz
- Item zip

### And an ordered list:

1.  Item one
    1.  Item one
    1.  Item two
        1.  Item one
        1.  Item two
        1.  Item three
    1.  Item three
1.  Item four
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long.
```

```
The final element.
```
