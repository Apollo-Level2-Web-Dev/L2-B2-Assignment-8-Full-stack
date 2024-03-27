# **Travel Buddy Matching Assignment**

## **Assignment Requirements:**

### **Technology Stack:**

- **Programming Language:** TypeScript
- **Web Framework:** Express.js
- **Object Relational Mapping (ORM):** Prisma with PostgreSQL
- **Authentication:** JWT (JSON Web Tokens)

## **Models:**

### **1. User Model:**

- **Fields:**
    - **id (String):** A distinctive identifier for each user.
    - **name (String):** The name of the user.
    - **email (String):** The email address of the user.
    - **p*assword (St*ring):** The hashed password of the user.
    - **createdAt (DateTime):** The timestamp indicates when the user was created.
    - **updatedAt (DateTime):** The timestamp indicates when the user was last updated.

### **2. Trip Model:**

- **Fields:**
    - **id (String):** A distinctive identifier for each trip.
    - **userId (String):** A reference to the user who created the trip.
    - **destination (String):** The destination of the trip.
    - **startDate (String):** The start date of the trip.
    - **endDate (String):** The end date of the trip.
    - **budget (Number):** The budget for the trip.
    - **activities (String[]):** An array of activities planned for the trip.
    - **createdAt (DateTime):** The timestamp indicates when the trip was created.
    - **updatedAt (DateTime):** The timestamp indicates when the trip was last updated.

### **3. Travel Buddy Request Model:**

- **Fields:**
    - **id (String):** A distinctive identifier for each travel buddy pairing.
    - **tripId (String):** A reference to the trip associated with the travel buddy pairing.
    - **userId (String):** A reference to the user who is a potential travel buddy.
    - **status (String):** The status of the travel buddy pairing (e.g., PENDING, APPROVED, REJECTED).
    - **createdAt (DateTime):** The timestamp indicates when the travel buddy pairing was created.
    - **updatedAt (DateTime):** The timestamp indicates when the travel buddy pairing was last updated.

### **4. UserProfile Model:**

- **Fields:**
    - **id (String):** A distinctive identifier for each user profile.
    - **userId (String):** A reference to the user associated with the profile.
    - **bio (String):** A brief bio or description of the user.
    - **age (Integer):** Age of the user.
    - **createdAt (Date):** The timestamp indicating when the user profile was created.
    - **updatedAt (Date):** The timestamp indicating when the user profile was last updated.

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

Error Scenarios: `JWT Expiry`, `Invalid JWT`, `Undefined JWT`, `Not Authorized User`, `Access Denied`, etc.

## **Endpoints:**

**N.B.** For now, no role is required, allowing anyone to perform any operation without restrictions.

**`POST /api/register:`  The request method like GET, PUT, PATCH, DELETE, POST should not be included in the route path. Follow the pattern as shown in the example for every endpoint: `"/api/register"`**

### **1. User Registration**

- **Endpoint:** **`POST /api/register`**
- **Request Body:**
- **Note**: You may need to use transaction for creating user and user profile altogether

```json
{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password", // password should be stored as a hash
    "profile": {
        "bio": "Passionate about helping people find their lost items.",
        "age": 30
		}
}
```

- **Response** (Response should not include the password):
- Note: In response you don’t need to send the user profile info, just send the user info

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

### **3. Create a Trip**

- **Endpoint:** **`POST /api/trips`**
- **Request Headers:**
    - `Authorization: <JWT_TOKEN>`
- **Request Body:**

```json
{
    "destination": "Paris, France",
    "startDate": "2024-06-01",
    "endDate": "2024-06-07",
    "budget": 1500,
    "activities": ["Eiffel Tower visit", "Louvre Museum tour"],
}
```

- **Response:**

```json
{
    "success": true,
    "statusCode": 201,
    "message": "Trip created successfully",
    "data": {
        "id": "b9964127-2924-42bb-9970-60f93c016ghi",
        "userId": "b9964127-2924-42bb-9970-60f93c016bvf",
        "destination": "Paris, France",
        "startDate": "2024-06-01",
        "endDate": "2024-06-07",
        "budget": 1500,
        "activities": ["Eiffel Tower visit", "Louvre Museum tour"],
        "createdAt": "2024-03-24T12:00:00Z",
        "updatedAt": "2024-03-24T12:00:00Z"
		}
}
```

### **4. Get Paginated and Filtered Trips**

- **Endpoint:** **`GET /api/trips`**

**Query Parameters for API Requests:**

When interacting with the API, you can utilize the following query parameters to customize and filter the results according to your preferences.

