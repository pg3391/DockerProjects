Basic Django e-commerce website in a Docker container. We'll go over setting up a simple Django project and app, creating templates, and configuring the project. Here's a step-by-step guide:

**Prerequisites**:
- You should have Docker installed on your development machine.

**Steps**:

1. **Create a New Django Project**:

   Run the following commands to create a new Django project (replace `ecommerce_project` with your preferred project name):

   ```bash
   django-admin startproject ecommerce_project
   ```

   This will create a new directory called `ecommerce_project` with the initial project structure.

2. **Create a Django App**:

   Navigate to your project directory and create a Django app (replace `ecommerce_app` with your preferred app name):

   ```bash
   cd ecommerce_project
   python manage.py startapp ecommerce_app
   ```

3. **Define Models and Views**:

   In the `ecommerce_app` directory, you can define models to represent your e-commerce data and views to handle requests. For example, you can create a `Product` model in `models.py` and views to display products.
    # ecommerce_app/models.py
    from django.db import models

    class Product(models.Model):
        name = models.CharField(max_length=100)
        price = models.DecimalField(max_digits=10, decimal_places=2)


4. **Create Templates**:

   Create HTML templates for your e-commerce website in the `templates` directory within your app. For example, you can create a `product_list.html` template to display a list of products. Here's a basic template:

   ```html
   <!-- ecommerce_app/templates/product_list.html -->
   <!DOCTYPE html>
   <html>
   <head>
       <title>Product List</title>
   </head>
   <body>
       <h1>Product List</h1>
       <ul>
           {% for product in products %}
               <li>{{ product.name }} - ${{ product.price }}</li>
           {% endfor %}
       </ul>
   </body>
   </html>
   ```

   In this template, we assume a `Product` model with fields `name` and `price`. You can adjust this based on your actual model.

5. **Configure Django Settings**:

   Open the `settings.py` file in your project directory and add your app to the `INSTALLED_APPS` list:

   ```python
   # ecommerce_project/settings.py

   INSTALLED_APPS = [
       # ...
       'ecommerce_app',
   ]
   ```

6. **Run Migrations**:

   Apply migrations to create the database schema for your models:

   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

7. **Define URLs**:

   Create URL patterns for your app in the `urls.py` file. For example:

   ```python
   # ecommerce_app/urls.py

   from django.urls import path
   from . import views

   urlpatterns = [
       path('products/', views.product_list, name='product_list'),
   ]
   ```

8. **Create a View**:

   Create a view in the `views.py` file to render the `product_list.html` template and display products:

   ```python
    # ecommerce_app/views.py
    from django.shortcuts import render
    from .models import Product

    def product_list(request):
        products = Product.objects.all()
        return render(request, 'product_list.html', {'products': products})
   ```

9. **Build a Dockerfile**:
``` Dockerfile
# Use an official Python runtime as a parent image
FROM python:3.8-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Expose port 8000
EXPOSE 8000

# Define environment variable
ENV NAME World

# Run the Django development server
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

10. **Build the Docker Image**:

    Build a Docker image from your project directory:

    ```bash
    docker build -t e-commerce-django-app .
    ```

11. **Run the Docker Container**:

    Start the Docker container:

    ```bash
    docker run -d -p 8000:8000 e-commerce-django-app
    ```

12. **Access Your E-commerce Website**:

    Open a web browser and navigate to `http://localhost:8000/products` to access your Django e-commerce website. This URL corresponds to the `product_list` view defined in your app's URLs.

These steps provide a simplified starting point for creating a basic Django e-commerce website in a Docker container. You can further develop and customize your website by adding features and functionality based on your requirements.

13. **For admin Credentials**:
```bash
python3 manage.py createsuperuser
python3 manage.py runserver
```
