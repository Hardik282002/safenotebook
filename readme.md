To run it locally:-
 --cd to iNotebook-Backend-main
 --npm install alll the dependencies
 --same with frontend part
 --Then in iNotebook-Frontend-main run npm run both
 --It starts both frontend and backend together 

# POST/create user

Request Payload Structure:
The request payload should contain the following fields:

name: The name of the user (minimum 2 characters).
email: The email address of the user (valid email format).
password: The password for the user account (minimum 6 characters).
Response Payload Structure:
The response payload structure varies based on the outcome of the operation:

Success Response (Status 201):

status: "PENDING"
message: "OTP Verification mail sent"
data:
userId: The ID of the newly created user.
email: The email address to which the OTP verification mail was sent.
Error Response (Status 400 or 500):

status: "FAILED"
message: A descriptive error message indicating the reason for failure.
error: (Only in case of validation errors) An array containing individual validation errors.
Validation Error Messages:

Name must consists of minimum 2 characters
Enter a valid Email
Password must consists of minimum 6 characters
Additional Notes:

The user's password is encrypted using the bcryptjs package before storing it in the database.
An OTP (One Time Password) verification email is sent to the user's provided email address.
The OTP is hashed before storing it in the database for security purposes.
The email is sent using the configured email transporter and Mailgen library to generate a structured email template.


# POST /login
Request Payload Structure:
{
  "email": "string",
  "password": "string"
}
email (string): The email address of the user.
password (string): The password associated with the user's account.
Response Payload Structure:
{
  "status": "SUCCESS",
  "authToken": "string"
}
status (string): Status of the operation. Possible values: "SUCCESS".
authToken (string): The JSON Web Token (JWT) generated upon successful authentication. This token can be used for subsequent authorized requests.
--Errors:
400 Bad Request: If the request payload is invalid or missing required fields.
404 Not Found: If no user exists with the provided email.
500 Internal Server Error: If there's an internal server error during the login process.
--Additional Notes:
If the user's email is not verified, an OTP verification mail will be sent again. The user needs to verify their email before logging in.
If the user's email is not verified and you choose not to send another OTP but rather return an error response, you can uncomment the appropriate lines in the code provided.
If the provided password does not match the stored password, an error response is returned.
Upon successful login, an authentication token is generated and returned in the response. This token should be included in the headers of subsequent authorized requests for accessing protected routes.

# POST /getuser

--Middleware:
fetchUser

This middleware is responsible for extracting the authenticated user's information from the request.

Response Payload Structure:
{
  "_id": "string",
  "name": "string",
  "email": "string",
  "verified": "boolean",
  "createdAt": "string",
  "updatedAt": "string"
}
_id (string): The unique identifier of the user.
name (string): The name of the user.
email (string): The email address of the user.
verified (boolean): Indicates whether the user's email is verified.
createdAt (string): Timestamp indicating when the user account was created.
updatedAt (string): Timestamp indicating when the user account was last updated.

--Errors:
401 Unauthorized: If the request does not include a valid authentication token or if the token is expired.
500 Internal Server Error: If there's an internal server error while fetching user details.

Additional Notes:
This endpoint requires the user to be authenticated. The authentication token should be included in the headers of the request.
Upon successful retrieval of user details, the response contains the user's information excluding the password field.

# PUT /editUser/:id

Middleware:
fetchUser
This middleware is responsible for extracting the authenticated user's information from the request.
Request Payload Structure:

{
  "name": "string",
  "email": "string"
}
name (string, optional): The updated name of the user. Minimum 2 characters required.
email (string, optional): The updated email address of the user. Should be a valid email format.

Response Payload Structure:
{
  "success": "boolean",
  "user": {
    "_id": "string",
    "name": "string",
    "email": "string",
    "verified": "boolean",
    "createdAt": "string",
    "updatedAt": "string"
  }
}
success (boolean): Indicates whether the operation was successful.
user (object): Updated user details.
_id (string): The unique identifier of the user.
name (string): The updated name of the user.
email (string): The updated email address of the user.
verified (boolean): Indicates whether the user's email is verified.
createdAt (string): Timestamp indicating when the user account was created.
updatedAt (string): Timestamp indicating when the user account was last updated.

