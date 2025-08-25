# cse-135-hw3

# Part 0

## JSON Server running

### npm install json-server ( Install and Usage )

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

posts: 6（GET/POST/PUT/PATCH/DELETE））

GET /posts

GET /posts/:id

POST /posts

PUT /posts/:id

PATCH /posts/:id

DELETE /posts/:id



comments: 6 （GET/POST/PUT/PATCH/DELETE））

profile: 3 （GET/PUT/PATCH）


### Params 
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


## Proxy setup

### To hide the :3000 port from the URL

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

   Test new URL `cse135.online/json` and `cse135.online/json/posts`
   
