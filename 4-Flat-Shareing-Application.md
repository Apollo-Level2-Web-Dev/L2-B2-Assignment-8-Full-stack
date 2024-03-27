# **Flat Share Application**

## **Assignment Requirements:**

### **Technology Stack:**

- **Programming Language:** TypeScript
- **Web Framework:** Express.js
- **Object Relational Mapping (ORM):** Prisma for PostgreSQL
- **Authentication:** JWT (JSON Web Tokens)

## **Models:**

### **1. User Model:**

- **Fields:**
    - **id (String):** A distinctive identifier for each user.
    - **name (String):** The name of the user.
    - **email (String):** The email address of the user.
    - **password (String):** The hashed password of the user.
    - **createdAt (DateTime):** The timestamp indicates when the user was created.
    - **updatedAt (DateTime):** The timestamp indicates when the user was last updated.

### **2. Flat Model:**

- **Fields:**
    - **id (String):** A distinctive identifier for each flat.
    - **squareFeet(Integer):** The size of the flat by means of square feet.
    - **totalBedrooms(Integer):** Number of bedrooms (e.g. 1/2/3/4).
    - **totalRooms(Integer):** Number of bedrooms (e.g. 1/2/3/4/5/6).
    - **utilitiesDescription(String):** The utilities available (e.g. gas, water, electricity, generator, internet).
    - **location (String):** The location where the flat is currently located (e.g. holding number, street, area, city).
    - **description (String):** A description of the flat (e.g. Master bed room size(square feet), others room size(square feet), open space, veranda, terrace space etc. ).
    - **rent(Integer):** Rent Amount.
    - **availability(Boolean):** whether it is available now or not.
    - **advanceAmount(Integer):** Advance amount.
    - **createdAt (DateTime):** The timestamp indicates when flat was created.
    - **updatedAt (DateTime):** The timestamp indicates when the flat was last updated.

### **3. Booking Model:**

- **Fields:**
    - **id**(**String**): A distinctive identifier for each booking.
    - **flatId (String): reference Id from flat model**.
    - **userId (String): reference Id from user model**.
    - **status (String):** is it available or not ref from availability of flat model  (e.g.  PENDING, BOOKED, REJECTED).By default it will be PENDING
    - **createdAt (DateTime):** The timestamp indicates when the application was created.
    - **updatedAt (DateTime):** The timestamp indicates when the application was last updated.
    
- **UserProfile Model:**
    - **id (String):** A distinctive identifier for each user profile.
    - **userId (String):** A reference to the user associated with the profile.
    - **bio (String):** A brief bio or description of the user.
    - **profession (String):** Profession of the user.
    - **address (String):** Permanent address of the user
    - **createdAt (DateTime):** The timestamp indicating when the user profile was created.
    - **updatedAt (DateTime):** The timestamp indicating when the user profile was last updated.

## **Error Handling:**

Implement proper error handling throughout the application. Use global error handling middleware to catch and handle errors, providing appropriate error responses with status codes and error messages.

### **Sample Error Response:**

- For Validation Error (Zod):

```json
{
    "success": false,
    "message": "Name field is required. Email must be a valid email address.",
    "errorDetails": {
        "issues": [
            {
                "field": "name",
                "message": "Name field is required."
            },
            {
                "field": "email",
                "message": "Email must be a valid email address."
            }
        ]
    }
}

```

- For General or Generic Errors

```json
{
    "success": false,
    "message": "error mesage",
    "errorDetails": error
}

```

- Unauthorized Error Response

If an unauthorized access attempt is detected, the system will respond with the following error message:

```json
{
    "success": false,
    "message": "Unauthorized Access",
    "errorDetails": error
}

```

This error may occur under the following circumstances:

- **JWT Expiry:** The provided JWT (JSON Web Token) has expired.
- **Invalid JWT:** The JWT provided is invalid or malformed.
- **Undefined JWT:** No JWT is provided in the request headers.
- **Not Authorized User:** The user does not possess the required permissions for the requested action or resource.
- **Access Denied:** The user is attempting to access a resource without the necessary authorization.

## **Endpoints:**

### **1. User Registration**

- **Endpoint:** **`POST /api/register`**
- **Request Body:**

```json
{
  "name": "John Doe",
  "email": "johndoe@example.com",
  "password": "securepassword",
  "bio":"My Bio",
  "profession":"Developer"
  "address":"Permanent Address"
}
```

