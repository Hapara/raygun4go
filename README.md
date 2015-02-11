# raygun4go
[![Build Status](https://travis-ci.org/kaeuferportal/raygun4go.svg?branch=master)](https://travis-ci.org/kaeuferportal/raygun4go)
[![Coverage](http://gocover.io/_badge/github.com/kaeuferportal/raygun4go)](http://gocover.io/github.com/kaeuferportal/raygun4go)
[![GoDoc](https://godoc.org/github.com/kaeuferportal/raygun4go?status.svg)](http://godoc.org/github.com/kaeuferportal/raygun4go)

raygun4go adds Raygun-based error handling to your golang code. It catches all
occuring errors, extracts as much information as possible and sends the error
to Raygun via their REST-API.

## Getting Started

### Installation
```
  $ go get github.com/kaeuferportal/raygun4go
```

### Basic Usage

Include the package and then defer the HandleError-method as soon as possible
in a context as global as possible. In webservers, this will probably be your
request handling method, in all other programs it should be your main-method.
Having found the right spot, just add the following example code:

```
raygun, err := raygun4go.New("appName", "apiKey")
if err != nil {
  log.Println("Unable to create Raygun client:", err.Error())
}
raygun.Silent(true)
defer raygun.HandleError()
```

where ``appName`` is the name of your app and ``apiKey`` is your
Raygun-API-key. If your program runs into a panic now (which you can easily
test by adding ``panic("foo")`` after the call to ``defer``), the handler will
print the resulting error message. If you remove the line
```
raygun.Silent(true)
```
the error will be sent to Raygun using your API-key.

### Options

The client returned by ``New`` has several chainable option-setting methods

Method                    | Description
--------------------------|------------------------------------------------------------
`Silent(bool)`            | If set to true, this prevents the handler from sending the error to Raygun, printing it instead.
`Request(*http.Request)`  | Adds the responsible http.Request to the error.
`Version(string)`         | If your program has a version, you can add it here.
`Tags([]string)`          | Adds the given tags to the error. These can be used for filtering later.
`CustomData(interface{})` | Add arbitrary custom data to you error. Will only reach Raygun if it works with `json.Marshal()`.
`User(string)`            | Add the name of the affected user to the error.

## Bugs and feature requests

Have a bug or a feature request? Please first check the list of
[issues](https://github.com/kaeuferportal/raygun4go/issues).

If your problem or idea is not addressed yet, [please open a new
issue](https://github.com/kaeuferportal/raygun4go/issues/new), or contact us at
[oss@kaeuferportal.de](mailto:oss@kaeuferportal.de).