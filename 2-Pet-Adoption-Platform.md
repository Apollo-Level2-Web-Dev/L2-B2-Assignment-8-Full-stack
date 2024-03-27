# **Pet Adoption Platform Assignment**

## **Assignment Requirements:**

### **Technology Stack:**

-  **Programming Language:** TypeScript
-  **Web Framework:** Express.js
-  **Object Relational Mapping (ORM):** Prisma with PostgreSQL
-  **Authentication:** JWT (JSON Web Tokens)

## **Models:**

### **1. User Model:**

-  **Fields:**
   -  **id (String):** A distinctive identifier for each user.
   -  **name (String):** The name of the user.
   -  **email (String):** The email address of the user.
   -  **password (String):** The hashed password of the user.
   -  **createdAt (DateTime):** The timestamp indicates when the user was created.
   -  **updatedAt (DateTime):** The timestamp indicates when the user was last updated.

### **2. Pet Model:**

-  **Fields:**
   -  **id (String):** A distinctive identifier for each pet.
   -  **name (String):** The name of the pet.
   -  **species (String):** The species of the pet (e.g., dog, cat).
   -  **breed (String):** The breed of the pet.
   -  **age (Integer):** The age of the pet.
   -  **size (String):** The size of the pet (e.g., small, medium, large).
   -  **location (String):** The location where the pet is currently located.
   -  **description (String):** A description of the pet.
   -  **temperament (String):** The temperament of the pet.
   -  **medicalHistory (String):** The medical history of the pet.
   -  **adoptionRequirements (String):** The requirements for adopting the pet.
   -  **createdAt (DateTime):** The timestamp indicates when the pet was created.
   -  **updatedAt (DateTime):** The timestamp indicates when the pet was last updated.

### **3. Adoption Request Model:**

-  **Fields:**
   -  **id (String):** A distinctive identifier for each adoption request.
   -  **userId (String):** A reference to the user who applied.
   -  **petId (String):** A reference to the pet for which the request is submitted.
   -  **status (String):** The status of the adoption request (e.g., PENDING, APPROVED, REJECTED). Default PENDING.
   -  **petOwnershipExperience (String):** The experience of the requester with pet ownership.
   -  **createdAt (DateTime):** The timestamp indicates when the request was created.
   -  **updatedAt (DateTime):** The timestamp indicates when the request was last updated.

### Relational Description

1. **User Model:**

   -  One-to-Many relationship with Adoption Request (each user can make multiple adoption requests).

2. **Pet Model:**

   -  One-to-Many relationship with Adoption Request (each pet can have multiple adoption requests).

3. **Adoption Request Model:**
   -  Many-to-One relationship with User (each adoption request belongs to one user).
   -  Many-to-One relationship with Pet (each adoption request is for one pet).

## **Error Handling:**

Implement proper error handling throughout the application. Use global error handling middleware to catch and handle errors, providing appropriate error responses with status codes and error messages.

### **Sample Error Response:**

-  For Validation Error (Zod):

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

-  For General or Generic Errors

```json
{
    "success": false,
    "message": "Something Went Wrong",
    "errorDetails": error
}
```

-  Unauthorized Error Response

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

**`POST /api/register:` The request method like GET, PUT, PATCH, DELETE, POST should not be included in the route path. Follow the pattern as shown in the example for every endpoint: `"/api/register"`**

### **1. User Registration**

-  **Endpoint:** **`POST /api/register`**
-  **Request Body:**

```json
{
   "name": "John Doe",
   "email": "john@example.com",
   "password": "password"
}
```

-  **Response** (Response should not include the password):

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

-  **Endpoint:** **`POST /api/login`**
-  **Request Body:**

```json
{
   "email": "john@example.com",
   "password": "password"
}
```

-  **Response:**

```json
{
   "success": true,
   "statusCode": 200,
   "message": "User logged in successfully",
   "data": {
      "id": "b9964127-2924-42bb-9970-60f93c016bvf",
      "name": "John Doe",
      "email": "john@example.com",
      "token": "<JWT token>"
   }
}
```

### **3. Add a Pet**

-  **Endpoint:** **`POST /api/pets`**
-  **Request Headers:**
   -  `Authorization: <JWT_TOKEN>`
-  **Request Body:**