Errors:
400 Bad Request: If the request payload is invalid or missing required fields.
401 Unauthorized: If the request does not include a valid authentication token or if the token is expired.
500 Internal Server Error: If there's an internal server error while updating user details.

Additional Notes:
This endpoint requires the user to be authenticated. The authentication token should be included in the headers of the request.
The user can update either their name or email or both.
Upon successful update of user details, the response contains the updated user information.

# DELETE /deleteUser/:id
Middleware:
fetchUser
This middleware is responsible for extracting the authenticated user's information from the request.


Response Payload Structure:
{
  "success": "boolean"
}
success (boolean): Indicates whether the operation was successful.
Errors:
401 Unauthorized: If the request does not include a valid authentication token or if the token is expired.
500 Internal Server Error: If there's an internal server error while deleting the user account.

Additional Notes:
This endpoint requires the user to be authenticated. The authentication token should be included in the headers of the request.
Upon successful deletion of the user account, the response contains a success message indicating the operation was successful.

# POST /finduser
Middleware:
fetchUser
This middleware is responsible for extracting the authenticated user's information from the request.

Request Payload Structure:
{
  "email": "string"
}
email (string): The email address of the user to be found.

Response Payload Structure:

{
  "success": "boolean",
  "authToken": "string"
}
success (boolean): Indicates whether the operation was successful.
authToken (string): The JSON Web Token (JWT) generated for the found user. This token can be used for subsequent authorized requests.

Errors:
400 Bad Request: If the request payload is missing required fields or contains invalid data.
500 Internal Server Error: If there's an internal server error while finding the user.

Additional Notes:
This endpoint requires the user to be authenticated. The authentication token should be included in the headers of the request.
Upon successful finding of the user, an authentication token is generated for that user and returned in the response. This token can be used for authorized requests on behalf of the found user.

# Notes endpoints #

# GET /fetchallNotes

Middleware:
fetchUser
This middleware is responsible for extracting the authenticated user's information from the request.

Response Payload Structure:
[
  {
    "_id": "string",
    "title": "string",
    "content": "string",
    "user": "string",
    "createdAt": "string",
    "updatedAt": "string"
  },
  {
    "_id": "string",
    "title": "string",
    "content": "string",
    "user": "string",
    "createdAt": "string",
    "updatedAt": "string"
  },
  ...
]
_id (string): The unique identifier of the note.
title (string): The title of the note.
content (string): The content of the note.
user (string): The unique identifier of the user who created the note.
createdAt (string): Timestamp indicating when the note was created.
updatedAt (string): Timestamp indicating when the note was last updated.

Errors:
401 Unauthorized: If the request does not include a valid authentication token or if the token is expired.
500 Internal Server Error: If there's an internal server error while fetching the notes.

Additional Notes:
This endpoint requires the user to be authenticated. The authentication token should be included in the headers of the request.
Upon successful retrieval of notes, the response contains an array of notes associated with the authenticated user.

# POST /addNotes

Middleware:
fetchUser
This middleware is responsible for extracting the authenticated user's information from the request.

Request Payload Structure:
{
  "title": "string",
  "description": "string",
  "tag": ["string"],
  "expdate": "string" // optional
}
title (string): The title of the note. Minimum 3 characters required.
description (string): The description or content of the note. Minimum 6 characters required.
tag (array of strings, optional): Tags associated with the note.
expdate (string, optional): The expiration date of the note (if any).

