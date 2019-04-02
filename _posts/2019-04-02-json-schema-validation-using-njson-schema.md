---
title: "Json Schema Validation using NJsonSchema"
layout: single
comments: true
date: 2019/04/02
categories:
    - .Net
tags:
    - .net
    - csharp
    - json
---
Json (JavaScript Object Notation ) is a lightweight, self-describing markup, which has become a universal notation for data exchange. Json is now everywhere replacing XML as standard format. Its used as a standard format for exchanging data between the services, when we have a 3rd party application it is always a best practice to validate the incoming json with its own schema.

IDE's like *Visual studio code* verify the schema (if specified as $schema property) when you open a json file.

```json
{
    "$schema": "./schema.json",
    "name" :"something",
    ... 
}
```

Schema can be relative file as mentioned above or even a publicly accessible http url. It is always recommended that the 3rd party json should be verified with the given schema to ensure data consistency, to achieve this there are many libraries available in .Net. The best I have come across in [NJsonSchema](https://github.com/RicoSuter/NJsonSchema).

NJsonSchema is a .Net library to read, generate and validate JSON schema. Loading a json schema from a file is straight forward using this library

```csharp
//Loading schema from a string
var schemaFromString = await JsonSchema4.FromJsonAsync(schemaAsString);
//loading schema from a file
var schemaFromFile = await JsonSchema4.FromFileAsync(schemaFilePath);
//loading schema from publicly url
var schemaFromUrl = await JsonSchema4.FromUrlAsync(schemaUrl);
//loading schema from a .Net type
var schemaFromType = await JsonSchema4.FromTypeAsync(typeof(ClassYouWantToDeserialize));

//Validating the json file
var errors = schemaFromFile.Validate(jsonString);
```

The *schemaFromType* is really interesting and important use case, as we can validate the data even before deserializing it into a .net object. Errors contains a list of validation error messages specifying the line number and the token that failed validation.

Json schema files can can also reference other schema files for modularity, which could either be a relative path to another file or a publicly accessible url. All this scenarios are considered when you load the schema using FromFileAsync.

You could also write a custom reference resolver when you have a mixed mode of relative schemas. Below is one such example of custom reference resolver.

```csharp
using System.IO;
using System.Threading.Tasks;
using NJsonSchema;
using NJsonSchema.References;

namespace CustomResolver
{
    public class JsonFileReferenceResolver : JsonReferenceResolver
    {
        private readonly IHttpClient _httpClient;
        private readonly string _originalFilePath;

        public JsonFileReferenceResolver(JsonSchemaResolver schemaResolver,
            string originalFilePath,
            IHttpClient httpClient) :
            base(schemaResolver)
        {
            _httpClient = httpClient;
            _originalFilePath = originalFilePath;
        }

        public override async Task<IJsonReference> ResolveFileReferenceAsync(string filePath)
        {
            //load the file path
            var filePath = new FileInfo(filePath).Name;
            var externalJson = await _httpClient.GetFileContentAsync($"{_originalFilePath}/{filePath}");
            return await JsonSchema4.FromJsonAsync(externalJson);
        }
    }
}

///Usage of the custom reference resolver
await JsonSchema4.FromJsonAsync(mailSchemaJson,
                "C:\\temp", (schema) =>
                {
                    return new JsonFileReferenceResolver(
                        new JsonSchemaResolver(schema,
                        new NJsonSchema.Generation.JsonSchemaGeneratorSettings()),
                        "metadata", _customClient);
                });
```

In the above code the IHttpClient is a custom class which get the file content from a url, all the references to external file which is relative file paths will be replaced with base url and resolved.

Happy Coding!!!