- **Response** (Response should not include the password):

```json
{
    "success": true,
    "statusCode": 201,
    "message": "User registered successfully",
    "data": {
        "id": "b9964127-2924-42bb-9970-60f93c016bvf",
        "name": "John Doe",
        "email": "john@example.com",
        "createdAt": "2024-03-24T12:00:00Z",
        "updatedAt": "2024-03-24T12:00:00Z"
    }
}
```

### **2. User Login**

- **Endpoint:** **`POST /api/login`**
- **Request Body:**

```json
{
    "email": "john@example.com",
    "password": "password"
}
```

- **Response:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "User logged in successfully",
    "data": {
        "id": "b9964127-2924-42bb-9970-60f93c016bvf",
        "name": "John Doe",
        "email": "john@example.com",
        "token": "<JWT token>",
    }
}
```

### **3. Add a Flat**

- **Endpoint:** **`POST /api/flats`**
- **Request Headers:**
    - `Authorization: <JWT_TOKEN>`
- **Request Body:**

```json
{
    "squareFeet": 1200,
    "totalBedrooms": 2,
    "totalRooms": 4,
    "utilitiesDescription": "Includes water and electricity",
    "location": "123 Main Street, Cityville",
    "description": "Cozy apartment with ample natural light and modern amenities.",
    "rent": 1500,
    "advanceAmount": 2000
}
```

- **Response:**

```json
{
    "success": true,
    "statusCode": 201,
    "message": "Flat added successfully",
    "data": {
        "id": "b9964127-2924-42bb-9970-60f93c016ghs",
        "squareFeet": 1200,
        "totalBedrooms": 2,
        "totalRooms": 4,
        "utilitiesDescription": "Includes water and electricity",
        "location": "123 Main Street, Cityville",
        "description": "Cozy apartment with ample natural light and modern amenities.",
        "rent": 1500,
        "availability": true,
        "advanceAmount": 2000,
        "createdAt": "2024-03-24T12:00:00Z",
        "updatedAt": "2024-03-24T12:00:00Z"
    }
}
```

### **4. Get Paginated and Filtered Flats**

- **Endpoint:** **`GET /api/flats`**

**Query Parameters for API Requests:**

When interacting with the API, you can utilize the following query parameters to customize and filter the results according to your preferences.

- **searchTerm**: (Optional) Searches for flats based on a keyword or phrase. Only applicable to the following fields: , `location`, `description`  , `utilitiesDescription`(searching mode insensitive)
- **page**: (Optional) Specifies the page number for paginated results. Default is 1. Example: ?page=1
- **limit**: (Optional) Sets the number of items per page. Default is a predefined limit. Example: ?limit=10
- **sortBy**: (Optional) Specifies the field by which the results should be sorted. Only applicable to the following fields: `rent`, `advanceAmount`, `squareFeet` , `totalRooms` , `totalBedrooms`
    - Example: ? sortBy=rent
- **sortOrder**: (Optional) Determines the sorting order, either 'asc' (ascending) or 'desc' (descending). Example: ?sortOrder=desc
- **availability**: (Optional) Filters flats based on the availability.
    - Example: `?availiblity=true`

- **Response:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "Flats retrieved successfully",
    "meta": {
        "total": 20,
        "page": 1,
        "limit": 10
    },
    "data": [
        {
        "id": "b9964127-2924-42bb-9970-60f93c016ghs",
        "squareFeet": 1200,
        "totalBedrooms": 2,
        "totalRooms": 4,
        "utilitiesDescription": "Includes water and electricity",
        "location": "123 Main Street, Cityville",
        "description": "Cozy apartment with ample natural light and modern amenities.",
        "rent": 1500,
        "availability": true,
        "advanceAmount": 2000
        "createdAt": "2024-03-24T12:00:00Z",
        "updatedAt": "2024-03-24T12:00:00Z"
        },
        // More flats
    ]
}

```

### 5. Update Flat Information

- **Endpoint:** **`PUT /api/flats/:flatId`**
- **Request Headers:**
    - **`Authorization: <JWT_TOKEN>`**
- **Request Body:**

```json
{
    "location": "Shelter ABC"
}

```

