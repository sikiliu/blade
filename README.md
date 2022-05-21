<p align="center">
    <a href="https://lets-blade.com"><img src="https://i.loli.net/2018/09/18/5ba0cd93c710e.png" width="650"/></a>
</p>
<p align="center">Based on <code>Java8</code> + <code>Netty4</code> to create a lightweight, high-performance, simple and elegant Web framework 😋</p>
<p align="center">Spend <b>1 hour</b> to learn it to do something interesting, a tool in addition to the other available frameworks.</p>
<p align="center">
    🐾 <a href="#quick-start" target="_blank">Quick Start</a> |
    🌚 <a href="https://lets-blade.github.io/" target="_blank">Documentation</a> |
    :green_book: <a href="https://www.baeldung.com/blade" target="_blank">Guidebook</a> |
    💰 <a href="https://lets-blade.github.io/donate" target="_blank">Donate</a> |
    🇨🇳 <a href="README_CN.md">简体中文</a>
</p>
<p align="center">
    <a href="https://travis-ci.org/lets-blade/blade"><img src="https://img.shields.io/travis/lets-blade/blade.svg?style=flat-square"></a>
    <a href="http://search.maven.org/#search%7Cga%7C1%7Cblade-core"><img src="https://img.shields.io/maven-central/v/com.hellokaton/blade-core.svg?style=flat-square"></a>
    <a href="LICENSE"><img src="https://img.shields.io/badge/license-Apache%202-4EB1BA.svg?style=flat-square"></a>
    <a class="badge-align" href="https://www.codacy.com/gh/lets-blade/blade/dashboard"><img src="https://app.codacy.com/project/badge/Grade/1eff0e30bf694402ac1b0ebe587bfa5a"/></a>
    <a href="https://gitter.im/lets-blade/blade"><img src="https://badges.gitter.im/hellokaton/blade.svg?style=flat-square"></a>
    <a href="https://www.codetriage.com/lets-blade/blade"><img src="https://www.codetriage.com/lets-blade/blade/badges/users.svg"></a>
</p>

***

## What Is Blade?

`Blade` is a pursuit of simple, efficient Web framework, so that `JavaWeb` development becomes even more powerful, both in performance and flexibility.
If you like to try something interesting, I believe you will love it.
If you think it's good, you can support it with a [star](https://github.com/lets-blade/blade/stargazers) or by [donating](https://ko-fi.com/hellokaton) :blush:

## Features

* [x] A new generation MVC framework that doesn't depend on other libraries
* [x] Get rid of SSH's bloated, modular design
* [x] Source is less than `500kb`, learning it is also simple
* [x] RESTful-style routing design
* [x] Template engine support, view development more flexible
* [x] High performance, 100 concurrent qps 20w/s
* [x] Run the `JAR` package to open the web service
* [x] Streams-style API
* [x] `CSRF` and `XSS` defense
* [x] `Basic Auth` and `Authorization`
* [x] Supports plug-in extensions
* [x] Support webjars resources
* [x] Tasks based on `cron` expressions
* [x] Built-in a variety of commonly used middleware
* [x] Built-in Response output
* [x] JDK8 +

## Overview

» Simplicity: The design is simple, easy to understand and doesn't introduce many layers between you and the standard library. The goal of this project is that the users should be able to understand the whole framework in a single day.<br/>
» Elegance: `blade` supports the RESTful style routing interface, has no invasive interceptors and provides the writing of a DSL grammar.<br/>
» Easy deploy: supports `maven` package `jar` file running.<br/>

## Quick Start

Create a basic `Maven` or `Gradle` project.

> Do not create a `webapp` project, Blade does not require much trouble.

Run with `Maven`:

```xml
<dependency>
    <groupId>com.hellokaton</groupId>
    <artifactId>blade-core</artifactId>
    <version>2.1.2.RELEASE</version>
</dependency>
```

or `Gradle`:

```sh
compile 'com.hellokaton:blade-core:2.1.2.RELEASE'
```

Write the `main` method and the `Hello World`:

```java
public static void main(String[] args) {
    Blade.create().get("/", ctx -> ctx.text("Hello Blade")).start();
}
```

Open http://localhost:9000 in your browser to see your first `Blade` application!

## Contents

