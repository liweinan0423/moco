# Configurations
Moco mainly focuses on server configuration. There are only two kinds of configuration right now: **Request** and **Response**.

That means if we get the expected request and then return our response. Now, you can see a Moco reference in details.

**WARNING** the json configuration below is just configuration snippet for one pair of request and response, instead of the whole configuration file.

## Request

### Content
If you want to response according to request content, Moco server can be configured as following:

* API

```java
server.request(by("foo")).response("bar");
```

* JSON

```json
{
  "request" :
    {
      "text" : "foo"
    },
  "response" :
    {
      "text" : "bar"
    }
}
```

If request content is too large, you can put it in a file:

* API

```java
server.request(by(file("foo.request"))).response("bar");
```

* JSON

```json
{
  "request" :
    {
      "file" : "foo.request"
    },
  "response" :
    {
      "text" : "bar"
    }
}
```

### URI
If request uri is your major focus, Moco server could be like this:

* API

```java
server.request(by(uri("/foo"))).response("bar");
```

* JSON

```json
{
  "request" :
    {
      "uri" : "/foo"
    },
  "response" :
    {
      "text" : "bar"
    }
}
```
### Query parameter
Sometimes, your request has parameters:

* API

```java
server.request(and(by(uri("/foo")), eq(query("param"), "blah"))).response("bar")
```

* JSON

```json
{
  "request" :
    {
      "uri" : "/foo",
      "queries" : 
        {
          "param" : "blah"
        }
    },
  "response" :
    {
      "text" : "bar"
    }
}
```

### HTTP method
It's easy to response based on specified HTTP method:

* API

```java
server.get(by(uri("/foo"))).response("bar");
```

* JSON

```json
{
  "request" :
    {
      "method" : "get",
      "uri" : "/foo"
    },
  "response" :
    {
      "text" : "bar"
    }
}
```

Also for POST method:

* API

```java
server.post(by("foo")).response("bar");
```

* JSON

```json
{
  "request" :
    {
      "method" : "post",
      "text" : "foo"
    },
  "response" :
    {
      "text" : "bar"
    }
}
```

### Version
We can return different response for different HTTP version:

* API

```java
server.request(by(version("HTTP/1.0"))).response("version");
```

* JSON

```json
{
  "request": 
    {
      "version": "HTTP/1.0"
    },
  "response": 
    {
      "text": "version"
    }
}
```

### Header
We will focus HTTP header at times:

* API

```java
server.request(eq(header("foo"), "bar")).response("blah")
```

* JSON

```json
{
  "request" :
    {
      "method" : "post",
      "headers" : 
      {
        "content-type" : "application/json"
      }
    },
  "response" :
    {
      "text" : "bar"
    }
}
```

### Cookie
Cookie is widely used in web development.

* API

```java
server.request(eq(cookie("loggedIn"), "true")).response(status(200));
```

* JSON

```json
{
  "request" :
    {
      "uri" : "/cookie",
      "cookies" :
        {
          "login" : "true"
        }
    },
  "response" :
    {
      "text" : "success"
    }
}
```

### Form
In web development, form is often used to submit information to server side.

* API

```java
server.post(eq(form("name"), "foo")).response("bar");
```

* JSON


```json
{
  "request" :
    {
      "method" : "post",
      "forms" :
        {
          "name" : "foo"
        }
    },
  "response" : 
    {
      "text" : "bar"
    }
}
```

### XML
XML is a popular format for Web Services. When a request is in XML, only the XML structure is important in most cases and whitespace can be ignored. The `xml` operator can be used for this case.

* API

```java
server.request(xml(text("<request><parameters><id>1</id></parameters></request>"))).response("foo");
```

* JSON

```json
{
  "request": 
    {
      "uri": "/xml",
      "text": 
        {
          "xml": "<request><parameters><id>1</id></parameters></request>"
        }
    },
  "response": 
    {
      "text": "foo"
    }
}
```

**NOTE**: Please escape the quote in text.

The large request can be put into a file:

```json
{
   "request": 
     {
        "uri": "/xml",
        "file": 
          {
            "xml": "your_file.xml"
          }
    },
  "response": 
    {
      "text": "foo"
    }
}
```

### XPath

For the XML/HTML request, Moco allows us to match request with XPath.

* API

```java
server.request(eq(xpath("/request/parameters/id/text()"), "1")).response("bar");
```

* JSON

```json
{
  "request" :
    {
      "method" : "post",
      "xpaths" : 
        {
          "/request/parameters/id/text()" : "1"
        }
    },
  "response" :
    {
      "text" : "bar"
    }
}
```

### JSON Request
Json is rising with RESTful style architecture. Just like XML, in the most case, only JSON structure is important, so `json` operator can be used.

* API

```java
server.request(json(text("{\"foo\":\"bar\"}"))).response("foo");
```

* JSON

```json
{
  "request": 
    {
      "uri": "/json",
      "text": 
        {
          "json": "{\"foo\":\"bar\"}"
        }
    },
  "response": 
    {
      "text": "foo"
    }
}
```