- **Response:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "Flat information updated successfully",
    "data": {
        "id": "b9964127-2924-42bb-9970-60f93c016ghs",
        "squareFeet": 1200,
        "totalBedrooms": 2,
        "totalRooms": 4,
        "utilitiesDescription": "Includes water and electricity",
        "location": "123 Main Street, Cityville",
        "description": "Cozy apartment with ample natural light and modern amenities.",
        "rent": 1500,
        "availability": true,
        "advanceAmount": 2000
        "createdAt": "2024-03-24T12:00:00Z",
        "updatedAt": "2024-03-24T12:05:00Z"
    }
}

```

This endpoint allows users with appropriate permissions to update the profile of a pet.

### 6. Flat Booking Request

- **Endpoint:** **`POST /api/booking-applications`**
- **Request Headers:**
    - `Authorization: <JWT_TOKEN>`
- **Request Body:**

```json
{
    "flatId": "b9964127-2924-42bb-9970-60f93c016ghs"
}
```

- **Response:**

```json
{
    "success": true,
    "statusCode": 201,
    "message": "Booking requests submitted successfully",
    "data": {
        "id": "9b0dadf5-10fd-41d1-8355-80e67c85727c",
        "userId": "b9964127-2924-42bb-9970-60f93c016bvf",
        "flatId": "b9964127-2924-42bb-9970-60f93c016ghs",
        "status":"PENDING",
        "createdAt": "2024-03-24T12:00:00Z",
        "updatedAt": "2024-03-24T12:00:00Z" 
    }
}
```

### 7. Get  Booking  Requests

- **Endpoint:** **`GET /api/booking-requests`**
- **Request Headers:**
    - `Authorization: <JWT_TOKEN>`
- **Response:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "Booking requests retrieved successfully",
    "data": [
       {
	        "id": "9b0dadf5-10fd-41d1-8355-80e67c85727c",
	        "userId": "b9964127-2924-42bb-9970-60f93c016bvf",
	        "flatId": "b9964127-2924-42bb-9970-60f93c016ghs",
	        "status":"BOOKED",
	        "createdAt": "2024-03-24T12:00:00Z",
	        "updatedAt": "2024-03-24T12:00:00Z" 
        }
        // More adoption applications
    ]
}
```

### 8. Update Booking Flat Application Status

- **Endpoint:** **`PUT /api/booking-requests/:bookingId`**
- **Request Headers:**
    - `Authorization: <JWT_TOKEN>`
- **Request Body:**

```json
{
    "status": "BOOKED"
}

```

- **Response:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "Booking request updated successfully",
    "data": {
        "id": "9b0dadf5-10fd-41d1-8355-80e67c85727c",
        "userId": "b9964127-2924-42bb-9970-60f93c016bvf",
        "flatId": "b9964127-2924-42bb-9970-60f93c016ghs",
        "status":"BOOKED",
        "createdAt": "2024-03-24T12:00:00Z",
        "updatedAt": "2024-03-24T12:00:00Z" 
    }
}
```

### 9. Get User Profile

- **Endpoint:** **`GET /api/profile`**
- **Request Headers:**
    - `Authorization: <JWT_TOKEN>`
- **Response:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "User profile retrieved successfully",
    "data": {
        "id": "9b0dadf5-10fd-41d1-8355-80e67c85727c",
        "userId":"9b0dadf5-10fd-41d1-8355-80e67c8577y"
        "bio ":"John Doe",
        "profession ": "john@example.com",
        "address":"Permanent address"
        "createdAt": "2024-03-24T12:00:00Z",
        "updatedAt": "2024-03-24T12:00:00Z"
    }
}
```

### 10. Update User Profile

- **Endpoint:** **`PUT /api/profile`**
- **Request Headers:**
    - `Authorization: <JWT_TOKEN>`
- **Request Body:**

```json
{ "bio":"My Bio" }
```

- **Response:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "User profile updated successfully",
    "data": {
        "id": "9b0dadf5-10fd-41d1-8355-80e67c85727c",
        "userId":"9b0dadf5-10fd-41d1-8355-80e67c8577y",
        "bio":"My Bio",
        "profession ": "john@example.com",
        "address":"Permanent address",
        "createdAt": "2024-03-24T12:00:00Z",
        "updatedAt": "2024-03-24T12:00:00Z"
    }
}
```

This endpoint allows users to update their profile information such as bio, profession, address