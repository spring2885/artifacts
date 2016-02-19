# Using CURL with the Backend API.

Now that the backend API server works, you can interact with it using [curl](https://curl.haxx.se/).
If you are not familiar with curl, it's installed on most Mac and Linux machines, and is pretty useful.

## Setup

First thing you'll need to do is to start the server. You can do that either from Eclipse by running
the ```ServerApplication``` java class, or from the commandline using the command: ```mvn spring-boot:run```
or the ```run.sh``` script in the ```git/backend/server``` directory.

Once running, you must authenticate yourself once.  If you don't have a user account already
open the root page ````http://localhost:8888/```` with your web browser and follow the link
to create a new account. When authenticating, your username is the email address and the
password is the password you picked.  You'll also see your account in the ```Person``` table of
the MySQL backend database.

### Login:
First you must log in, this will store a cookie with the JSESSIONID into the cookie-jar you specify.
(The cookie-jar is just a text file... go ahead, take a look)

Command:
````
curl -v --cookie-jar cookie -d username=rob@spring-2885.org -d password=welcome -L http://localhost:8888/login
````

My results:
````
*   Trying ::1...
* Connected to localhost (::1) port 8888 (#0)
> POST /login HTTP/1.1
> Host: localhost:8888
> User-Agent: curl/7.43.0
> Accept: */*
> Content-Length: 29
> Content-Type: application/x-www-form-urlencoded
> 
* upload completely sent off: 29 out of 29 bytes
< HTTP/1.1 302 Found
< Server: Apache-Coyote/1.1
< X-Content-Type-Options: nosniff
< X-XSS-Protection: 1; mode=block
< Cache-Control: no-cache, no-store, max-age=0, must-revalidate
< Pragma: no-cache
< Expires: 0
< X-Frame-Options: DENY
* Added cookie JSESSIONID="2D8C0E285ACCCCD7D11AB0A67C243861" for domain localhost, path /, expire 0
< Set-Cookie: JSESSIONID=2D8C0E285ACCCCD7D11AB0A67C243861; Path=/; HttpOnly
< Location: http://localhost:8888/
< Content-Length: 0
< Date: Fri, 19 Feb 2016 01:02:04 GMT
< 
* Connection #0 to host localhost left intact
* Issue another request to this URL: 'http://localhost:8888/'
* Switch from POST to GET
* Found bundle for host localhost: 0x7faf38d01350
* Re-using existing connection! (#0) with host localhost
* Connected to localhost (::1) port 8888 (#0)
> GET / HTTP/1.1
> Host: localhost:8888
> User-Agent: curl/7.43.0
> Accept: */*
> Cookie: JSESSIONID=2D8C0E285ACCCCD7D11AB0A67C243861
````

The line that is important here is this one:
````
< Set-Cookie: JSESSIONID=2D8C0E285ACCCCD7D11AB0A67C243861; Path=/; HttpOnly
````
This is where Spring set a cookie that identifies our user session on the back end.  
Each REST api call we make must include this cookie so the server can match the request
up with our "logged in" user.  This is how HTTP, a stateless protocol, appears stateful
to browsers.


## Fetch profile by ID:
curl -v --cookie cookie -L http://localhost:8888/api/v1/profiles/1

Notice the cookie that's on each header. This is the session state that shows you are logged in:
````
Cookie: JSESSIONID=2D8C0E285ACCCCD7D11AB0A67C243861
````  

### Example
````
 curl -v --cookie cookie -L http://localhost:8888/api/v1/profiles/1
*   Trying ::1...
* Connected to localhost (::1) port 8888 (#0)
> GET /api/v1/profiles/1 HTTP/1.1
> Host: localhost:8888
> User-Agent: curl/7.43.0
> Accept: */*
> Cookie: JSESSIONID=2D8C0E285ACCCCD7D11AB0A67C243861
> 
< HTTP/1.1 200 OK
< Server: Apache-Coyote/1.1
< X-Content-Type-Options: nosniff
< X-XSS-Protection: 1; mode=block
< Cache-Control: no-cache, no-store, max-age=0, must-revalidate
< Pragma: no-cache
< Expires: 0
< X-Frame-Options: DENY
< X-Application-Context: application:8888
< Content-Type: application/json;charset=UTF-8
< Transfer-Encoding: chunked
< Date: Fri, 19 Feb 2016 00:52:23 GMT
< 
* Connection #0 to host localhost left intact


{
  "id": 1,
  "name": null,
  "student_id": 0,
  "title": null,
  "about_me": null,
  "resume_url": null,
  "image_url": null,
  "email": "rob",
  "phone": null,
  "occupation": null,
  "company_name": null,
  "birthdate": null,
  "variety": "0",
  "last_login_date": null
}

````

### Other Commands

Now that you know how to make an API call to one command, the rest are the same.

Also note that after you log in to the web browser the **try it now** feature on 
the swagger-ui page at ````http://localhost:8888/api-docs/index.html```` should
work as expected.  However CURL shows you how everything works and that's important 
to know.

