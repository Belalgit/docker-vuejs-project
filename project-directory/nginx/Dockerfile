# Use the official Nginx image
FROM nginx:stable-alpine

# Copy custom Nginx config file into the container
COPY nginx.conf /etc/nginx/nginx.conf

# Expose port 80 to allow external traffic
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
