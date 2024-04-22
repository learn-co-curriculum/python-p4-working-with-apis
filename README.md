# Working with APIs

NOTE: The API used in this lab can't be accessed anymore. We have an updated
lesson [here](https://github.com/learn-co-curriculum/python-p4-working-with-apis-v2).

## Learning Goals

- Learn some of the basic ways to work with APIs.

---

## Key Vocab

- **Request**: an attempt by one machine to contact another over the internet.
- **Client**: an application or machine that accesses services being provided by
  a server through the internet.
- **Web Server**: a combination of software and hardware that uses Hypertext
  Transfer Protocol (HTTP) and other protocols to respond to requests made over
  the internet.

---

## Define API

An API, or application programming interface, is a manner in which companies and
organizations, like Twitter or the New York City government, expose their data
and/or functionality to the public (i.e. talented programmers like yourself) for
use. APIs allow us to add important data and functionality to the applications
we build. Here's just a few examples of some of the cool things you can do by
using APIs:

- Create an app that allows users to sign up/sign in via
  Facebook/Google/Twitter/Github/etc.
- Use the NYC Open Data API to get and map data––everything from Health
  Department restaurant ratings to public park locations and hours to New York
  City Public Housing repair issues to noise complaints to public school
  construction, you name it!
- Use the Yelp API to find and deliver popular local spots to your users.
- Use the Weather Underground API to give your users up-to-date weather alerts.
- Use the Ticket Master API to inform your users if their favorite musician has
  an upcoming show.
- So much more!

This is just a small sample of what working with APIs allows us to do as
developers. Throughout the course of your programming life, you'll likely be
exposed to working with many different APIs. You'll even learn how to build your
own API later on in this course. This reading seeks to introduce the topic,
emphasize some of the benefits of getting comfortable working with APIs and
offer a brief intro into some of the common methods of working with APIs.

---

## How to Work with APIs

Different APIs expose their data and functionalities in different ways. However,
there are commonalities among them and there are common approaches that we'll
discuss here. Generally speaking, there are two main uses for APIs––getting data
and adding functionality (i.e. signing in with Facebook or posting to
Instagram). We'll be discussing the "getting data" part of working with APIs
here.

Many APIs are built on what is referred to as a REST-ful framework. That means
that the "endpoints", or URLs to which we can send a request for data, follow
certain conventions. These URLs should allow you to request information, send
information, update information and delete information. Let's focus on the
"getting information" request.

### Retrieving Data from an API

For this walk-through, we'll be working with the NYC Open Data API. Let's say
we've been hired by the city to create an app that will help parents sign their
children up for after-school activities. To connect parents to after-school
clubs around the city, we need a data set of such clubs. Luckily for us, the
city has collected that information and allows the public to access it via their
Open Data API. Let's check out the documentation for their [after-school club
data set][].

[after-school club data set]:
  https://dev.socrata.com/foundry/data.cityofnewyork.us/szgz-awuh

#### Finding the API Endpoint

Take a few minutes and familiarize yourself with resource linked to above.
Learning how to read API documentation is an important skill. It's okay if you
don't understand everything in the above document. For now, just look it over
briefly.

As you read through, you'll see that the documentation lists the API endpoint.
**Endpoint** refers to the URL we can submit a request to and that will return
to us the desired data.

Open up a new tab in your browser and paste in the following URL:
`http://data.cityofnewyork.us/resource/uvks-tn5n.json`

