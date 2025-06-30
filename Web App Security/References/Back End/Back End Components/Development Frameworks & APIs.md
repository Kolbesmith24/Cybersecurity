# Development Frameworks & APIs
There are many common web development frameworks that help in developing core web application files and functionality.

Some of the most common web development frameworks include:

- [Laravel](https://laravel.com/) (`PHP`): usually used by startups and smaller companies, as it is powerful yet easy to develop for.
- [Express](https://expressjs.com/) (`Node.JS`): used by `PayPal`, `Yahoo`, `Uber`, `IBM`, and `MySpace`.
- [Django](https://www.djangoproject.com/) (`Python`): used by `Google`, `YouTube`, `Instagram`, `Mozilla`, and `Pinterest`.
- [Rails](https://rubyonrails.org) (`Ruby`): used by `GitHub`, `Hulu`, `Twitch`, `Airbnb`, and even `Twitter` in the past.

`It must be noted that popular websites usually utilize a variety of frameworks and web servers, rather than just one.`
## APIs
Web [APIs](https://en.wikipedia.org/wiki/API) and HTTP Request parameters connect the front end and the back end to be able to send data back and forth between front end and back end components and carry out various functions within the web application.

For the front end component to interact with the back end and ask for certain tasks to be carried out, they utilize APIs to ask the back end component for a specific task with specific input. The back end components process these requests, perform the necessary functions, and return a certain response to the front end components, which finally renderers the end user's output on the client-side.
### Query Parameters
The default method of sending specific arguments to a web page is through `GET` and `POST` request parameters.
## Web APIs
An API ([Application Programming Interface](https://en.wikipedia.org/wiki/API)) is an interface within an application that specifies how the application can interact with other applications. For Web Applications, it is what allows remote access to functionality on back end components. APIs are not exclusive to web applications and are used for software applications in general. Web APIs are usually accessed over the `HTTP` protocol and are usually handled and translated through web servers.

![API diagram with connections to cloud, user, shopping, video, audio, and key icons](https://academy.hackthebox.com/storage/modules/75/api_examples.jpg)

A weather web application, for example, may have a certain API to retrieve the current weather for a certain city. We can request the API URL and pass the city name or city id, and it would return the current weather in a `JSON` object.
## SOAP
The `SOAP` ([Simple Objects Access](https://en.wikipedia.org/wiki/SOAP)) standard shares data through `XML`, where the request is made in `XML` through an HTTP request, and the response is also returned in `XML`. Front end components are designed to parse this `XML` output properly.

`SOAP` is very useful for transferring structured data (i.e., an entire class object), or even binary data, and is often used with serialized objects, all of which enables sharing complex data between front end and back end components and parsing it properly. It is also very useful for sharing _stateful_ objects -i.e., sharing/changing the current state of a web page-, which is becoming more common with modern web applications and mobile applications.
## REST
The `REST` ([Representational State Transfer](https://en.wikipedia.org/wiki/Representational_state_transfer)) standard shares data through the URL path 'i.e. `search/users/1`', and usually returns the output in `JSON` format 'i.e. userid `1`'.

Unlike Query Parameters, `REST` APIs usually focus on pages that expect one type of input passed directly through the URL path, without specifying its name or type. 

This is usually useful for queries like `search`, `sort`, or `filter`. This is why `REST` APIs usually break web application functionality into smaller APIs and utilize these smaller API requests to allow the web application to perform more advanced actions.

Responses to `REST` API requests are usually made in `JSON` format, and the front end components are then developed to handle this response and render it properly. Other output formats for `REST` include `XML`, `x-www-form-urlencoded`, or even raw data.