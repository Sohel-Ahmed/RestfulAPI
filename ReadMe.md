No REST for the Wicked
In fact, this client/server relationship is a prerequisite of a set of principles called REST (or Representational State Transfer). This sounds kind of scary, but it's super easy—let's walk through it together.

Remember how we said HTTP involves sending hypertext (text with links)? Whenever you navigate through a site by clicking links, you're making a state transition, which brings you to the next page (representing the next state of the application). That's it!

By following this simple model of clicking from page to page, you're starting to follow REST principles. When something follows REST principles, we say that thing is RESTful. We'll cover these principles in the next exercise.


A RESTful API

An API, or application programming interface, is kind of like a coding contract: it specifies the ways a program can interact with an application. For example, if you want to write a program that reads and analyzes data from Twitter, you'd need to use the Twitter API, which would specify the process for authentication, important URLs, classes, methods, and so on.

For an API or web service to be RESTful, it must do the following:

Separate the client from the server
Not hold state between requests (meaning that all the information necessary to respond to a request is available in each individual request; no data, or state, is held by the server from request to request)
Use HTTP and HTTP methods (as explained in the next section).
There are some other requirements, but they're beyond the scope of this lesson.

The Four Verbs
The number of HTTP methods you'll use is quite small—there are just four HTTP "verbs" you'll need to know! They are:

GET: retrieves information from the specified source.
POST: sends new information to the specified source.
PUT: updates existing information of the specified source.
DELETE: removes existing information from the specified source.
So when we sent our GET request to placekitten.com, we retrieved information. When you add to or update your blog, you're sending POST or PUT requests; when you delete a tweet, there goes a DELETE request.

Anatomy of a Request
An HTTP request is made up of three parts:

The request line, which tells the server what kind of request is being sent (GET, POST, etc.) and what resource it's looking for;
The header, which sends the server additional information (such as which client is making the request)
The body, which can be empty (as in a GET request) or contain data (if you're POSTing or PUTing information, that information is contained here).

Check out the text in the editor. This is what a typical request looks like! Note the POST information in the request line, the header information below it, and the data to be POSTed at the bottom (line 5).
# POST /codecademy/learn-http HTTP/1.1
# Host: www.codecademy.com
# Content-Type: text/html; charset=UTF-8

# Name=Eric&Age=26

Endpoints
Endpoints are API-defined locations where particular data are stored. Just as you'll GET a pair of pants from PantsWorld or you'll GET a bag of peanuts from PeanutHut, you'll GET something different depending on the endpoint you use.

For instance, if you're using the API for a video hosting service, there might be endpoints for the most popular videos, the most recent videos, or videos belonging to a certain genre or category.

# What are the four commonly used HTTP methods ("verbs")?

# A: FIND, SEND, UPDATE, REMOVE
# B: CREATE, READ, UPDATE, DELETE
# C: GET, SEND, PUT, DELETE
# D: GET, POST, PUT, DELETE

answer = 'D'

Authentication & API Keys
Many APIs require an API key. Just as a real-world key allows you to access something, an API key grants you access to a particular API. Moreover, an API key identifies you to the API, which helps the API provider keep track of how their service is used and prevent unauthorized or malicious activity.

Some APIs require authentication using a protocol called OAuth. We won't get into the details, but if you've ever been redirected to a page asking for permission to link an application with your account, you've probably used OAuth.

API keys are often long alphanumeric strings. We've made one up in the editor to the right! (It won't actually work on anything, but when you receive your own API keys in future projects, they'll look a lot like this.)

api_key = "FtHwuH8w1RDjQpOr0y0gF3AWm8sRsRzncK3hHh9"


HTTP Status Codes
A successful request to the server results in a response, which is the message the server sends back to you, the client.

The response from the server will contain a three-digit status code. These codes can start with a 1, 2, 3, 4, or 5, and each set of codes means something different. (You can read the full list here). They work like this:

1xx: You won't see these a lot. The server is saying, "Got it! I'm working on your request."

2xx: These mean "okay!" The server sends these when it's successfully responding to your request.

3xx: These mean "I can do what you want, but I have to do something else first." You might see this if a website has changed addresses and you're using the old one; the server might have to reroute the request before it can get you the resource you asked for.

4xx: These mean you probably made a mistake. The most famous is "404," meaning "file not found": you asked for a resource or web page that doesn't exist.

5xx: These mean the server goofed up and can't successfully respond to your request.


Anatomy of a Response
The HTTP response structure mirrors that of the HTTP request. It contains:

A response line, which includes the three-digit HTTP status code;

A header, which includes further information about the server and its response;

The body, which contains the text of the response.

Instructions
Check out the code in the editor. See how its structure resembles the request layout? Click Save & Submit Code when you're ready to continue.

# HTTP/1.1 200 OK
# Content-Type: text/xml; charset=UTF-8

# <?xml version="1.0" encoding="utf-8"?>
# <string xmlns="https://www.codecademy.com/">Accepted</string>

Parsing XML
XML (which stands for E xtensible Markup Language) is very similar to HTML—it uses tags between angle brackets. The difference is that XML allows you to use tags that you make up, rather than tags that the W3C decided on. For instance, you could create an API that returns information about a pet:

<pet>
  <name>Jeffrey</name>
  <species>Giraffe</species>
</pet>
As long as you document the structure of your API's response, other people can use your API to get information about <pet>s.

Instructions
Check out the code in the editor. We've required in the 'rexml/document' module for parsing XML, and we're just using File.open to read from pets.txt (under the "Files") tab. Reading from this file is just like reading XML sent by a server.

require "rexml/document"

file = File.open("pets.txt")
doc = REXML::Document.new file
file.close

doc.elements.each("pets/pet/name") do |element|
  puts element
end

pets.txt
<pets>
  <pet>
    <name>Jeffrey</name>
    <species>Giraffe</species>
  </pet>
  <pet>
    <name>Gustav</name>
    <species>Dog</species>
  </pet>
  <pet>
    <name>Gregory</name>
    <species>Duck</species>
  </pet>
</pets>

Parsing JSON
JSON (which stands for JavaScript Object Notation) is an alternative to XML. It gets its name from the fact that its data format resembles JavaScript objects, and it is often more succinct than the equivalent XML. For instance, our same <pet> Jeffrey would look like this in JSON:

{
  "pets": {
    "name": "Jeffrey",
    "species": "Giraffe"
  }
}
Look, ma! No tags!

Instructions
Here we've required the 'json' module for parsing JSON, and we're using File.open as before to read from pets.txt (under the "Files") tab. Reading from this file is just like reading JSON sent by a server.

script.rb

require 'json'

pets = File.open("pets.txt", "r")

doc = ""
pets.each do |line|
  doc << line
end
pets.close

puts JSON.parse(doc)

pets.txt
{"pets":{"name":"Jeffrey","species":"Giraffe"}}


XML or JSON?
This leads us to wonder, though: how do we know whether an API will reply with XML or JSON?

The only way you'll know what type of data an API will send you is to read that API's documentation! Some will reply with one, and some will reply with the other. Documentation is a programmer's best friend, and it's always in your best interest to read it so you understand that what the API expects from you and what the API intends to send you when you make a request.