```json
{
   "name": "Buddy",
   "species": "dog",
   "breed": "Labrador Retriever",
   "age": 3,
   "size": "Large",
   "location": "Shelter XYZ",
   "description": "Buddy is a friendly and energetic Labrador Retriever. He loves playing fetch and going for long walks.",
   "temperament": "Friendly, playful",
   "medicalHistory": "Up to date on vaccinations, neutered.",
   "adoptionRequirements": "Buddy needs a home with a fenced yard and an active family."
}
```

-  **Response:**

```json
{
   "success": true,
   "statusCode": 201,
   "message": "Pet added successfully",
   "data": {
      "id": "b9964127-2924-42bb-9970-60f93c016ghs",
      "name": "Buddy",
      "species": "Dog",
      "breed": "Labrador Retriever",
      "age": 3,
      "size": "Large",
      "location": "Shelter XYZ",
      "description": "Buddy is a friendly and energetic Labrador Retriever. He loves playing fetch and going for long walks.",
      "temperament": "Friendly, playful",
      "medicalHistory": "Up to date on vaccinations, neutered.",
      "adoptionRequirements": "Buddy needs a home with a fenced yard and an active family.",
      "createdAt": "2024-03-24T12:00:00Z",
      "updatedAt": "2024-03-24T12:00:00Z"
   }
}
```

### **4. Get Paginated and Filtered Pets**

-  **Endpoint:** **`GET /api/pets`**

**Query Parameters for API Requests:**

When interacting with the API, you can utilize the following query parameters to customize and filter the results according to your preferences.

-  `species`: (Optional) Filter pets by species (e.g., dog, cat).
-  `breed`: (Optional) Filter pets by breed.
-  `age`: (Optional) Filter pets by age.
-  `size`: (Optional) Filter pets by size.
-  `location`: (Optional) Filter pets by location.
-  `searchTerm`: (Optional) Searches for pets based on a keyword or phrase. Only applicable to the following fields: `species`, `breed`, `location`, etc.
-  `page`: (Optional) Specifies the page number for paginated results. Default is 1. Example: ?page=2
-  `limit`: (Optional) Sets the number of data per page. Default is 10. Example: ?limit=5
-  `sortBy`: (Optional) Specifies the field by which the results should be sorted. Only applicable to the following fields: `species`, `breed`, `size`. Example: ?sortBy=cat
-  `sortOrder`: (Optional) Determines the sorting order, either 'asc' (ascending) or 'desc' (descending). Example: ?sortOrder=desc
-  **Response:**

```json
{
   "success": true,
   "statusCode": 200,
   "message": "Pets retrieved successfully",
   "meta": {
      // only for paginated result
      "page": 1,
      "limit": 10,
      "total": 20
   },
   "data": [
      {
         "id": "b9964127-2924-42bb-9970-60f93c016ghs",
         "name": "Buddy",
         "species": "Dog",
         "breed": "Labrador Retriever",
         "age": 3,
         "size": "Large",
         "location": "Shelter XYZ",
         "description": "Buddy is a friendly and energetic Labrador Retriever. He loves playing fetch and going for long walks.",
         "temperament": "Friendly, playful",
         "medicalHistory": "Up to date on vaccinations, neutered.",
         "adoptionRequirements": "Buddy needs a home with a fenced yard and an active family.",
         "createdAt": "2024-03-24T12:00:00Z",
         "updatedAt": "2024-03-24T12:00:00Z"
      }
      // More pets
   ]
}
```

### 5. Update Pet profile

-  **Endpoint:** **`PUT /api/pets/:petId`**
-  **Request Headers:**
   -  **`Authorization: <JWT_TOKEN>`**
-  **Request Body:**

```json
{
   "location": "Shelter ABC"
}
```

-  **Response:**

```json
{
   "success": true,
   "statusCode": 200,
   "message": "Pet profile updated successfully",
   "data": {
      "id": "b9964127-2924-42bb-9970-60f93c016ghs",
      "name": "Buddy",
      "species": "Dog",
      "breed": "Labrador Retriever",
      "age": 3,
      "size": "Large",
      "location": "Shelter ABC",
      "description": "Buddy is a friendly and energetic Labrador Retriever. He loves playing fetch and going for long walks.",
      "temperament": "Friendly, playful",
      "medicalHistory": "Up to date on vaccinations, neutered.",
      "adoptionRequirements": "Buddy needs a home with a fenced yard and an active family.",
      "createdAt": "2024-03-24T12:00:00Z",
      "updatedAt": "2024-03-24T12:05:00Z"
   }
}
```

