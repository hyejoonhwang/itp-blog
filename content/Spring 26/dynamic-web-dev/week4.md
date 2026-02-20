**Node.js** : runtime environment
npm (Node Package Manager)
Express.js : **framework**


## Breaking Down server.js (The Main Server File)
#### 1. Import the library

```javascript
const express = require("express");
```
#### 2. Create your app
```javascript
const app = express();
```
#### 3. Middleware (Setup/Configuration)
```javascript
app.use(express.static("public"));
app.use(express.urlencoded({ extended: true }));
```
#### 4. Routes (The Core Concept)
So What Is a "Route"? 
A **route** is a rule you write in your server code that says:
 "When someone visits **this specific path**, run **this specific code**."
 
A route is just an **if statement for URLs**. You're telling your server:
 "**IF** someone visits **this path**, **THEN** do **this thing**."

- `https` = the **protocol** (how to communicate)
- `www.example.com` = the **domain** (which server to talk to)
- `/blog/category/my-article` = the **path** (which page on that server)

#### 5. Listen for requests
```javascript
app.listen(5001, () => { console.log("server is running"); });
```
"Hey server, start listening for visitors on port 5001, and once you're up and running, print 'server is running' in my terminal so I know it worked."


### Project 4

This week we moved from client-side JavaScript to building our own server using Node.js and Express. This was a big shift because up until now, everything we built ran entirely in the browser. Now we're actually controlling what happens on the backend too.

The Concept
  I made a simple app where people can share their favorite food spots around 370 Jay St. You type in your name, your favorite food place, and what you usually order. The responses show up on the page and you can edit or delete them. It's basically a mini community board for food recommendations near campus.

  Process

  The first thing I had to figure out was how to structure the server. I knew I needed CRUD — Create, Read, Update, and Delete — so I started by thinking about what routes I'd need. Create made sense as a app.post() since we already used forms with POST in the class demo. For Read, Update, and Delete, I went with app.get() routes and passed data through URL query parameters, since that was the same pattern we used with the OMDB API.

  For the data itself, I just used a plain array on the server. Each entry stores a name, a food place, and a menu item. Nothing fancy — push() to add, a loop to rebuild the array without an item for delete, and direct index access for update.

  On the frontend, I needed a way to display the data and let users interact with it. The page fetches all messages from the server on load, then builds each one out with createElement and appendChild. For delete, it just sends a fetch request with the index and refreshes the list. For edit, I went with swapping the message text out for input fields inline — the user changes the values and hits Save, which sends the updated data back to the server.

  The last step was getting it onto my Digital Ocean droplet. Uploaded the files through Cyberduck, ran npm install on the server, and started it with pm2 so it stays running after I disconnect from SSH.


Reflection
  The biggest takeaway for me is understanding how the client and server communicate. Before this week, APIs felt like a black box. Now that I've built my own routes and handled my own requests, I have a much better sense of what's happening behind the scenes. It's a simple app, but the pattern of sending data to a server, processing it, and sending a response back is the foundation of how most web apps work.