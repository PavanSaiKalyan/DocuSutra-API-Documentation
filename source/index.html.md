---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  -  shell: CURL
  -  ruby : PHP
  -  python: Node.js
  - json

toc_footers:
  # - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>
  - <a href ='https://docparser.com/'>Discover Parser</a>
  - <a href ='https://app.docparser.com/account/login/?red='>Visit Docparser App</a>
  - <a href='https://docparser.com/legal/terms-of-service'>Terms of Service</a>

includes:
  # - errors

search: true

code_clipboard: true
---
# Get Started

## Introduction
Welcome to the API of Docparser! You can use this API to

* List Document Parsers created with Docparser
* Load documents to a Document Parser
* Obtain your parsed data

 The Docparser API is organized around REST principles. Our API has predictable, resource-oriented URLs, and uses clear response messages to indicate API errors.

The code examples in the right sidebar are designed to show you how to call our API. All you need to do is to replace the secret_api_key in the sample with your [private API token](https://app.docparser.com/account/login/?red=myaccount%2Fapi).

This documentation was last updated 2021-04-29.

## Client Libraries (SDKs)
Docparser comes with two official client libraries to make it easier for you to build an integration with Docparser.

Both client libraries are open source and are published under the MIT license. Which means that you can use them in your projects without any restrictions. If you want to contribute to the development of our libraries, please don’t hesitate to create a pull-requests in our Github repositories.
<aside>
Please note that the Docparser API can be used with any programming language capable of making HTTPS calls - even if there is no ready-to-use client library available. In case you developed a library in another programming language than the ones listed below, we would be thrilled to list it here as open source.
</aside>

### Official Libraries

* PHP Client Library
* Node.js Client Library

### Third Party Libraries And Code Snippets

* Salesforce Apex Code Snippets
* Coldfusion Library

# Authentication 
Every request to the Docparser API needs to be authenticated with a secret API key linked to your account. You can obtain and reset your secret API key in the API Settings of your Docparser Account. Your API key carries many privileges, so be sure to keep them secret!

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. [API requests](https://app.docparser.com/account/login/?red=myaccount%2Fapi) without authentication will also fail.

Authentication to the API can be performed in two ways:

* via HTTP Basic Auth (recommended)
* by directly providing your API key in your request

You can test if the authentication works by pinging the following URL. Please make sure to include the correct authentication headers / parameters as described below.

`GET https://api.docparser.com/v1/ping`



## HTTP Basic Auth

```shell
curl https://api.docparser.com/v1/ping \
   -u <secret_api_key>:
```

```ruby
```

```python
```

```json
{"msg": "pong"}
```
This authentication method is the preferred way of authenticating your requests to Docparser. When using HTTP Basic Auth, use your secret API key as the “username” and leave the “password” blank.



## With API Key

```shell
curl https://api.docparser.com/v1/ping -H 'api_key: <secret_api_key>'

curl https://api.docparser.com/v1/ping?api_key=<secret_api_key>
```

```ruby
require('./vendor/autoload.php');
use Docparser\Docparser;
$docparser = new Docparser("secret_api_key");
echo $docparser->ping();
```

```python
var docparser = require('docparser-node');

var client = new docparser.Client("secret_api_key");

client.ping()
  .then(function() {
    console.log('authentication succeeded!')
  })
  .catch(function(err) {
    console.log('authentication failed!')
  });
```

```json
{"msg": "pong"}
```

In case Basic Auth is not an option for you, it is also possible to include your secret API key directly within your request. You can provide your API key either as a header (api_key: ABC123), a post-field (api_key=ABC123) or an URL query parameter (&api_key=ABC123).


 Please note that including your API as an URL query parameter is the least secure method and we don’t recommend doing this. Including API keys in URLs comes with a high risk of accidentally exposing them to others.


# Parsers

## List Document Parsers

```shell
curl https://api.docparser.com/v1/parsers \
   -u <secret_api_key>:
```
```ruby
     $docparser->getParsers();
```
``` python
 client.getParsers()
    .then(function (parsers) {
        console.log(parsers)
    })
    .catch(function (err) {
        console.log(err)
    })
```
```json
[{
  "id":"mwekrupomwekrupo",
  "label":"Acme Inc. Invoice Parser"
},{
  "id":"cadqtvgjcadqtvgj",
  "label":"Acme Corp. Invoice Parser"
}]
```

This endpoint returns a list of all Document Parsers linked to your account. Each entry contains an id and a label. The id value an be used in other API routes, e.g. for importing documents to a specific Document Parser or obtaining parsing results.

`GET https://api.docparser.com/v1/parsers`



## List Parser Model Layouts
```shell
curl https://api.docparser.com/v1/parser/models/<PARSER_ID> \
   -u <secret_api_key>:
```

```ruby
```

```python
```

```json
[{
  "id":"1",
  "label":"Acme Inc. Invoice Parser Layout #1"
},{
  "id":"2",
  "label":"Acme Inc. Invoice Parser Layout #2"
}]
```
This endpoint returns a list of all the Model Layouts for a specific parser linked to your account.

`GET https://api.docparser.com/v1/parser/models/<PARSER_ID>`



# Documents

## Import Documents

We offer several options to import a document to Docparser to make it as easy as possible for you to integrate Docparser in your existing workflow.

Next to manually uploading your documents with our app, Docparser also allows you to import documents using this API. You can upload your document with a HTTP POST request, or by providing a publicly accessible URL which can be used to fetch the document.

Hint: Another easy way of importing your documents is to forward them by e-mail to a private email inbox linked to your account. You can learn more about this method in the settings of your Document Parser.

### Upload Document From Local Path

```shell
curl \
  -u <secret_api_key>: \
  -F "file=@/home/your/local/file.jpdf" \
  https://api.docparser.com/v1/document/upload/<PARSER_ID>
```

```ruby
$docparser->uploadDocumentByPath($parserId, $filePath, $remoteId = null);

```

```python
client.uploadFileByPath('PARSER_ID', './test.pdf', {remote_id: 'test'})
  .then(function (result) {
    console.log(result)
  })
  .catch(function (err) {
    console.log(err)
  })
```

```json
{
    "id" : "abc123efg456",
    "file_size" : 119540,
    "quota_used" : 642,
    "quota_left" : 258,
    "quota_refill" : "2017-05-02T02:43:48+00:00"
}
```

Docparser allows you to upload documents from your local hard-drive with a multipart/form-data request. This is the same type of request a HTML form with a file upload field would send. The field name used for the document upload needs to be `file`.

The return value of a successful upload is the ID of the newly created document, the filesize of the imported document as well as account usage data.

Each of your Document Parsers has a unique API route to which you need to send your request. The `<PARSER_ID>` shown in the URL below can be obtained by calling the `List Parsers` API route. You can also easily obtain the `<PARSER_ID>` inside the Docparser app in the settings of your Document Parser under `Settings > API.`

In addition, you can submit an arbitrary string to Docparser which will be stored together with the uploaded document. The submitted value (`remote_id`) will be kept throughout the processing and will be available later once you obtain the parsed data with our API, as CSV/XLS/XML file or through webhooks. This optional parameter makes it easy to relate the parsed data returned by Docparser with document records in your own system.

`POST https://api.docparser.com/v1/document/upload/<PARSER_ID>`

Parameter | Description
--------- | -----------
file | The file object to upload
remote_id | Optional parrameter to pass through your own document ID.

### Upload Document By Content

```shell
curl \
  -u <secret_api_key>: \
  -F "file=@/home/your/local/file.jpdf" \
  https://api.docparser.com/v1/document/upload/<PARSER_ID>
```

```ruby
$docparser->uploadDocumentByPath($parserId, $filePath, $remoteId = null);
```

```python
client.uploadFileByPath('PARSER_ID', './test.pdf', {remote_id: 'test'})
  .then(function (result) {
    console.log(result)
  })
  .catch(function (err) {
    console.log(err)
  })
```

```json
{
    "id" : "abc123efg456",
    "file_size" : 119540,
    "quota_used" : 642,
    "quota_left" : 258,
    "quota_refill" : "2017-05-02T02:43:48+00:00"
}
```

Alternatively to uploading a document from your hard drive, you can also send files in using a simple form-data HTTP POST request. To make this work, name your form field `file_content` and use base64 encoding for safe delivery of the data. The document name can be transferred in a second form field called `file_name.`

The return value of a successful upload is the ID of the newly created document, the filesize of the imported document as well as account usage data.

Each of your Document Parsers has a unique API route to which you need to send your request. The `<PARSER_ID>` shown in the URL below can be obtained by calling the `List Parsers` API route. You can also easily obtain the `<PARSER_ID>` inside the Docparser app in the settings of your Document Parser under `Settings > API.`

In addition, you can submit an arbitrary string to Docparser which will be stored together with the uploaded document. The submitted value (`remote_id`) will be kept throughout the processing and will be available later once you obtain the parsed data with our API, as CSV/XLS/XML file or through webhooks. This optional parameter makes it easy to relate the parsed data returned by Docparser with document records in your own system.

`POST https://api.docparser.com/v1/document/upload/<PARSER_ID>`

Parameter | Description 
--------- | -----------
file_content | The file content encoded with base64.
file_name | The file name for this document. This parameter is optional and we will attribute a file-name based on the time of uploading if empty.
remote_id | Optional parameter to pass through your own document ID.



### Fetch Document From URL

```shell
curl \
  -u <secret_api_key>: \
  -F "file_content=....&file_name=...." \
  https://api.docparser.com/v1/document/upload/<PARSER_ID>
```

```ruby
$docparser->uploadDocumentByPath($parserId, $filePath, $remoteId = null);
```

```python
client.uploadFileByPath('PARSER_ID', './test.pdf', {remote_id: 'test'})
  .then(function (result) {
    console.log(result)
  })
  .catch(function (err) {
    console.log(err)
  })
```

```json
{
    "id" : "abc123efg456",
    "file_size" : 119540,
    "quota_used" : 642,
    "quota_left" : 258,
    "quota_refill" : "2017-05-02T02:43:48+00:00"
}
```

If your files are stored under a publicly accessible URL, you can also import a document by providing the URL to our API. This method is really straight forward and you just need to perform a simple POST or GET request with `url` as the parameter.

The return value of a successful fetch request is the ID of the newly created document, as well as account usage data.

Each of your Document Parsers has a unique API route to which you need to send your request. The `<PARSER_ID>` shown in the URL below can be obtained by calling the `List Parsers` API route. You can also easily obtain the `<PARSER_ID>` inside the Docparser app in the settings of your Document Parser under `Settings > API.`

In addition, you can submit an arbitrary string to Docparser which will be stored together with the fetched document. The submitted value (`remote_id`) will be kept throughout the processing and will be available later once you obtain the parsed data with our API, as CSV/XLS/XML file or through webhooks. This optional parameter makes it easy to relate the parsed data returned by Docparser with document records in your own system.

`POST https://api.docparser.com/v1/document/fetch/<PARSER_ID>`

`GET https://api.docparser.com/v1/document/fetch/<PARSER_ID>`

parameter | Description
----------|-------------
url | The location of a publicly accessible document
remote_id | Optional parameter to pass through your own document ID



# Parsed Data

Docparser provides a couple of different ways to obtain the data parsed from your documents. Basically, you have the following three options:

* Create permanent [download links](https://support.docparser.com/article/1250-what-are-the-options-for-downloading-parsed-data)
* Send parsed data to your API with [webhooks](https://support.docparser.com/article/1252-what-are-webhooks-and-cloud-integrations)
* Fetch parsed data with this API

## Get Data of One Document

```shell
curl \
  -u <secret_api_key>: \
  https://api.docparser.com/v1/results/<PARSER_ID>/<DOCUMENT_ID>
```

```ruby
$docparser->getResultsByDocument($parserId, $documentId, $format = 'object');
```

```python
client.getResultsByDocument(parserId, documentId, {format: 'object'})
  .then(function (result) {
    console.log(result)
  })
  .catch(function (err) {
    console.log(err)
  })
```

```json
[
    {
        "id": "967bcf5658d73c80563072373d5002e3",
        "document_id": "1d35639d4b53b59e77f737c93cd1d3d7",
        "remote_id": "your_optional_id",
        "file_name": "pdf.pdf",
        "media_link": "https://api.docparser.com/v1/document/media/...",
        "media_link_original": "https://api.docparser.com/v1/document/media/.../original",
        "media_link_data": "https://api.docparser.com/v1/document/media/.../data",
        "page_count": 4,
        "uploaded_at": "2016-07-27T14:57:05+00:00",
        "processed_at": "2016-07-27T14:57:10+00:00",
        "purchase_number": "ABC123",
        "customer": {
            "last_name" : "Doe",
            "first_name" : "John"
        },
        "table_data": [{
            "key" : "value"
        }, {
            "key" : "value"
        },
        ...
        ],
        "....": "...."
    }
]
```

This API route returns the parsed data of one document. The response structure is identical to the `list` route above, only that the contains a single object representing the data of the requested document.
<aside class="notice">
 A much better way than polling our API for parsed data is to use our `Webhook` integration. By using webhooks, parsed data will be pushed to your API immediately after parsing. You’ll find the webhook integration in your Document Parser under ‘Integrations > Advanced Webhook’.
</aside>


 The `<PARSER_ID>` shown in the URL below can be obtained by calling the List Parsers API route. You can also easily obtain the `<PARSER_ID>` inside the Docparser app in the settings of your Document Parser under `Settings > API`.

The `<DOCUMENT_ID>` is returned when uploading/importing a new document.

`GET https://api.docparser.com/v1/results/`<PARSER_ID>/<DOCUMENT_ID>``

Parameter | Default | Description
------|-----|------
format | object | Valid values are `object` or `flat`. By default, parsed document data is returned as nested JSON objects. Setting this parameter to flat will return a simplified version of the parsed data which does not contain flat key/value pairs instead of nested objects.
include_chilren |  | 	If child documents were created during preprocessing (e.g. when splitting documents), setting this parameter to `true` ensures that the parsed data of all child documents is returned.



## Get Data Of Multiple Documents

```shell
curl \
  -u <secret_api_key>: \
  https://api.docparser.com/v1/results/<PARSER_ID>
```

```ruby
$docparser->getResultsByParser($parserId, $options = []
```

```python
client.getResultsByParser(parserId, {format: 'object'})
  .then(function (result) {
    console.log(result)
  })
  .catch(function (err) {
    console.log(err)
  })
```

```json
[
    {
        "id": "967bcf5658d73c80563072373d5002e3",
        "document_id": "1d35639d4b53b59e77f737c93cd1d3d7",
        "remote_id": "your_optional_id",
        "file_name": "pdf.pdf",
        "media_link": "https://api.docparser.com/v1/document/media/...",
        "media_link_original": "https://api.docparser.com/v1/document/media/.../original",
        "media_link_data": "https://api.docparser.com/v1/document/media/.../data",
        "page_count": 1,
        "uploaded_at": "2016-07-27T14:57:05+00:00",
        "processed_at": "2016-07-27T14:57:10+00:00",
        "purchase_number": "ABC123",
        "customer": {
            "last_name" : "Doe",
            "first_name" : "John"
        },
        "table_data": [{
            "key" : "value"
        }, {
            "key" : "value"
        },
        ...
        ],
        "....": "...."
    },
    {
       ....
    }
]
```

This API route returns a list of JSON objects representing the parsed data. By default, the data of the last 100 documents in reverse chronological order. Additional parameters can be used to change this default behaviour.

<aside class="notice">
 A much better way than polling our API for parsed data is to use our `Webhook` integration. By using webhooks, parsed data will be pushed to your API immediately after parsing. You’ll find the webhook integration in your Document Parser under 'Integrations > Advanced Webhook’.
 </aside>

The `<PARSER_ID>` shown in the URL below can be obtained by calling the List Parsers API route. You can also easily obtain the `<PARSER_ID>` inside the Docparser app in the settings of your Document Parser under `Settings > API.`

`GET https://api.docparser.com/v1/results/<PARSER_ID>`

Parameter | Default |Description
------|-----|-----
format | object | Valid values are `object` or `flat`. By default, parsed document data is returned as nested JSON objects. Setting this parameter to flat will return a simplified version of the parsed data which contains `flat` key/value pairs instead of nested objects. 
list | last_uploaded | Valid values are `last_uploaded`, `uploaded_after` and `processed_after`. By default, the data of the last uploaded documents in reverse chronological order is returned. If set to `uploaded_after`, documents imported after a certain date are returned (see below). If set to `processed_after`, documents parsed after a certain date are returned (see below).
limit | 100 | This parameter indicates how many documents to include when the parameter `list` is set to `last_uploaded`. The maximum quantity of documents which can be returned is limited 10,000.
date | | This parameter is mandatory if the parameter list is set to `uploaded_after` or `processed_after`. The parameter needs to be a valid ISO 8601 (e.g. 2017-02-12T15:19:21+00:00) date string or a Linux timestamp and determines which documents are included in the return. Please note that the maximum quantity of documents which can be returned is limited 10,000.
remote_id | | When this parameter is set, only documents having the provided `remote_id` will be returned. The `remote_id` of a document can be set when importing the file via the API [(see above)](https://dev.docparser.com/#import-documents).
include_processing_queue| |By default, only documents which are fully processed (imported, preprocessed, parsed) are included in the results. By setting `include_processing_queue` to `true`, files which are not yet entirely processed are included in the results. 

