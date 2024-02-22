# API Testing with Postman

These instructions provide a basic overview of how to test different API methods using Postman, along with examples of endpoints and request body payloads. Make sure to replace the placeholder values with actual API endpoint URLs, request bodies, and other parameters as required by your API.

### GET Request:

1. Open Postman.
2. Create a new request.
3. Set the request type to GET.
4. Enter the API endpoint URL 
(e.g., get information about all users `https://api.example.com/users`, or a single user `https://api.example.com/:userId`).
5. Click "Send" to execute the GET request.
6. View the response data in the response body.

### POST Request:

1. Open Postman.
2. Create a new request.
3. Set the request type to POST.
4. Enter the API endpoint URL 
(e.g., add a single user `https://api.example.com/users` or order a single book `https://api.example.com/orders` ).
5. Add JSON body payload if required (e.g., `{ "name": "John", "email": "john@example.com" }`).
6. Click "Send" to execute the POST request.
7. View the response data in the response body.

### PUT Request:

1. Open Postman.
2. Create a new request.
3. Set the request type to PUT.
4. Enter the API endpoint URL 
(e.g., update every part of user information `https://api.example.com/users/123`, `https://api.example.com/:userId`).
5. Add JSON body payload with updated data (e.g., `{ "name": "Updated John", "email": "updatedjohn@example.com" }`).
6. Click "Send" to execute the PUT request.
7. View the response data in the response body.

PUT is used to update or replace an entire resource.

### PATCH Request:

1. Open Postman.
2. Create a new request.
3. Set the request type to PATCH.
4. Enter the API endpoint URL 
(e.g., update part of user information `https://api.example.com/users/123`, `https://api.example.com/:userId`).
5. Add JSON body payload with partial data to update (e.g., `{ "email": "updatedemail@example.com" }`).
6. Click "Send" to execute the PATCH request.
7. View the response data in the response body.

PATCH is used to apply partial modifications to a resource.

### DELETE Request:

1. Open Postman.
2. Create a new request.
3. Set the request type to DELETE.
4. Enter the API endpoint URL 
(e.g., delete a user `https://api.example.com/users/123`, `https://api.example.com/:userId`).
5. Click "Send" to execute the DELETE request.
6. View the response data in the response body.