**NOTE**: Please escape the quote in text.

The large request can be put into a file:

```json
{
  "request": 
    {
      "uri": "/json",
      "file": 
        {
          "json": "your_file.json"
        }
    },
  "response": 
    {
      "text": "foo"
    }
}
```

### match

match is not a functionality, it is an operator. You match your request with regular expression:

* API

```java
server.request(match(uri("/\\w*/foo"))).response(text("bar"));
```

* JSON

```json
{
  "request": 
    {
      "uri": 
        {
          "match": "/\\w*/foo"
        }
    },
  "response": 
    {
      "text": "bar"
    }
}
```

Moco is implemented by Java regular expression, you can refer [here](http://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html) for more details.

## Response

### Content

As you have seen in previous example, response with content is pretty easy.

* API

```java
server.request(by("foo")).response("bar");
```

* JSON

```json
{
  "request" :
    {
      "text" : "foo"
    },
  "response" :
    {
      "text" : "bar"
    }
}
```

The same as request, you can response with a file if content is too large to put in a string.

* API

```java
server.request(by("foo")).response(file("bar.response"));
```

* JSON

```json
{
  "request" :
    {
      "text" : "foo"
    },
  "response" :
    {
      "file" : "bar.response"
    }
}
```

### Status Code
Moco also supports HTTP status code response.

* API

```java
server.request(by("foo")).response(status(200));
```

* JSON

```json
{
  "request" :
    {
      "text" : "foo"
    },
  "response" :
    {
      "status" : 200
    }
}
```

### Version

By default, response HTTP version is supposed to request HTTP version, but you can set your own HTTP version:

* API

```java
server.response(version("HTTP/1.0"));
```

* JSON


```json
{
  "request": 
    {
      "uri": "/version10"
    },
  "response": 
    {
      "version": "HTTP/1.0"
    }
}
```


### Header
We can also specify HTTP header in response.

* API

```java
server.request(by("foo")).response(header("content-type", "application/json"));
```

* JSON

```json
{
  "request" :
    {
      "text" : "foo"
    },
  "response" :
    {
      "headers" :
        {
          "content-type" : "application/json"
        }
    }
}
```

### Url

We can also response with the specified url, just like a proxy.

* API

```java
server.request(by("foo")).response(proxy("http://www.github.com"));
```

* JSON

```json
{
  "request" :
    {
      "text" : "foo"
    },
  "response" :
    {
      "proxy" : "http://www.github.com"
    }
}
```

### Redirect

Redirect is a common case for normal web development. We can simply redirect a request to different url.

* API

```java
server.get(by(uri("/redirect"))).redirectTo("http://www.github.com");
```

* JSON

```json
{
  "request" :
    {
      "uri" : "/redirect"
    },
  "redirectTo" : "http://www.github.com"
}
```

### Cookie

Cookie can also be in the response.

* API

```java
server.response(cookie("loggedIn", "true"), status(302));
```

* JSON

```json
{
  "request" :
    {
      "uri" : "/cookie"
    },
  "response" :
    {
      "cookies" :
      {
        "login" : "true"
      }
    }
}
```


### Latency

Sometimes, we need a latency to simulate slow server side operation.

* API

```java
server.request(by("foo")).response(latency(5000));
```

* JSON

```json
{
  "request" :
    {
      "text" : "foo"
    },
  "response" :
    {
      "latency" : 5000
    }
}
```

### Sequence

Sometimes, we want to simulate a real-world operation which change server side resource. For example:
* First time you request a resource and "foo" is returned
* We update this resource
* Again request the same URL, updated content, e.g. "bar" is expected.

We can do that by
```java
server.request(by(uri("/foo"))).response(seq("foo", "bar", "blah"));
```

## Mount

Moco allows us to mount a directory to uri.

* API

```java
server.mount(dir, to("/uri"));
```

* JSON

```json
{
  "mount" :
    {
      "dir" : "dir",
      "uri" : "/uri"
    }
}
```

Wildcard is acceptable to filter specified files, e.g we can include by

* API

```java
server.mount(dir, to("/uri"), include("*.txt"));
```

* JSON

```json
{
  "mount" :
    {
      "dir" : "dir",
      "uri" : "/uri",
      "includes" :
        [
          "*.txt"
        ]
    }
}
```

or exclude by

* API

```java
server.mount(dir, to("/uri"), exclude("*.txt"));
```

* JSON

```json
{
  "mount" :
    {
      "dir" : "dir",
      "uri" : "/uri",
      "excludes" :
        [
          "*.txt"
        ]
    }
}
```

even compose them by

* API

```java
server.mount(dir, to("/uri"), include("a.txt"), exclude("b.txt"), include("c.txt"));
```

* JSON

```json
{
  "mount" :
    {
      "dir" : "dir",
      "uri" : "/uri",
      "includes" :
        [
          "a.txt",
          "b.txt"
        ],
      "excludes" :
        [
          "c.txt"
        ]
    }
}
```