- [**`Register Route`**](#register-route)
    - [**`HardCode`**](#hardCode)
    - [**`Controller`**](#controller)
- [**`Request Parameter`**](#request-parameter)
    - [**`URL Parameter`**](#URL-parameter)
    - [**`Form Parameter`**](#form-parameter)
    - [**`Path Parameter`**](#path-parameter)
    - [**`Body Parameter`**](#body-parameter)
    - [**`Parse To Model`**](#parse-to-model)
- [**`Get Environment`**](#get-environment)
- [**`Get Header`**](#get-header)
- [**`Get Cookie`**](#get-cookie)
- [**`Static Resource`**](#static-resource)
- [**`Upload File`**](#upload-file)
- [**`Download File`**](#download-file)
- [**`Set Session`**](#set-session)
- [**`Render To Browser`**](#render-to-browser)
    - [**`Render Response`**](#render-json)
    - [**`Render Text`**](#render-text)
    - [**`Render Html`**](#render-html)
- [**`Render Template`**](#render-template)
    - [**`Default Template`**](#default-template)
    - [**`Jetbrick Template`**](#jetbrick-template)
- [**`Redirects`**](#redirects)
- [**`Write Cookie`**](#write-cookie)
- [**`Web Hook`**](#web-hook)
- [**`Logging`**](#logging)
- [**`Basic Auth`**](#basic-auth)
- [**`Change Server Port`**](#change-server-port)
- [**`Configuration SSL`**](#configuration-ssl)
- [**`Custom Exception Handler`**](#custom-exception-handler)

## Register Route

### HardCode

```java
public static void main(String[] args) {
    // Create multiple routes GET, POST, PUT, DELETE using Blade instance
    Blade.create()
        .get("/user/21", getting)
        .post("/save", posting)
        .delete("/remove", deleting)
        .put("/putValue", putting)
        .start();
}
```

### `Controller`

```java
@Path
public class IndexController {

    @GET("/login")
    public String login(){
        return "login.html";
    }
    
    @POST(value = "/login", responseType = ResponseType.JSON)
    public RestResponse doLogin(RouteContext ctx){
        // do something
        return RestResponse.ok();
    }

}
```

## Request Parameter

### URL Parameter

**Using RouteContext**

```java
public static void main(String[] args) {
    Blade.create().get("/user", ctx -> {
        Integer age = ctx.queryInt("age");
        System.out.println("age is:" + age);
    }).start();
}
```

**Using `@Query` annotation**

```java
@GET("/user")
public void savePerson(@Query Integer age){
  System.out.println("age is:" + age);
}
```

Test it with sample data from the terminal

```bash
curl -X GET http://127.0.0.1:9000/user?age=25
```

### Form Parameter

Here is an example:

**Using RouteContext**

```java
public static void main(String[] args) {
    Blade.create().get("/user", ctx -> {
        Integer age = ctx.fromInt("age");
        System.out.println("age is:" + age);
    }).start();
}
```

**Using `@Form` Annotation**

```java
@POST("/save")
public void savePerson(@Form String username, @Form Integer age){
  System.out.println("username is:" + username + ", age is:" + age);
}
```

Test it with sample data from the terminal

```bash
curl -X POST http://127.0.0.1:9000/save -F username=jack -F age=16
```

### Path Parameter

**Using RouteContext**

```java
public static void main(String[] args) {
    Blade blade = Blade.create();
    // Create a route: /user/:uid
    blade.get("/user/:uid", ctx -> {
        Integer uid = ctx.pathInt("uid");
        ctx.text("uid : " + uid);
    });

    // Create two parameters route
    blade.get("/users/:uid/post/:pid", ctx -> {
        Integer uid = ctx.pathInt("uid");
        Integer pid = ctx.pathInt("pid");
        String msg = "uid = " + uid + ", pid = " + pid;
        ctx.text(msg);
    });
    
    // Start blade
    blade.start();
}
```

**Using `@PathParam` Annotation**

```java
@GET("/users/:username/:page")
public void userTopics(@PathParam String username, @PathParam Integer page){
    System.out.println("username is:" + usernam + ", page is:" + page);
}
```

Test it with sample data from the terminal

```bash
curl -X GET http://127.0.0.1:9000/users/hellokaton/2
```

### Body Parameter

```java
public static void main(String[] args) {
    Blade.create().post("/body", ctx -> {
        System.out.println("body string is:" + ctx.bodyToString());
    }).start();
}
```

**Using `@Body` Annotation**

```java
@POST("/body")
public void readBody(@Body String data){
    System.out.println("data is:" + data);
}
```

Test it with sample data from the terminal

```bash
curl -X POST http://127.0.0.1:9000/body -d '{"username":"hellokaton","age":22}'
```

### Parse To Model

This is the `User` model.

```java
public class User {
    private String username;
    private Integer age;
    // getter and setter
}
```

**By Annotation**

```java
@POST("/users")
public void saveUser(@Form User user) {
    System.out.println("user => " + user);
}
```

Test it with sample data from the terminal

```bash
curl -X POST http://127.0.0.1:9000/users -F username=jack -F age=16
```

**Custom model identification**

```java
@POST("/users")
public void saveUser(@Form(name="u") User user) {
    System.out.println("user => " + user);
}
```

Test it with sample data from the terminal

```bash
curl -X POST http://127.0.0.1:9000/users -F u[username]=jack -F u[age]=16
```

**Body Parameter To Model**

```java
@POST("/body")
public void body(@Body User user) {
    System.out.println("user => " + user);
}
```

Test it with sample data from the terminal

```bash
curl -X POST http://127.0.0.1:9000/body -d '{"username":"hellokaton","age":22}'
```

## Get Environment

```java
Environment environment = WebContext.blade().environment();
String version = environment.get("app.version", "0.0.1");
```

## Get Header

**By Context**

```java
@GET("header")
public void readHeader(RouteContext ctx){
    System.out.println("Host => " + ctx.header("Host"));
    // get useragent
    System.out.println("UserAgent => " + ctx.userAgent());
    // get client ip
    System.out.println("Client Address => " + ctx.address());
}
```

**By Annotation**

```java
@GET("header")
public void readHeader(@Header String host){
    System.out.println("Host => " + host);
}
```

## Get Cookie

**By Context**

```java
@GET("cookie")
public void readCookie(RouteContext ctx){
    System.out.println("UID => " + ctx.cookie("UID"));
}
```

**By Annotation**

```java
@GET("cookie")
public void readCookie(@Cookie String uid){
    System.out.println("Cookie UID => " + uid);
}
```

## Static Resource

Blade builds a few static resource catalog, as long as you will save the resource file in the static directory under the classpath, and then browse http://127.0.0.1:9000/static/style.css

If you want to customize the static resource URL

```java
Blade.create().addStatics("/mydir");
```

Of course you can also specify it in the configuration file. `application.properties` (location in classpath)

```bash
mvc.statics=/mydir
```

## Upload File

**By Request**

```java
@POST("upload")
public void upload(Request request){
    request.fileItem("img").ifPresent(fileItem -> {
        fileItem.moveTo(new File(fileItem.getFileName()));
    });
}
```

**By Annotation**

```java
@POST("upload")
public void upload(@Multipart FileItem fileItem){
    // Save to new path
    fileItem.moveTo(new File(fileItem.getFileName()));
}
```

## Download File

```java
@GET(value = "/download", responseType = ResponseType.STREAM)
public void download(Response response) throws IOException {
    response.write("abcd.pdf", new File("146373013842336153820220427172437.pdf"));
}
```

**If you want to preview certain files in your browser**

```java
@GET(value = "/preview", responseType = ResponseType.PREVIEW)
public void preview(Response response) throws IOException {
    response.write(new File("146373013842336153820220427172437.pdf"));
}
```

## Set Session

The session is disabled by default, you must enable the session.

```java
Blade.create()
     .http(HttpOptions::enableSession)
     .start(Application.class, args);
```

> 💡 It can also be enabled using a configuration file，`http.session.enabled=true` 

```java
public void login(Session session){
    // if login success
    session.attribute("login_key", SOME_MODEL);
}
```

## Render To Browser

### Render Response

**By Context**

```java
@GET("users/json")
public void printJSON(RouteContext ctx){
    User user = new User("hellokaton", 18);
    ctx.json(user);
}
```

**By Annotation**

This form looks more concise 😶

```java
@GET(value = "/users/json", responseType = ResponseType.JSON)
public User printJSON(){
    return new User("hellokaton", 18);
}
```

### Render Text

```java
@GET("text")
public void printText(RouteContext ctx){
    ctx.text("I Love Blade!");
}
```

or

```java
@GET(value = "/text", responseType = ResponseType.TEXT)
public String printText(RouteContext ctx){
    return "I Love Blade!";
}
```

### Render Html

```java
@GET("html")
public void printHtml(RouteContext ctx){
    ctx.html("<center><h1>I Love Blade!</h1></center>");
}
```

or

```java
@GET(value = "/html", responseType = ResponseType.HTML)
public String printHtml(RouteContext ctx){
    return "<center><h1>I Love Blade!</h1></center>";
}
```

## Render Template

By default all template files are in the templates directory; in most of the cases you do not need to change it.

### Default Template

By default, Blade uses the built-in template engine, which is very simple. In a real-world web project, you can try several other extensions.

```java
public static void main(String[] args) {
    Blade.create().get("/hello", ctx -> {
        ctx.attribute("name", "hellokaton");
        ctx.render("hello.html");
    }).start(Hello.class, args);
}
```

The `hello.html` template

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Hello Page</title>
</head>
<body>

    <h1>Hello, ${name}</h1>

</body>
</html>
```

### Jetbrick Template

**Config Jetbrick Template**

Create a `BladeLoader` class and load some config

```java
@Bean
public class TemplateConfig implements BladeLoader {

    @Override
    public void load(Blade blade) {
        blade.templateEngine(new JetbrickTemplateEngine());
    }

}
```

Write some data for the template engine to render

```java
public static void main(String[] args) {
    Blade.create().get("/hello", ctx -> {
        User user = new User("hellokaton", 50);
        ctx.attribute("user", user);
        ctx.render("hello.html");
    }).start(Hello.class, args);
}
```

The `hello.html` template

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Hello Page</title>
</head>
<body>

    <h1>Hello, ${user.username}</h1>

    #if(user.age > 18)
        <p>Good Boy!</p>
    #else
        <p>Gooood Baby!</p>
    #end

</body>
</html>
```

[Render API](http://static.javadoc.io/com.hellokaton/blade-core/2.1.2.RELEASE/com/hellokaton/blade/mvc/http/Response.html#render-com.ModelAndView-)

## Redirects

```java
@GET("redirect")
public void redirectToGithub(RouteContext ctx){
    ctx.redirect("https://github.com/hellokaton");
}
```

[Redirect API](http://static.javadoc.io/com.hellokaton/blade-core/2.1.2.RELEASE/com/hellokaton/blade/mvc/http/Response.html#redirect-java.lang.String-)

## Write Cookie

```java
@GET("write-cookie")
public void writeCookie(RouteContext ctx){
    ctx.cookie("hello", "world");
    ctx.cookie("UID", "22", 3600);
}
```

[Cookie API](http://static.javadoc.io/com.hellokaton/blade-core/2.1.2.RELEASE/com/hellokaton/blade/mvc/http/Response.html#cookie-java.lang.String-java.lang.String-)

## Web Hook

`WebHook` is the interface in the Blade framework that can be intercepted before and after the execution of the route.

```java
public static void main(String[] args) {
    // All requests are exported before execution before
    Blade.create().before("/*", ctx -> {
        System.out.println("before...");
    }).start();
}
```

## Logging

Blade uses slf4j-api as logging interface, the default implementation of a simple log package (modified from simple-logger); if you need complex logging you can also use a custom library, you only need to exclude the `blade-log` from the dependencies.

```java
private static final Logger log = LoggerFactory.getLogger(Hello.class);

public static void main(String[] args) {
    log.info("Hello Info, {}", "2017");
    log.warn("Hello Warn");
    log.debug("Hello Debug");
    log.error("Hello Error");
}
```

## Basic Auth

Blade includes a few middleware, like Basic Authentication; of course, it can also be customized to achieve more complex goals.

```java
public static void main(String[] args) {
    Blade.create().use(new BasicAuthMiddleware()).start();
}
```

Specify the user name and password in the `application.properties` configuration file.

```bash
http.auth.username=admin
http.auth.password=123456
```

## Change Server Port

There are three ways to modify the port: hard coding it, in a configuration file, and through a command line parameter.

**Hard Coding**

```java
Blade.create().listen(9001).start();
```

**Configuration For `application.properties`**

```bash
server.port=9001
```

**Command Line**

```bash
java -jar blade-app.jar --server.port=9001
```

## Configuration SSL

**Configuration For `application.properties`**

```bash
server.ssl.enable=true
server.ssl.cert-path=cert.pem
server.ssl.private-key-path=private_key.pem
server.ssl.private-key-pass=123456
```

## Custom Exception Handler

Blade has an exception handler already implemented by default; if you need to deal with custom exceptions, you can do it like follows.

```java
@Bean
public class GlobalExceptionHandler extends DefaultExceptionHandler {
    
    @Override
    public void handle(Exception e) {
        if (e instanceof CustomException) {
            CustomException customException = (CustomException) e;
            String code = customException.getCode();
            // do something
        } else {
            super.handle(e);
        }
    }

}
```

Besides looking easy, the features above are only the tip of the iceberg, and there are more surprises to see in the documentation and sample projects:

+ [Blade Demos](https://github.com/lets-blade/blade-demos)
+ [Awesome Blade](https://github.com/lets-blade/awesome-blade)

## Change Logs

[See Here](https://lets-blade.com/about/change-logs)

## Contact

- Twitter: [hellokaton](https://twitter.com/hellokaton)
- Mail: hellokaton@gmail.com

## Contributors

Thanks goes to these wonderful people

![contributors.svg](https://opencollective.com/blade/contributors.svg?width=890&button=false)

Contributions of any kind are welcome!

## Licenses

Please see [Apache License](LICENSE)
test 647
