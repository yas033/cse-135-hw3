# cse-135-hw3

## Part 0

### JSON Server running

#### npm install json-server ( Install and Usage )

`cd /var/www/cse135.online/public_html`

`npm init -y`

`npm install json-server`

`npm install json-server@0.17.4 --save --force`


```{
"posts": [
    { "id": "1", "title": "a title", "views": 100 },
    { "id": "2", "title": "another title", "views": 200 }
  ],
  "comments": [
    { "id": "1", "text": "a comment about post 1", "postId": "1" },
    { "id": "2", "text": "another comment about post 1", "postId": "1" }
  ],
  "profile": {
    "name": "typicode"
  }
} 

```

Pass it to JSON Server CLI

`$ npx json-server db.json`

Get a REST API `$ curl http://localhost:3000/posts/1`

```{
{
  "id": "1",
  "title": "a title",
  "views": 100
}

```

Leave it open, and open a new terminal: Run `npx json-server --help` for a list of options

### posts: 6 endpoints（GET/POST/PUT/PATCH/DELETE）

GET /posts → `GET /json/posts` → get all posts

GET /posts/:id  → `GET /json/posts/1` → get post with id=1

POST /posts → `POST /json/posts` → add a new post

PUT /posts/:id → `PUT /json/posts/1` → replace a post with id=1

PATCH /posts/:id → `PATCH /json/posts/1` → partially update post with id=1

DELETE /posts/:id → `DELETE /json/posts/1` → remove post with id=1

#### comments: 6 endpoints（GET/POST/PUT/PATCH/DELETE , same structure as posts)

#### profile: 3 endpoints（GET/PUT/PATCH）

`GET /json/profile`

`PUT /json/profile`

`PATCH /json/profile`

#### Params 
`https://github.com/typicode/json-server/blob/main/README.md`

give write access to db.json
`chmod 666 db.json`


After the JSON Server is installed, you will need to expose the server on an endpoint that isn't localhostLinks to an external site. to your droplet. To do that, use the following commands:

Allow port 3000 through the firewall

`sudo ufw allow 3000`

Install pm2 globally

`sudo npm install -g pm2@latest`

Start JSON Server in the background with pm2

`pm2 start npx -- json-server db.json --port 3000 --host 0.0.0.0`

Check running processes

`pm2 list`

Follow the example: `https://github.com/typicode/json-server/issues/253`

1. `cd /var/www/cse135.online/public_html`
   
2. `npm install json-server --save`

3. `ls` we can see there was created a  node_modules folder 

4. inside `ls node_modules | grep json-server` there is the `json-server`

`npm install json-server --save` 

5. run with`node server.js` 

6. if there thrown the error,use `sudo lsof -i :3000` to check if there is any "node" still open `kill -9 put_PID_number_here` kill the certain node 

7. Stop the server

`pm2 stop all` 

after made the node server.js, we can run:
`pm2 start server.js`

Now, our server should be up and running at `cse135.online:3000` To test it, you can make a simple GET request to `cse135.online:3000/posts` and we get a small JSON packet (data in db.json/posts)in return.


### Proxy setup

#### To hide the :3000 port from the URL

1. Enable Apache proxy modules

   `sudo a2enmod proxy`

   `sudo a2enmod proxy_http`

2. Edit your Virtual Host config and Add proxy rules
    `cd /etc/apache2/sites-available`
   
   `sudo nano cse135.online-le-ssl.conf`add those three lines under "DocumentRoot"
```
ProxyPreserveHost On
ProxyPass /json http://localhost:3000
ProxyPassReverse /json http://localhost:3000
```
   Save and exit.

3. After those lines have been added, save and quit from the file. Restart your server using

   `cd /var/www/cse135.online/public_html`

   `sudo systemctl restart apache2`

   `pm2 start server.js` (if anything is in using, kill it , then restart `pm2 restart server`)

   Now your JSON server should be all configured! The base route for it is http://cse135.online/json

   Test new URL http://cse135.online/json and http://cse135.online/json/posts

4. Remove 3000 status for safty reason: `sudo ufw status numbered` (Ignore any 3000 rule that has a colon after it, we only need 3000 and 3000(v6))
   `sudo ufw delete the_number_that_we_need`(let's say "sudo ufw delete 7") refresh the numbers with `sudo ufw status numbered` then delete the next one if needed


### Sign up Postman

Hit the endpoint with Postman, and get the feel of how a REST endpoint for a CRUD application works

#### REST = Representational State Transfer

It’s an architectural style for designing web APIs.

Core idea: every resource (data) is represented by a URL.

Example:

`/posts` → represents the “posts” resource

`/posts/1` → represents the post with `id=1`

#### CRUD = Create, Read, Update, Delete

These are the four basic operations you can perform on data.


## part 1

ssh grader@cse135.online

id: grader

password: Grader!PassWord8888 $$$


server path:

cd /var/www/cse135.online/public_html/hw3

（`pm2 kill`， `sudo pm2 list`->`sudo pm2 delete all`）

json-server:

npx json-server db.json --port 3000


API endpoints:

https://cse135.online/json/sessions

https://cse135.online/json/events



`sudo pm2 startup`

`sudo pm2 save`

stop：`sudo pm2 stop json-server`

restart：`sudo pm2 restart json-server`

delete：`sudo pm2 delete json-server`


for some reason it was fine on local version, but after connect to site getting 403 forbidden

We used the 

```
curl -i -X POST https://cse135.online/json/sessions \
  -H "Content-Type: application/json" \
  -d '{"test":"modsec tuned"}'
```

in the terminal is still working fine


````
yanhua@CSE135-ss2-tap:/var/www/cse135.online/public_html/hw3$ curl -i -X POST https://cse135.online/json/sessions \
  -H "Content-Type: application/json" \
  -d '{"test":"modsec tuned"}'
HTTP/1.1 201 Created
Date: Sat, 30 Aug 2025 06:15:51 GMT
Server: CSE135 Server
X-Powered-By: Express
Vary: Origin,X-HTTP-Method-Override,Accept-Encoding
Access-Control-Allow-Credentials: true
Cache-Control: no-cache
Pragma: no-cache
Expires: -1
Access-Control-Expose-Headers: Location
Location: http://cse135.online/sessions/29
X-Content-Type-Options: nosniff
Content-Type: application/json; charset=utf-8
Content-Length: 40
ETag: W/"28-AAuNEm3gPLoBvd4J5Na36X1oZvE"

{
  "test": "modsec tuned",
  "id": 29
````
