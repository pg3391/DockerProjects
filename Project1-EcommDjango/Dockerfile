# Use an official Python runtime as a parent image
FROM python:3.8-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip3 install --trusted-host pypi.python.org -r requirements.txt

# Expose port 8000
EXPOSE 8000

# Define environment variable
ENV NAME World

# Run the Django development server
CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"]