Response Payload Structure:
{
  "_id": "string",
  "title": "string",
  "description": "string",
  "tag": ["string"],
  "expdate": "string", // optional
  "user": "string",
  "createdAt": "string",
  "updatedAt": "string"
}
_id (string): The unique identifier of the note.
title (string): The title of the note.
description (string): The description or content of the note.
tag (array of strings): Tags associated with the note.
expdate (string): The expiration date of the note (if any).
user (string): The unique identifier of the user who created the note.
createdAt (string): Timestamp indicating when the note was created.
updatedAt (string): Timestamp indicating when the note was last updated.

Errors:
400 Bad Request: If the request payload is invalid or missing required fields.
401 Unauthorized: If the request does not include a valid authentication token or if the token is expired.
500 Internal Server Error: If there's an internal server error while adding the note.

Additional Notes:
This endpoint requires the user to be authenticated. The authentication token should be included in the headers of the request.
Upon successful addition of the note, the response contains the details of the newly created note.

# PUT /updateNote/:id

Middleware:
fetchUser
This middleware is responsible for extracting the authenticated user's information from the request.

Request Payload Structure:
{
  "title": "string",
  "description": "string",
  "tag": ["string"],
  "expdate": "string" // optional
}
title (string, optional): The updated title of the note.
description (string, optional): The updated description or content of the note.
tag (array of strings, optional): The updated tags associated with the note.
expdate (string, optional): The updated expiration date of the note.

Response Payload Structure:
{
  "note": {
    "_id": "string",
    "title": "string",
    "description": "string",
    "tag": ["string"],
    "expdate": "string", // optional
    "user": "string",
    "createdAt": "string",
    "updatedAt": "string"
  }
}
note (object): Updated note details.
_id (string): The unique identifier of the note.
title (string): The updated title of the note.
description (string): The updated description or content of the note.
tag (array of strings): The updated tags associated with the note.
expdate (string): The updated expiration date of the note (if any).
user (string): The unique identifier of the user who created the note.
createdAt (string): Timestamp indicating when the note was created.
updatedAt (string): Timestamp indicating when the note was last updated.

Errors:
400 Bad Request: If the request payload is invalid.
401 Unauthorized: If the user is not authorized to update the note (if the user is not the owner of the note).
500 Internal Server Error: If there's an internal server error while updating the note.

Additional Notes:
This endpoint requires the user to be authenticated. The authentication token should be included in the headers of the request.
The user can update the title, description, tags, and expiration date of the note.
Upon successful update of the note, the response contains the updated note details.


## Endpoint: Delete a Note
This endpoint allows the authenticated user to delete a note.

# Way 1: Using POST "/api/notes/deleteNote1"
This method deletes a note based on the note title.


# POST /api/notes/deleteNote1
Middleware:
fetchUser
This middleware is responsible for extracting the authenticated user's information from the request.

Request Payload Structure:
{
  "title": "string"
}
title (string): The title of the note to be deleted.

Response Payload Structure:
{
  "message": "string"
}
message (string): A message indicating the outcome of the deletion operation.

Errors:
400 Bad Request: If no note exists with the provided title.
500 Internal Server Error: If there's an internal server error while deleting the note.

Additional Notes:
This endpoint requires the user to be authenticated. The authentication token should be included in the headers of the request.
If a note with the provided title exists, it will be deleted. Otherwise, an error message will be returned.
# Way 2: Using DELETE "/api/notes/deleteNote"
This method deletes a note based on the note ID.


# DELETE /api/notes/deleteNote/:id

Middleware:
fetchUser
This middleware is responsible for extracting the authenticated user's information from the request.

Request URL Parameters:
id (string): The unique identifier of the note to be deleted.

Response Payload Structure:
{
  "Success": "string",
  "note": {}
}
Success (string): A message indicating the success of the deletion operation.
note (object): Details of the deleted note.

Errors:
400 Bad Request: If no note exists with the provided ID.
401 Unauthorized: If the user is not authorized to delete the note (if the user is not the owner of the note).
500 Internal Server Error: If there's an internal server error while deleting the note.

