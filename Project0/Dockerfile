# Use the official Nginx base image
FROM nginx:alpine

# Copy the local configuration file to the container
COPY nginx.conf /etc/nginx/nginx.conf

# Expose port 8080
EXPOSE 8080

# Start Nginx when the container starts
CMD ["nginx", "-g", "daemon off;"]

