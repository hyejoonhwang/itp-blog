![[Pasted image 20260130001551.png]]

For this week assignment I wanted to make a simple webpage with a flower bouquet I made with Gen AI and a letter to the people who opens it. 

first, I designed the elements using FloraAI and Figma and placed in into the project folder. 

Based on the code I learned from the class I added the background image to fully cover the screen and placed the envelope images on the same position and linked those two elements to the other page so that the user can toggle on and off the envelope. 

I had to google how I can use style in the simplest way, I think I would love to know more on how to style the elements. 

```html
<html>

  <head>

    <title>📬a message from summer has arrived📬</title>

  </head>

  <p>📬a message from summer has arrived📬</p>

    <body>

        <img src="bg-bouquet.png" style="margin: 0; padding: 0; height: 100vh; width: 100vw; object-fit: cover;" />

        <a href="page2.html"> <img src="envelope-closed.png" style="position: absolute; right: 100px; top: 50%; width: 300px; cursor: pointer;" /></a>

    </body>

</html>
```


```html
<html>

  <head>

    <title>📬a message from summer has arrived📬</title>

  </head>

  <p>📬a message from summer has arrived📬</p>

    <body>

        <img src="bg-bouquet.png" style="margin: 0; padding: 0; height: 100vh; width: 100vw; object-fit: cover;" />


        <a href="index.html"> <img src="envelope-open.png" style="position: absolute; right: 100px; top: 50%; width: 300px; cursor: pointer;" /></a>


    </body>

</html>
```

![[Pasted image 20260130002029.png]]