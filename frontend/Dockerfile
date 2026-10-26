# Frontend Dockerfile - builds an nginx web server image that serves the page

# Start from the official nginx image (a web server), small alpine version.
FROM nginx:alpine

# Replace nginx's default settings with ours (see nginx.conf).
# Ours serves the web page AND forwards /api/ requests to the backend.
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy the web page and its JavaScript into the folder nginx serves files from.
COPY index.html index.js /usr/share/nginx/html/

# Document that nginx listens on port 80 (normal web traffic).
EXPOSE 80

# No CMD needed: the nginx base image already starts nginx automatically.
