# Notes of Go

Hi there! I'm [nono.ma](http://nono.ma). On Jan 30, 2017, I decided to dive into Go. These notes were extremely helpful for me to start going, and include recommended reads, videos, and resources I found along the way. I hope you find them useful.

## Contents

* [Reading](#reading)
* [Videos](#videos)
* [Packages](#packages)
* [Tools](#tools)
* [Resources](#resources)
* [Syntax](#syntax)
* [Examples](#examples-with-library-snippets)

<!-- 
* [Examples with Library Snippets](#examples-with-library-snippets)
* [Examples with play.golang.org](#examples-with-playgolangorg)
* [Examples with Go By Example](#examples-with-go-by-example)
 -->
 
## Reading

* [Embrace Go – A modern programming language](https://developer.washingtonpost.com/pb/blog/post/2016/04/06/embrace-go/) by Samaresh Panda on The Washington Post.
* [Four Years of Go](https://blog.golang.org/4years).

## Videos

* [Go: a simple programming environment](https://vimeo.com/69237265) by Andrew Gerrand at Railsberry 2013. Useful, well-explained examples.
* [Writing, building, installing, and testing Go code](https://www.youtube.com/watch?v=XCsL89YtqCs).

## Packages

* [blackfriday](http://github.com/russross/blackfriday) - markdown parser.
* [expvar](https://golang.org/pkg/expvar/) - "a standardized interface to public variables, such as operation counters in servers. It exposes these variables via HTTP at /debug/vars in JSON format."
* [gin](https://github.com/gin-gonic/gin) - HTTP web framework. Martini-like API (with better performance; 40x).
* [gjson](https://github.com/tidwall/gjson) - “Get JSON values very quickly in Go.” Allows to access JSON values directly from a string, byte slice, obtain multiple values at once, etc. by just specifying a path with dot notation (e.g. `name.last`).
* [godotenv](http://github.com/joho/godotenv) - load environment variables from the `.env` file.
* [go-hashids](https://github.com/speps/go-hashids) - a Go implementation of [www.hashids.org](http://www.hashids.org).
* [kv](https://github.com/cznic/kv) - simple and easy to use persistent key/value (KV) store.
* [logrus](https://github.com/Sirupsen/logrus) - "Structured, pluggable logging for Go."
* [mgo](http://gopkg.in/mgo.v2) - “Rich MongoDB driver for Go.” ([Sample code](https://gist.github.com/border/3489566).)
* [ql](https://github.com/cznic/ql) - pure Go embedded SQL database.
* [redigo](https://github.com/garyburd/redigo/) - Go client for Redis. ("Redis is an open-source, in-memory data structure store, used as a database, cache and message broker.") ([Redis Quick Start Quide](https://redis.io/topics/quickstart).)
* [sky](https://github.com/Shopify/sky) - an open source, behavioral analytics database.

***

* https://github.com/zmb3/spotify
* https://github.com/everdev/mack

https://github.com/gorilla/schema Package gorilla/schema fills a struct with form values.

http://www.gorillatoolkit.org/pkg/feeds Syndication (feed) generator library

## Tools

* [JSON-to-Go](https://mholt.github.io/json-to-go/) - Convert JSON to Go instantly.

## Resources

* [Create A Real Time Chat App With Golang, Angular 2, And Websockets](https://www.thepolyglotdeveloper.com/2016/12/create-real-time-chat-app-golang-angular-2-websockets/).
* [Go Websockets (Gin-gonic + Gorilla)](http://arlimus.github.io/articles/gin.and.gorilla/).
* [Parse Complicated JSON with Go Unmarshal?](http://stackoverflow.com/questions/30341588/how-to-parse-a-complicated-json-with-go-lang-unmarshal) at StackOverflow.
* [Go Podcasts](https://github.com/golang/go/wiki/Podcasts)

## Syntax

### String to byte[]

```go
byteArray := []byte(aString)
```

### Formatting A String with Numbers

```go
var number = 30
var phrase = "My number is"
var stringA = phrase + " " + strconv.Itoa(number) + "." // My number is 30.
var stringB = fmt.Sprintf("%s %v.", phrase, number) // My number is 30.
```

### Structs

Go structs resemble the structure of JSON objects. For instance, we can define a `person` struct. Because the name of the struct is lowercase, it won’t be exported and shared between `.go` files of your application.

```go
type person struct {
	Name	string
	Age		int
}
```

We can share a struct between files of an application by capitalizing its name, and we can also specify the name each field should have when parsed into JSON.

```go
type Sphere struct {
	Origin	float64	`json:"origin"`
	Radius	float64	`json:"radius"`
}
```

## Examples with Library Snippets

### gin

The following are examples from the official repository. Some of them are modified and others are exactly like they were.

```go
package main

import "github.com/gin-gonic/gin"

func main() {
    r := gin.Default()
    r.GET("/ping", func(c *gin.Context) {
        c.JSON(200, gin.H{
            "message": "pong",
        })
    })
    r.Run() // listen and serve on 0.0.0.0:8080
}
```

#### Serving Static Files

```go
func main() {
    router := gin.Default()
    router.Static("/css", "./public/css")
    router.StaticFS("/more_static", http.Dir("my_file_system"))
    router.StaticFile("/favicon.ico", "./resources/favicon.ico")
    router.Run()
}
```

### gjson

```go
  json := `{"name":{"first":"Nono","last":"Martinez"},"age":26,"web":"nono.ma"}`
  firstname := gjson.Get(json, "name.first")
  web := gjson.Get(json, "web")
  println(firstname.String(),web.String())
```

### encoding/json

A problem I’ve been having lately is to receive relatively *flexible* JSON data from a client and convert it — unmarshal it — into a pure Go struct.

Let’s say our JSON looks something like this:

```json
{
	"action": string,
	"data":{
		"x": int,
		"y": int
	}
}
```

So a sample JSON object of a received message might look like:

```json
{
	"action":"click",
	"data":{
		"x":75,
		"y":40
	}
}
```

In our Go program, we want to work with as many native structs as we can, so we have the struct `Message`.

```go
type Message struct {
	Action	string		`json:"action"
	Data	interface{}		`json:"data"	
}
```

Here, we *have* to leave our `Data` property with an `interface{}` type, which leaves it open for different types of data.

In the previous example, the data sent matches with the following `Point` struct.

```go
type Point struct {
	X	Int	`json:"x"`
	Y	Int	`json:"y"`
}
```

Then comes the magic. We first get the string of JSON that contains our `data` and then convert it to bytes to call `json.Unmarshal` on it.

```go
data := gjson.GetBytes(message, "data")

var point Point
err := json.Unmarshal([]byte(data.String()), &point)
if err != nil {
println("error: could not unmarshal")
} else {
fmt.Println("point",point)
fmt.Println("point.X",point.X)
fmt.Println("point.Y",point.Y)
}
```

### godotenv

Install with `go get github.com/joho/godotenv`. Your `.env` file might look like this:

```
PORT=8080
REDIS_URL=redis://localhost:6379
```

Let’s try loading the content of the `.env` file as environment variables. (Usually, this is done at the beginning of your `main` function.)

```go
	err := godotenv.Load()
  if err != nil {
    log.Fatal("Error loading .env file")
  }
```

All of your `.env` variables will be available as every other environment variable:

```go
	port := os.Getenv("PORT")
	if port == "" {
		log.WithField("PORT", port).Fatal("$PORT must be set")
	}
```

## govendor

Manage your project dependencies locally with `govendor`.

Add the following to your project’s `.gitignore` to ensure that dependencies are ignored but your `vendor.json` is still pushed to your repository. If you clone your repository, a simple `go vendor sync` will do to fetch back all dependencies.

```
vendor/*/
```

## Examples on [play.golang.org](http://play.golang.org), [Go By Example](http://gobyexample.com), and more

* play.golang.org - Blocking Channels with [string](https://play.golang.org/p/hGCtCoxZhO) or [int](https://play.golang.org/p/wfjgXvphSg).
* play.golang.org - [Unmarshal JSON which structure isn't defined by a Go struct](https://play.golang.org/p/gfP6P4SmaC)
* Go By Example - [Closing Channels](https://gobyexample.com/closing-channels)
- Go Blueprints - [Chat Example](https://github.com/matryer/goblueprints/tree/master/chapter3/chat) (Chapter 3)

## Me

I'm [Nono Martínez Alonso](http://nono.ma) ([nono.ma](http://nono.ma)), a computational designer with a penchant for design, code, and simplicity.  
I tweet at [@nonoesp](http://www.twitter.com/nonoesp) and write at [Getting Simple](http://gettingsimple.com/). If you find this useful, I would love to hear about it. Thanks!