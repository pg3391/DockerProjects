Certainly! Here's a simple example of a basic HTML and JavaScript-based GUI website that you can run in a Docker container. This example provides a single HTML page with a button, and clicking the button triggers a JavaScript function to change the content on the page.

**index.html**:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Simple GUI Website</title>
</head>
<body>
    <h1>Welcome to the Simple GUI Website</h1>
    <p>Click the button to change the content:</p>
    <button onclick="changeContent()">Click Me</button>
    <p id="content">Hello, Docker!</p>

    <script>
        function changeContent() {
            var contentElement = document.getElementById("content");
            contentElement.innerHTML = "Content Changed!";
        }
    </script>
</body>
</html>
```

This HTML page includes a button that, when clicked, calls the `changeContent` JavaScript function to change the text displayed on the page.

To run this simple GUI website in a Docker container, you can create a Dockerfile as follows:

**Dockerfile**:

```Dockerfile
# Use an official web server image as the base image
FROM nginx:alpine

# Copy the HTML file to the web server's document root
COPY index.html /usr/share/nginx/html/index.html
```

This Dockerfile uses the lightweight NGINX web server image as the base image and copies the `index.html` file into the web server's document root.

**Build and Run the Docker Container**:

Assuming you have both the `index.html` and `Dockerfile` in the same directory, you can build and run the Docker container as follows:

1. Build the Docker image:

   ```bash
   docker build -t simple-gui-website .
   ```

2. Run the Docker container:

   ```bash
   docker run -d -p 80:80 --name simple-gui-website-container simple-gui-website
   ```

This will start an NGINX web server with the simple GUI website. You can access the website in your web browser at "http://localhost." When you click the button on the web page, the content will change to "Content Changed!" as defined by the JavaScript function.