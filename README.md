# Notes of Go

Hi there! I'm [nono.ma](http://nono.ma). On Jan 30, 2017, I decided to dive into Go. These notes were extremely helpful for me to start going, and include recommended reads, videos, and resources I found along the way. I hope you find them useful.

## Reading

* [Embrace Go – A modern programming language](https://developer.washingtonpost.com/pb/blog/post/2016/04/06/embrace-go/) by Samaresh Panda on The Washington Post
* [Four Years of Go](https://blog.golang.org/4years)

## Videos

* [Go: a simple programming environment](https://vimeo.com/69237265) by Andrew Gerrand at Railsberry 2013. Useful, well-explained examples.
* [Writing, building, installing, and testing Go code](https://www.youtube.com/watch?v=XCsL89YtqCs)

## Packages

* [expvar](https://golang.org/pkg/expvar/) - "a standardized interface to public variables, such as operation counters in servers. It exposes these variables via HTTP at /debug/vars in JSON format."
* [kv](https://github.com/cznic/kv) - simple and easy to use persistent key/value (KV) store.
* [ql](https://github.com/cznic/ql) - pure Go embedded SQL database.
* [sky](https://github.com/Shopify/sky) - an open source, behavioral analytics database.

## Tools

* [JSON-to-Go](https://mholt.github.io/json-to-go/) - Convert JSON to Go instantly.

## Go Lang

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

## Me

I'm [Nono Martínez Alonso](http://nono.ma) ([nono.ma](http://nono.ma)), a computational designer with a penchant for design, code, and simplicity.  
I tweet at [@nonoesp](http://www.twitter.com/nonoesp) and write at [Getting Simple](http://gettingsimple.com/). If you find this useful, I would love to hear about it. Thanks!
