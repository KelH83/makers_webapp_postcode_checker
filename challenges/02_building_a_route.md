# Building a Route

_**This is a Makers Vine.** Vines are designed to gradually build up sophisticated skills. They contain a mixture of text and video, and may contain some challenge exercises without proposed solutions. [Read more about how to use Makers
Vines.](https://github.com/makersacademy/course/blob/main/labels/vines.md)_

Learn to build a route to respond to HTTP requests.

<!-- OMITTED -->

# Building a Route

## Routing

Remember that a web server receives HTTP requests, execute some code depending on the received request, and returns a response.

To decide on what code to execute depending on the request, internally Flask keeps a "routing" table, which associates a given request **method and path** to a block of Python code:

| HTTP Method | Path | Python code                        |
| ----------- | ---- | ---------------------------------- |
| GET         | /    | `# some code to execute`           |
| POST        | /    | `# some different code to execute` |

<details>
    <summary>:confused: Are these like methods in Python classes?</summary>

    ---

    No. These are a different kind of method — a HTTP method.

    Each HTTP request comes with a label called a method which tells the server what kind of request it is. Common methods are GET (usually used to retrieve data) and POST (usually used to send data).

    ---

</details>

Different requests will execute different code in our Flask application, and lead to different responses being sent back.

Here is an example of a minimal Flask application, configuring a single **route**:

```python
# file: app.py
from flask import Flask

app = Flask(__name__)

# Declares a route that listens for a GET request to the path /
@app.route('/', methods=['GET'])
def index():
    # The code here is executed when a request is received and we need to 
    # send a response. 

    # We can return a string which will be used as the response content.
    # Unless specified, the response status code will be 200 (OK).
    return 'Some response data'
```

The Python function associated with a HTTP method and path is called a "route". The code in this block is executed _only_ is the received request matches the HTTP method and path.

When Flask received a request, it looks through all the route blocks configured in that class, and execute the code of the first one matching the request.

```python
from flask import Flask

app = Flask(__name__)

# Let's look at an example where Flask receives this request:
# > GET /

# There are a number of routes. We'll look through each one in turn and see if
# it matches.

@app.route('/', methods=['POST'])
def post_index():
    # DOES NOT RUN: The HTTP method (GET) doesn't match the route's (POST)
    return "Not me! :("

@app.route('/hello', methods=['GET'])
def get_hello():
    # DOES NOT RUN: The path (`/hello`) doesn't match the route's (`/`)
    return "Not me either!"

@app.route('/', methods=['GET'])
def get_index():
    # RUNS: This route matches! The code inside the block will be executed now.
    return "I am the chosen one!"

@app.route('/', methods=['GET'])
def other_get_index():
    # DOES NOT RUN: This route also matches, but will not be executed.
    # Only the first matching route (above) will run.
    return "It isn't me, the other route stole the show"
```

_In the following sections, we will use the shorthand notation `GET /some_path` to designate a route which responds to `GET` requests to the path `/some_path`._

## Accessing GET request parameters

We can use the `request.args` dictionary inside a route block to access the request _query parameters._

If a client sends a request with a query parameter with key `name` and value `David`, this parameter will be present in the `request.args` dictionary, and we can access the value like this:

```python
from flask import Flask, request # NOTE: we must import `request` too

app = Flask(__name__)

# Request:
# GET /hello?name=David

@app.route('/hello', methods=['GET'])
def hello():
    name = request.args['name'] # The value is 'David'

    # Send back a friendly greeting with the name
    return f"Hello {name}!"

# You can open this with http://localhost:5000/hello?name=David
```

## Accessing POST request parameters

We can also send POST requests with data. However, to retrieve the data, we must
use the `request.form` dictionary instead of `request.args`.

```python
from flask import Flask, request # Remember to import `request`

app = Flask(__name__)

# Request:
# POST /goodbye
#   With body parameter: name=Alice

@app.route('/goodbye', methods=['POST'])
def goodbye():
    name = request.form['name'] # The value is 'Alice'

    # Send back a fond farewell with the name
    return f"Goodbye {name}!"

```

You won't be able to visit that last request using the browser's address bar.

Try sending a POST request with the right data using Postman. You could also
use the `curl` command line tool, like this:

```shell
; curl -X POST -d "name=Alice" http://localhost:5000/goodbye
Goodbye Alice!
```

## Demonstration

[Video demonstration](https://www.youtube.com/watch?v=iCMsemJVbqo) <!-- OMITTED -->

## Exercise

Work through the following in your `hello_web_project` project.

Create a new route that responds to requests sent with:
  * A method `POST`
  * A path `/submit`

Here's the expected behaviour of this route:

```
# Request:
POST /submit

# With body parameters:
name=Leo
message=Hello world

# Expected response (2OO OK):
Thanks Leo, you sent this message: "Hello world"
```

Make sure your server is running — then, using Postman, check the route is working.

[Example Solution](https://www.youtube.com/watch?v=iCMsemJVbqo&t=1106s) <!-- OMITTED -->

## Challenge

Work through the following in your `hello_web_project` project.

Create a new route that responds to requests sent with:
  * A method `GET`
  * A path `/wave`

It should return the text `'I am waving at [NAME]'`, where `[NAME]` is replaced by the value of the `name` _query parameter_.

```
# Request:
GET /wave?name=Leo

# Expected response (200 OK):
I am waving at Leo
```

Make sure your server is running — then, using Postman, check the route is working.


[Next Challenge](03_test_driving_a_route.md)

<!-- BEGIN GENERATED SECTION DO NOT EDIT -->

---

**How was this resource?**  
[😫](https://airtable.com/shrUJ3t7KLMqVRFKR?prefill_Repository=makersacademy%2Fweb-applications-in-python&prefill_File=challenges%2F02_building_a_route.md&prefill_Sentiment=😫) [😕](https://airtable.com/shrUJ3t7KLMqVRFKR?prefill_Repository=makersacademy%2Fweb-applications-in-python&prefill_File=challenges%2F02_building_a_route.md&prefill_Sentiment=😕) [😐](https://airtable.com/shrUJ3t7KLMqVRFKR?prefill_Repository=makersacademy%2Fweb-applications-in-python&prefill_File=challenges%2F02_building_a_route.md&prefill_Sentiment=😐) [🙂](https://airtable.com/shrUJ3t7KLMqVRFKR?prefill_Repository=makersacademy%2Fweb-applications-in-python&prefill_File=challenges%2F02_building_a_route.md&prefill_Sentiment=🙂) [😀](https://airtable.com/shrUJ3t7KLMqVRFKR?prefill_Repository=makersacademy%2Fweb-applications-in-python&prefill_File=challenges%2F02_building_a_route.md&prefill_Sentiment=😀)  
Click an emoji to tell us.

<!-- END GENERATED SECTION DO NOT EDIT -->