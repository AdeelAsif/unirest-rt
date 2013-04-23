This is a port of the Unicorn library to .NET. This is a work in progress and presented as-is. I am not affiliated with the original Unicorn library. The official Readme is presented below and may or may not have anything to do with this port.

--------

Unicorn-Java
============================================

Unicorn is a set of lightweight HTTP libraries available in PHP, Ruby, Python, Java, Objective-C.

Documentation
-------------------

### Installing
Is easy as pie. Kidding. It's about as easy as doing these little steps:

Using with Maven by adding the Mashape repository:

```xml
<repository>
    <id>mashape-releases</id>
    <url>http://maven.mashape.com/releases</url>
</repository>
```

and including the library:

```xml
<dependency>
    <groupId>com.mashape.unicorn</groupId>
    <artifactId>unicorn-java</artifactId>
    <version>1.0.0</version>
</dependency>
```

There are dependencies for the Java library, these should be already installed, and they are as follows:

```xml
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.2.3</version>
</dependency>
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpmime</artifactId>
    <version>4.2.3</version>
</dependency>
<dependency>
    <groupId>org.json</groupId>
    <artifactId>json</artifactId>
    <version>20090211</version>
</dependency>
```



### Creating Request
So you're probably wondering how using Unicorn makes creating requests in Java easier, here is a basic POST request that will explain everything:

```java
HttpResponse<JsonNode> jsonResponse = Unicorn.post("http://httpbin.org/post")
  .header("accept", "application/json")
  .field("parameter", "value")
  .field("foo", "bar")
  .asJson();
```

Requests are made when `as[Type]()` is invoked, possible types include `Json`, `Binary`, `String`. If the request supports and it is of type `HttpRequestWithBody`, a body it can be passed along with `.body(String|JsonNode)`. If you already have a map of parameters or do not wish to use seperate field methods for each one there is a `.fields(Map<String, Object> fields)` method that will serialize each key - value to form parameters on your request.

`.headers(Map<String, String> headers)` is also supported in replacement of multiple header methods.

### Asynchronous Requests
Sometimes, well most of the time, you want your application to be asynchronous and not block, Unicorn supports this in Java using anonymous callbacks, or direct method placement:

```java
Thread thread = Unicorn.post("http://httpbin.org/post")
  .header("accept", "application/json")
  .field("param1", "value1")
  .field("param2", "value2")
  .asJson(new Callback<JsonNode>() {
    public void completed(HttpResponse<JsonNode> response) {
      int code = response.getCode();
      Map<String, String> headers = response.getHeaders();
      JsonNode body = response.getBody();
      InputStream rawBody = response.getRawBody();
    }
  });
```

### File Uploads
Creating `multipart` requests with Java is trivial, simply pass along a `File` Object as a field:

```java
HttpResponse<JsonNode> jsonResponse = Unicorn.post("http://httpbin.org/post")
  .header("accept", "application/json")
  .field("parameter", "value")
  .field("file", new File("/tmp/file"))
  .asJson();
```

### Custom Entity Body

```java
HttpResponse<JsonNode> jsonResponse = Unicorn.post("http://httpbin.org/post")
  .header("accept", "application/json")
  .body("{\"parameter\":\"value\", \"foo\":\"bar\"}")
  .asJson();
```

### Request Reference

The Java Unicorn library follows the builder style conventions. You start building your request by creating a `HttpRequest` object using one of the following:

```java
HttpRequest request = Unicorn.get(String url);
HttpRequestWithBody request = Unicorn.post(String url);
HttpRequestWithBody request = Unicorn.put(String url);
HttpRequestWithBody request = Unicorn.patch(String url);
HttpRequest request = Unicorn.delete(String url);
```

### Response Reference

Upon recieving a response Unicorn returns the result in the form of an Object, this object should always have the same keys for each language regarding to the response details.

`.getCode()`  
HTTP Response Status Code (Example 200)

`.getHeaders()`  
HTTP Response Headers

`.getBody()`  
Parsed response body where applicable, for example JSON responses are parsed to Objects / Associative Arrays.

`.getRawBody()`  
Un-parsed response body



License
---------------

The MIT License

Copyright (c) 2013 Mashape (http://mashape.com)

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