- `destination`: (Optional) Filter trips by destination.
- `startDate`: (Optional) Filter trips by start date.
- `endDate`: (Optional) Filter trips by end date.
- `budget`: (Optional) Filter trips by budget range. Example: ?minBudget=100&maxBudget=10000
- `searchTerm`: (Optional) Searches for trips based on a keyword or phrase. Only applicable to the following fields: `destination`, `budget`, etc.
- `page`: (Optional) Specifies the page number for paginated results. Default is 1. Example: ?page=2
- `limit`: (Optional) Sets the number of data per page. Default is 10. Example: ?limit=5
- `sortBy`: (Optional) Specifies the field by which the results should be sorted. Only applicable to the following fields: `destination`, `budget`. Example: ?sortBy=budget
- `sortOrder`: (Optional) Determines the sorting order, either 'asc' (ascending) or 'desc' (descending). Example: ?sortOrder=desc
- **Response:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "Trips retrieved successfully",
    "meta": { // only for paginated result
      "page": 1,
      "limit": 10,
      "total": 20
	   },
    "data": [
        {
            "id": "b9964127-2924-42bb-9970-60f93c016ghi",
            "userId": "b9964127-2924-42bb-9970-60f93c016bvf",
            "destination": "Paris, France",
            "startDate": "2024-06-01",
            "endDate": "2024-06-07",
            "budget": 1500,
            "activities": ["Eiffel Tower visit", "Louvre Museum tour"],
            "createdAt": "2024-03-24T12:00:00Z",
            "updatedAt": "2024-03-24T12:00:00Z"
        },
        // More trips
    ]
}
```

### **6. Send Travel Buddy Request**

- **Endpoint:** **`POST /api/trip/:tripId/request`**
- **Request Headers:**
    - `Authorization: <JWT_TOKEN>`
- **Request Body:**

```json
{
    "userId": "b9964127-2924-42bb-9970-60f93c016xyz"
}
```

- **Response:**

```json
{
    "success": true,
    "statusCode": 201,
    "message": "Travel buddy request sent successfully",
    "data": {
        "id": "9b0dadf5-10fd-41d1-8355-80e67c85727c",
        "tripId": "b9964127-2924-42bb-9970-60f93c016ghi",
        "userId": "b9964127-2924-42bb-9970-60f93c016bvf",
        "status": "PENDING",
        "createdAt": "2024-03-24T12:00:00Z",
        "updatedAt": "2024-03-24T12:00:00Z"
    }
}
```

### **7. Get Potential Travel Buddies For a Specific Trip**

- **Endpoint:** **`GET /api/travel-buddies/:tripId`**
- **Request Headers:**
    - `Authorization: <JWT_TOKEN>`
- **Response:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "Potential travel buddies retrieved successfully",
    "data": [
        {
            "id": "9b0dadf5-10fd-41d1-8355-80e67c85727c",
            "tripId": "b9964127-2924-42bb-9970-60f93c016ghi",
            "userId": "b9964127-2924-42bb-9970-60f93c016xyz",
            "status": "PENDING",
            "createdAt": "2024-03-24T12:00:00Z",
            "updatedAt": "2024-03-24T12:00:00Z",
            "user": {
	            "name": "John Doe",
              "email": "john@example.com",
              // other user fields are optional
            }
        },
        // More potential travel buddies
    ]
}
```

### **8. Respond to Travel Buddy Request**

- **Endpoint:** **`PUT /api/travel-buddies/:buddyId/respond`**
- **Request Headers:**
    - `Authorization: <JWT_TOKEN>`
- **Request Body:**

```json
{
    "status": "APPROVED"
}
```

- **Response:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "Travel buddy request responded successfully",
    "data": {
        "id": "9b0dadf5-10fd-41d1-8355-80e67c85727c",
        "tripId": "b9964127-2924-42bb-9970-60f93c016ghi",
        "userId": "b9964127-2924-42bb-9970-60f93c016xyz",
        "status": "APPROVED",
        "createdAt": "2024-03-24T12:00:00Z",
        "updatedAt": "2024-03-24T12:05:00Z"
    }
}
```

### **9. Get User Profile**

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
        "name": "John Doe",
        "email": "john@example.com",
        "createdAt": "2024-03-24T12:00:00Z",
        "updatedAt": "2024-03-24T12:00:00Z"
    }
}
```

### **10. Update User Profile**

- **Endpoint:** **`PUT /api/profile`**
- **Request Headers:**
    - `Authorization: <JWT_TOKEN>`
- **Request Body:**

```json
{
    "name": "John Sina",
    "email": "john.doe@example.com"
}
```

- **Response:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "User profile updated successfully",
    "data": {
        "id": "9b0dadf5-10fd-41d1-8355-80e67c85727c",
        "name": "John Sina",
        "email": "john.doe@example.com",
        "createdAt": "2024-03-24T12:00:00Z",
        "updatedAt": "2024-03-24T12:05:00Z"
    }
}
```

This endpoint allows users to update their profile information such as name and email.