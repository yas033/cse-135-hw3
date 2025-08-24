# cse-135-hw3

## npm install json-server

`cd /c/Users/shiya/OneDrive/Desktop/CSE135/cse135-ss2-hw3`

`npm init -y`

`npm install json-server`

## Usage

Create a db.json or db.json5 file

````{
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
}````

## Pass it to JSON Server CLI

`$ npx json-server db.json`