The page brings you to is the desired set of data! Notice that the data is laid
out in what looks like a big list of nested dictionaries. This is actually a
[JSON](http://json.org/) object, which behaves just like a Python dictionary.
Working with the JSON data returned to you by requests to an API is one of the
reasons why we spent so much time in previous lessons learning how to manipulate
and operate on nested hashes.

**Top Tip:** Once you find the right URL for retrieving your data, test it out
directly in your browser _before_ you try to request the data from inside your
program. If pasting the URL into your browser brings you to the right data, you
can move on. This cuts down on debugging time. This way, once you try to request
the data from within your program, if it doesn't work, at least you'll know it's
something wrong with your code, as opposed to something wrong with the API.

#### Sending a Request to an API from a Program or Application

Now that we understand what an API is and have even dealt with a URL that takes
us to a real API endpoint, let's use that same URL to send a request for data
from a Python program. Fork and clone this repo locally.

Open up `lib/nyc_api.py`. Let's take a look at the code here:

```py
import requests
import json

class GetPrograms:

  def get_programs(self):
    URL = "http://data.cityofnewyork.us/resource/uvks-tn5n.json"

    response = requests.get(URL)
    return response.content


programs = GetPrograms().get_programs()
print(programs)

```

The `requests` library allows your program or application to send HTTP requests.

We define a ` get_programs()` method that assigns our API endpoint to a variable
name `URL`. The method submits a request with that URL using the `get()` method
defined in the `requests` library, and returns the content of the response.

Now, in your terminal in the directory of this lab, run `python lib/nyc_api.py`.
It should output the JSON response from the NYC Open Data API!

### Working with API Data

Take another look at the API described at [after-school club data set]:
https://dev.socrata.com/foundry/data.cityofnewyork.us/szgz-awuh

If you scroll down to the section that lists the data fields available, you'll
see many fields including `agency`. We'll write a new method to list this field
for each program.

Let's write a method, `program_agencies`, that just returns a list of the
organizations that are running our after school programs.

Copy and paste the following code into our GetPrograms class:

```py
def program_agencies(self):
    # we use the JSON library to parse the API response into nicely formatted JSON
        programs_list = []
        programs = json.loads(self.get_programs())
        for program in programs:
            programs_list.append(program["agency"])

        return programs_list
```

At the bottom of the file, comment out:

```py
programs = GetPrograms().get_programs()
print(programs)
```

Let's `print` out a unique list of the agencies. Paste in the following two
lines right underneath where you commented out the above two lines. We will
create a set from the resulting list of agencies to avoid printing duplicates.

```py
programs = GetPrograms()
agencies = programs.program_agencies()

for agency in set(agencies):
    print(agency)

```

Now, run the program with `python lib/nyc_api.py` in your terminal. You should
see something like this:

```txt
Harlem Educational Activities Fund, Inc.
New Life Child Development Corporation
Hudson Guild
Big Five Block Association Inc.
Shuang Wen Academy Network
Sesame Flyers International, Inc.
Women's Housing and Economic Development Corporation (WHEDCO
Latino Pastoral Action Center, Inc.
Inwood Community Services, Inc.
Yeshiva Kehilath Yakov
Filipino American Human Services, Inc.
Manhattan Youth Recreation & Resources Center
Greater Ridgewood Youth Council, Inc
...
```

So far we've used the `requests` to send a request for data to an API endpoint,
a URL. We've operated on the data that was returned to us, making sure it was
formatted properly as JSON and iterating over that JSON to retrieve the name of
the school hosting each after-school program. That was a lot of work! I wonder
if there is an easier way to work with popular APIs...

---

## Solution

The final version of `nyc_api.py`:

```py

import requests
import json

class GetPrograms:

  def get_programs(self):
    URL = "http://data.cityofnewyork.us/resource/uvks-tn5n.json"

    response = requests.get(URL)
    return response.content

  def program_agencies(self):
    # we use the JSON library to parse the API response into nicely formatted JSON
        programs_list = []
        programs = json.loads(self.get_programs())
        for program in programs:
            programs_list.append(program["agency"])

        return programs_list

#programs = GetPrograms().get_programs()
#print(programs)

programs = GetPrograms()
agencies = programs.program_agencies()

for agency in set(agencies):
    print(agency)

```

## Conclusion

To recap: APIs generally either provide a user with data or added functionality.
We can use APIs that serve data to get information for our own applications and
projects. To get this data, we need to send a request to the URL of the API and
know how to work with the response we receive. Many APIs serve data in JSON
format, which needs to be parsed before we can use it. Once parsed, it becomes a
dictionary we can work with and extract data from.

In our example, we were able to retrieve remote information from an API using
the `requests` library. By putting this implementation inside a class, we can
develop highly reusable code that lets us access all sorts of information
remotely.

The `GetPrograms` class used a hard-coded URL, stored as a class constant, and
included an instance method called `get_programs`:

```py
def get_programs(self):
    URL = "http://data.cityofnewyork.us/resource/uvks-tn5n.json"

    response = requests.get(URL)
    return response.content

```

But we could easily adapt this code to be flexible and accept _any_ URL we pass
in using an `initialize` method and an instance variable. The `get_programs`
method is really just getting the response body from the requested URL, so we
could name this `get_response_body` to be more accurate. We could replace the
custom `program_school` method with a general `load_json` method, as well.
Instead of a specific class, we would instead have a class that retrieves JSON
from any provided URL!

---

## Resources

- [GET - Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET)
- [HTTP methods - Mozilla](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)
- [requests](https://requests.readthedocs.io/en/latest/)
- [Python JSON](https://docs.python.org/3/library/json.html)
