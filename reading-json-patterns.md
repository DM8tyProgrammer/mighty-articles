---
title: Patterns of Reading JSON
description: 'JSON can be read or mapped to domain objects using two broad categories of APIs: Binding (High-Level) and Streaming (Low-Level).'
keywords: json, gson, reading json, jackson, json streaming, json parsing, json binding
datePublished: '2018-06-03'
lastModified: '2020-06-18'
tags: 'json'
---

**J**ava**S**cript**O**bject**N**otion (_JSON_) is a _de-facto_ standard for exchanging data over Web API. JSON is a [recursive data structure](https://www.json.org/json-en.html) and can be visualized as a key-value pairs tree.

_There are two broad categories of APIs for reading information from JSON :_

- **Streaming Based**: It is parsing or visiting of JSON _part by part_. With each part iteration, you skip or map part to Object/s. Part maybe `{`, `[`, field, value, `:`, `}`, and `]` _(JSON Tokens!)._
  ![json-stream](https://miro.medium.com/max/1400/1*XGL1-Cf8cbK4xldmURLlzA.png)
- **Binding based**: _Binding_ is mapping of JSON field to Programming Objects; these may be domain or abstract*.*JSON is _entirely_ parsed and mapped to objects.
  ![json binding](https://miro.medium.com/max/1400/1*5vtOcLKmWxyqmMBNJwetaA.png)
  This article goes through different patterns of _Reading JSON_ data. _Java is chosen for demonstrating these patterns._

## JSON — Abstract Model Binding

JSON is mapped to the library provided objects or collections objects intermediately, then these objects are iterated for mapping with domain objects or for extracting information.
![json abstract model binding](https://miro.medium.com/max/1400/1*_wPoPRmYOhsWJsAttTHKfQ.png)
Formally, these intermediary objects are collectively called _Parsed Tree³_. The process of converting JSON to Parsed Tree is known as _parsing²_.
These objects do not adequately express business knowledge; this is the reason for calling these objects: abstract.

### In Action

Consider the extraction distance between New York and Washington from the output of a sample [Google Distance Matrix API](https://developers.google.com/maps/documentation/distance-matrix/start) :

```json
{
   “destination_addresses” : [ “New York, NY, USA” ],
   “origin_addresses” : [ “Washington, DC, USA” ],
   “rows” : [
      {
         “elements” : [
            {”distance” : {
                  “text” : “225 mi”,
                  “value” : 361715
               },
               “duration” : {
                  “text” : “3 hours 49 mins”,
                  “value” : 13725
               },
               “status” : “OK”
            }
         ]
      }
   ],
   “status” : “OK”
}
```

[GSON](https://github.com/google/gson) is one of the libraries to parse JSON. The code for extracting distance is as:

```java
JsonParser parser = new JsonParser();
// tree object structure is provided by lib.
JsonElement parsedTree = parser.parse(json);
JsonObject root = parsedTree.getAsJsonObject();
JsonObject firstRow = root.get("rows").getAsJsonArray().get(0).getAsJsonObject();
JsonObject firstElement = firstRow.get("elements").getAsJsonArray().get(0).getAsJsonObject();

// finally, distance is extracted ! Phew
String distance = firstElement.get("distance").getAsJsonObject().get("value").getAsString();

```

Repl.it → [https://repl.it/@DM8tyProgrammer/NonDomainBinding](https://repl.it/@DM8tyProgrammer/NonDomainBinding)

## JSON — Domain Model Binding

Almost whole JSON is mapped directly to equivalent Domain or Business Objects; Domain object mimics the structure of JSON with identical fields and compatible types.
![](https://miro.medium.com/max/1400/1*25PKdKqP9a2yIqJvKTfzgg.png)

### In Action

Consider Twitter’s [Tweet object](https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/tweet-object) from Web API for a demonstration of this pattern.

```json
{
 “created_at”: “Wed Oct 10 20:19:24 +0000 2018”,
 “id”: 1050118621198921728,
 “id_str”: “1050118621198921728”,
 “text”: “To make room for more expression, we will now count all emojis as equal—including those with gender‍‍‍ ‍‍and skin t… https://t.co/MkGjXf9aXm”,
 “user”: {…}
.
.
.
}
```

The equivalent class would be for the above JSON:

```java
class Tweet {

  private long id;
  private String id_str;
  private String text;
  private String source;
  private User user;
  private String created_at;

}
```

```json
{ “user”: {
    “id”: 6253282,
    “id_str”: “6253282”,
    “name”: “Twitter API”,
    “screen_name”: “TwitterAPI”,
    “location”: “San Francisco, CA”,
    “url”: “https://developer.twitter.com”,
    “description”: “The Real Twitter API. Tweets about API changes, service issues and our Developer Platform. Don’t get an answer? It’s on my website.”
.
  }
}
```

```java
class User {

  private long id;
  private String id_str;
  private String name;
  private String description;
  private String screen_name;
  private String url;
  private String location;

}
```

_You can verify: Classes mirror JSON field by field with the same name and compatible types. Some libraries offer relaxations in mappings: missing fields, unknown fields, field name mismatch, type mismatch, and conversion._

[Jackson](https://github.com/FasterXML/jackson) is a famous library for domain object mapping. Jackson provides [ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.7/com/fasterxml/jackson/databind/ObjectMapper.html) API to read or write JSON. The code of mapping is as:

```java
ObjectMapper objectMapper = new ObjectMapper();
Tweet tweet = objectMapper.readValue(tweetAsJson, Tweet.class);

// process Tweet object
```

Repl.it → [https://repl.it/@DM8tyProgrammer/jackson](https://repl.it/@DM8tyProgrammer/jackson)

## Expression Based Data Extraction

Selected data from JSON can be extracted using _JSONPath_, which is an algebraic expression for pointing fields. [XPath inspires the idea of JSONPath.](https://goessner.net/articles/JsonPath/)
Extraction using JSONPath can be described as:

1. **JSON Parsing**: JSON is parsed into Abstract Objects or _Parsed Tree._
2. **Expression Evaluation**: An expression is passed to API exposed by the previous step to query JSON.
   ![json expression steps](https://miro.medium.com/max/1400/1*NlGX4fKvzW7DONSMq9XVAg.png)

### JSONPath Expression

In _JSONPath_ context, JSON is considered as Tree. The below elements are concatenated to formulate expression:

- The root of JSON is selected by `$`.
- `.` selects the immediate child field.
- `..` selects any level child field.
- `[‘<field name>’]` or just name to select field.
- `[<number>]` to access the nth array element.
- `[*]` targets all fields of an array.

### In Action

Again, consider the extraction distance between New York and Washington from the output of a sample [Google Distance Matrix API](https://developers.google.com/maps/documentation/distance-matrix/start) :

```json
{
   “destination_addresses” : [ “New York, NY, USA” ],
   “origin_addresses” : [ “Washington, DC, USA” ],
   “rows” : [
      {
         “elements” : [
            {
				”distance” : {
                  “text” : “225 mi”,
                  “value” : 361715
               },
               “duration” : {
                  “text” : “3 hours 49 mins”,
                  “value” : 13725
               },
               “status” : “OK”
            }
         ]
      }
   ],
   “status” : “OK”
}
```

The JSON path would be: `$.rows[0].elements[0].distance.value.` JSONPath can be evaluated with [Jayway’s Jsonpath](https://github.com/json-path/JsonPath) _(notice name!)_ library*.*The code snippet for the extracting distance is as:

```java
ReadContext ctx = JsonPath.parse(distanceJson);
Double distance = ctx.read("$.rows[0].elements[0].distance.value", Double.class);
```

Repl.it → [https://repl.it/@DM8tyProgrammer/jsonpath](https://repl.it/@DM8tyProgrammer/jsonpath)

## JSON Streaming Parsing

Aforementioned, streaming JSON means iterating JSON part by part. These parts are formally known as _tokens_. These may be a single character or group of characters.
In JSON [Format Specification](https://www.crockford.com/mckeeman.html) , the following tokens are defined:

- Object Sentinels:: `{`:Start of Object, `}`: End of Object
- Array Sentinels:: `[`: Start of Array, `]`: End of Array
- Literal Types:: Number, String, Boolean (`true`, `false`), `null`.

### In Action

Consider a simple JSON to parse:

```json
{
  “name”: “Mighty”,
  “handler”: “DM8typrogrammer”
}
```

[GSON](https://github.com/google/gson) and [Jackson](https://github.com/FasterXML/jackson) both featured Streaming API. Jackson is arbitrarily chosen for demonstrating. The code of _parsing JSON through streaming tokens_ is showcased below; with each iteration data, is pushed to collections as:

```java
String json = "{\"name\": \"mighty\", \"handler\": \"DM8typrogrammer\"}";
JsonFactory jsonfactory = new JsonFactory();
JsonParser jsonParser = jsonfactory.createParser(json);

// putting values in map
Map<String, String> data = new HashMap<>();

JsonToken currentToken = jsonParser.nextToken();
while (currentToken != JsonToken.END_OBJECT) {

  if (currentToken == JsonToken.FIELD_NAME) {
    String fieldname = jsonParser.getCurrentName();
    jsonParser.nextToken(); // move to value

    // pushing data to map
    data.put(fieldname, jsonParser.getText());
  }

  // iterating token by token
  currentToken = jsonParser.nextToken();
}
jsonParser.close();

System.out.println(data);
```

Repl.it → [https://repl.it/@DM8tyProgrammer/stream-json](https://repl.it/@DM8tyProgrammer/stream-json)
The code can be understood as iterating though tape having JSON tokens:
![json streaming as ticker](https://miro.medium.com/max/1400/1*VG5vC0K49LGhh-mEdFwYFw.png)

## Closing Notes

Binding API internally _streams_ JSON for converting it into object form. It can be said Binding is a High-Level API.

Streaming, Abstract Model Binding, and Expression Based Extraction are used to*select a part of data from JSON*. Domain Model Binding method is used to extract the complete JSON information.

Expression-based JSON extraction is a *declarative variant*of Abstract Model Binding method; Instead of you writing code by hand for walking Parsed Tree, you are passing specifications to the API to extract data from JSON. It is a concise and flexible method; however, it is a slower variant.

## Footnotes

1. **Reading JSON** means _Parsing²_ of JSON and mapping the data into Domain Objects.
2. **Parsing** is a mechanism of converting formatted text into a data structure. In case of JSON parsing, the output data structure is of _Tree_ type.
3. **Parsed Tree** is the output data structure of JSON parsing.

## References

1. Introducing JSON, [https://www.json.org/json-en.html.](http://www.json.org/json-en.html.)
2. JSONPath — XPath for JSON, [https://goessner.net/articles/JsonPath.](http://goessner.net/articles/JsonPath.)
3. Douglas Crockford — [https://www.crockford.com/mckeeman.html](https://www.crockford.com/mckeeman.html)