This endpoint allows users with appropriate permissions to update the profile of a pet.

### 6. Submit Adoption Request

-  **Endpoint:** **`POST /api/adoption-request`**
-  **Request Headers:**
   -  `Authorization: <JWT_TOKEN>`
-  **Request Body:**

```json
{
   "petId": "b9964127-2924-42bb-9970-60f93c016ghs",
   "petOwnershipExperience": "Previous owner of a Labrador Retriever"
}
```

-  **Response:**

```json
{
   "success": true,
   "statusCode": 201,
   "message": "Adoption request submitted successfully",
   "data": {
      "id": "9b0dadf5-10fd-41d1-8355-80e67c85727c",
      "userId": "b9964127-2924-42bb-9970-60f93c016bvf",
      "petId": "b9964127-2924-42bb-9970-60f93c016ghs",
      "status": "PENDING",
      "petOwnershipExperience": "Previous owner of a Labrador Retriever",
      "createdAt": "2024-03-24T12:00:00Z",
      "updatedAt": "2024-03-24T12:00:00Z"
   }
}
```

### 7. Get Adoption Requests

-  **Endpoint:** **`GET /api/adoption-requests`**
-  **Request Headers:**
   -  `Authorization: <JWT_TOKEN>`
-  **Response:**

```json
{
   "success": true,
   "statusCode": 200,
   "message": "Adoption requests retrieved successfully",
   "data": [
      {
         "id": "9b0dadf5-10fd-41d1-8355-80e67c85727c",
         "userId": "b9964127-2924-42bb-9970-60f93c016bvf",
         "petId": "b9964127-2924-42bb-9970-60f93c016ghs",
         "status": "PENDING",
         "livingSituation": "House with fenced yard",
         "petOwnershipExperience": "Previous owner of a Labrador Retriever",
         "references": "Dr. Smith (veterinarian)",
         "createdAt": "2024-03-24T12:00:00Z",
         "updatedAt": "2024-03-24T12:00:00Z"
      }
      // More adoption requests
   ]
}
```

### 8. Update Adoption Request Status

-  **Endpoint:** **`PUT /api/adoption-requests/:requestId`**
-  **Request Headers:**
   -  `Authorization: <JWT_TOKEN>`
-  **Request Body:**

```json
{
   "status": "APPROVED"
}
```

-  **Response:**

```json
{
   "success": true,
   "statusCode": 200,
   "message": "Adoption request updated successfully",
   "data": {
      "id": "9b0dadf5-10fd-41d1-8355-80e67c85727c",
      "userId": "b9964127-2924-42bb-9970-60f93c016bvf",
      "petId": "b9964127-2924-42bb-9970-60f93c016ghs",
      "status": "APPROVED",
      "livingSituation": "House with fenced yard",
      "petOwnershipExperience": "Previous owner of a Labrador Retriever",
      "references": "Dr. Smith (veterinarian)",
      "createdAt": "2024-03-24T12:00:00Z",
      "updatedAt": "2024-03-24T12:00:00Z"
   }
}
```

### 9. Get User Information

-  **Endpoint:** **`GET /api/profile`**
-  **Request Headers:**
   -  `Authorization: <JWT_TOKEN>`
-  **Response:**

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

### 10. Update User Information

-  **Endpoint:** **`PUT /api/profile`**
-  **Request Headers:**
   -  `Authorization: <JWT_TOKEN>`
-  **Request Body:**

```json
{
   "name": "John Doe",
   "email": "john.doe@example.com"
}
```

-  **Response:**

```json
{
   "success": true,
   "statusCode": 200,
   "message": "User profile updated successfully",
   "data": {
      "id": "9b0dadf5-10fd-41d1-8355-80e67c85727c",
      "name": "John Doe",
      "email": "john.doe@example.com",
      "createdAt": "2024-03-24T12:00:00Z",
      "updatedAt": "2024-03-24T12:05:00Z"
   }
}
```

This endpoint allows users to update their profile information such as name and email.