Additional Notes:
This endpoint requires the user to be authenticated. The authentication token should be included in the headers of the request.
If a note with the provided ID exists and the authenticated user is the owner of the note, it will be deleted. Otherwise, an error message will be returned.

## Endpoint: Get Notes by Tag Name
This endpoint allows the authenticated user to retrieve all notes associated with a specific tag.


# POST /api/notes/fetchNotesByTag
Middleware:
fetchUser
This middleware is responsible for extracting the authenticated user's information from the request.

Request Payload Structure:
{
  "tag": "string"
}
tag (string): The name of the tag for which notes are to be fetched.

Response Payload Structure:
[
  {
    "_id": "string",
    "title": "string",
    "description": "string",
    "tag": ["string"],
    "expdate": "string", // optional
    "user": "string",
    "createdAt": "string",
    "updatedAt": "string"
  },
  {
    "_id": "string",
    "title": "string",
    "description": "string",
    "tag": ["string"],
    "expdate": "string", // optional
    "user": "string",
    "createdAt": "string",
    "updatedAt": "string"
  },
  ...
]
_id (string): The unique identifier of the note.
title (string): The title of the note.
description (string): The description or content of the note.
tag (array of strings): Tags associated with the note.
expdate (string, optional): The expiration date of the note.
user (string): The unique identifier of the user who created the note.
createdAt (string): Timestamp indicating when the note was created.
updatedAt (string): Timestamp indicating when the note was last updated.

Errors:
400 Bad Request: If the request payload is missing required fields.
500 Internal Server Error: If there's an internal server error while fetching the notes.

Additional Notes:
This endpoint requires the user to be authenticated. The authentication token should be included in the headers of the request.
The endpoint retrieves all notes associated with the specified tag for the authenticated user.
If no notes are found with the specified tag, an empty array is returned.


## Endpoint: Delete All Notes
This endpoint allows the authenticated user to delete all of their notes.

# DELETE /deleteAllNotes

Middleware:
fetchUser
This middleware is responsible for extracting the authenticated user's information from the request.

Request Payload Structure:
No request payload is required. Authentication token should be included in the request headers.
Response Payload Structure:
{
  "success": true
}
success (boolean): Indicates whether the operation was successful.

Errors:
500 Internal Server Error: If there's an internal server error while deleting the notes.

Additional Notes:
This endpoint requires the user to be authenticated. The authentication token should be included in the headers of the request.
Upon successful deletion of all notes, the response contains a success message indicating the operation was successful.

## Endpoint: Add Shared Note
This endpoint allows the authenticated user to add a shared note.

# POST /addSharedNote

Middleware:
fetchUser
This middleware is responsible for extracting the authenticated user's information from the request.

Request Payload Structure:
{
  "title": "string",
  "description": "string",
  "tag": ["string"],
  "expdate": "string" // optional
}
title (string): The title of the shared note.
description (string): The description or content of the shared note.
tag (array of strings): Tags associated with the shared note. Here, "shared" tag is assigned.
expdate (string, optional): The expiration date of the shared note.

Response Payload Structure:
{
  "_id": "string",
  "title": "string",
  "description": "string",
  "tag": ["string"],
  "expdate": "string", // optional
  "user": "string",
  "createdAt": "string",
  "updatedAt": "string"
}
_id (string): The unique identifier of the shared note.
title (string): The title of the shared note.
description (string): The description or content of the shared note.
tag (array of strings): Tags associated with the shared note.
expdate (string): The expiration date of the shared note (if any).
user (string): The unique identifier of the user who created the shared note.
createdAt (string): Timestamp indicating when the shared note was created.
updatedAt (string): Timestamp indicating when the shared note was last updated.

Errors:
500 Internal Server Error: If there's an internal server error while adding the shared note.

Additional Notes:
This endpoint requires the user to be authenticated. The authentication token should be included in the headers of the request.
Upon successful addition of the shared note, the response contains the details of the newly created shared note.



































