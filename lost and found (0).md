# **Lost and Found System Assignment**

## **Assignment Requirements:**

### **Technology Stack:**

-  **Programming Language:** TypeScript
-  **Web Framework:** Express.js
-  **Object Data Modeling (ODM):** Prisma for PostgreSQL
-  **Authentication:** JWT (JSON Web Tokens)

## **Models:**

### **1. User Model:**

-  **Fields:**
   -  **id (Integer):** A distinctive identifier for each user.
   -  **name (String):** The name of the user.
   -  **email (String):** The email address of the user.
   -  **password (String):** The hashed password of the user.
   -  **createdAt (Date):** The timestamp indicating when the user was created.
   -  **updatedAt (Date):** The timestamp indicating when the user was last updated.

### **2. FoundItemCategory Model:**

-  **Fields:**
   -  **id (Integer):** A distinctive identifier for each found item category.
   -  **name (String):** The name of the found item category.
   -  **createdAt (Date):** The timestamp indicating when the category was created.
   -  **updatedAt (Date):** The timestamp indicating when the category was last updated.

### **3. FoundItem Model:**

-  **Fields:**
   -  **id (Integer):** A distinctive identifier for each found item.
   -  **userId (Integer):** A reference to the user who found the item.
   -  **categoryId (Integer):** A reference to the category of the found item.
   -  foundItemName (String): Name of the found item, ex: **iPhone 15 pro**
   -  **description (String):** A description of the found item.
   -  **location (String):** The location where the item was found.
   -  **createdAt (Date):** The timestamp indicating when the found item was reported.
   -  **updatedAt (Date):** The timestamp indicating when the found item was last updated.

### **4. Claim Model:**

-  **Fields:**
   -  **id (Integer):** A distinctive identifier for each claim.
   -  **userId (Integer):** A reference to the user who claims the found item.
   -  **foundItemId (Integer):** A reference to the found item being claimed.
   -  **status (String):** The status of the claim (e.g., pending, approved, rejected) default will be “**pending”** while creating a claim.
   -  distinguishingFeatures (String): Any distinguishing features provided by the user regarding the claimed item, which can be used to identify the claimant as the owner.
   -  lostDate (Date): The timestamp indicating when the item was lost.
   -  **createdAt (Date):** The timestamp indicating when the claim was made.
   -  **updatedAt (Date):** The timestamp indicating when the claim was last updated.

### **5. UserProfile Model:**

-  **Fields:**
   -  **id (Integer):** A distinctive identifier for each user profile.
   -  **userId (Integer):** A reference to the user associated with the profile.
   -  **bio (String):** A brief bio or description of the user.
   -  **age (Integer):** Age of the user.
   -  **createdAt (Date):** The timestamp indicating when the user profile was created.
   -  **updatedAt (Date):** The timestamp indicating when the user profile was last updated.

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
    "message": "error mesage",
    "errorDetails": error
}

```

## **Endpoints:**

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

-  **Response:**
-  Response should not include the password

```json
{
   "success": true,
   "statusCode": 201,
   "message": "User registered successfully",
   "data": {
      "id": "b9964127-2924-42bb-9970-60f93c016bvf",
      "name": "John Doe",
      "email": "john@example.com",
      "createdAt": "2024-03-23T12:00:00Z",
      "updatedAt": "2024-03-23T12:00:00Z"
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

### **3. Create Found Item Category**

-  **Endpoint:** **`POST /api/found-item-categories`**
-  **Request Headers:**
   -  **`Authorization: <JWT_TOKEN>`**
-  **Request Body:**

```json
jsonCopy code
{
    "name": "Electronics"
}

```

-  **Response:**

```json
jsonCopy code
{
    "success": true,
    "statusCode": 201,
    "message": "Found item category created successfully",
    "data": {
        "id": "9deaf54e-3f4f-4b50-9902-6c272f73db4a",
        "name": "Electronics",
        "createdAt": "2024-03-26T12:00:00Z",
        "updatedAt": "2024-03-26T12:00:00Z"
    }
}

```

This endpoint allows authenticated users to create a new category for found items.

### **4. Report a Found Item**

-  **Endpoint:** **`POST /api/found-items`**
-  **Request Headers:**
   -  `Authorization: <JWT_TOKEN>`
-  **Request Body:**

```json
{
   "userId": "9deaf54e-3f4f-4b50-9902-6c272f73db9a",
   "categoryId": "165d0533-873d-426a-88b0-a8dd320a2854",
   "foundItemName": "iphone 12 pro",
   "description": "Lost iPhone 12 Pro, some other information about the item",
   "location": "Rampura, banssree"
}
```

-  **Response:**

```json
{
    "success": true,
    "statusCode": 201,
    "message": "Found item reported successfully",
    "data": {
        "id": 789,
        "userId": 123,
        "categoryId": 456,
        "foundItemName": "iphone 12 pro",
        "description": "Lost iPhone 12 Pro, some other information about the item",
        "location": "Rampura, banssree"
        "createdAt": "2024-03-23T12:00:00Z",
        "updatedAt": "2024-03-23T12:00:00Z"
    }
}

```

### **5. Get Paginated and Filtered Found Items**

-  **Endpoint:** **`GET /api/found-items`**

**Query Parameters for API Requests:**

When interacting with the API, you can utilize the following query parameters to customize and filter the results according to your preferences.

-  searchTerm: (Optional) Searches for items based on a keyword or phrase. Only applicable to the following fields: `foundItemName`, `location`, `description`
-  page: (Optional) Specifies the page number for paginated results. Default is 1. Example: ?page=2
-  limit: (Optional) Sets the number of items per page. Default is a predefined limit. Example: ?limit=10
-  sortBy: (Optional) Specifies the field by which the results should be sorted. Only applicable to the following fields: `foundItemName`, `category`, `foundDate`. Example: ?sortBy=foundItemName
-  sortOrder: (Optional) Determines the sorting order, either 'asc' (ascending) or 'desc' (descending). Example: ?sortOrder=desc
-  foundItemName: (Optional) Filters results by the name of the found item. Example: ?foundItemName=iphone15pro
-  **Response:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "Found items retrieved successfully",
    "data": [
        {
            "id": 789,
            "userId": 123,
            "categoryId": 456,
            "foundItemName": "iphone 12 pro",
            "description": "Lost iPhone 12 Pro, some other information about the item",
            "location": "Rampura, banssree"
            "createdAt": "2024-03-23T12:00:00Z",
            "updatedAt": "2024-03-23T12:00:00Z"
        },
        // More found items
    ]
}

```

### **6. Create a Claim**

-  **Endpoint:** **`POST /api/claims`**
-  **Request Headers:**
   -  `Authorization: <JWT_TOKEN>`
-  **Request Body:**

```json
{
   "userId": "9b0dadf5-10fd-41d1-8355-80e67c85727c",
   "foundItemId": "b9964127-2924-42bb-9970-60f93c016bff",
   "distinguishingFeatures": "My phone has a dent in the back",
   "lostDate": "2024-03-23T12:00:00Z"
}
```

-  **Response:**

```json
jsonCopy code
{
    "success": true,
    "statusCode": 201,
    "message": "Claim created successfully",
    "data": {
        "id": "9b0dadf5-10fd-41d1-8355-80e67c85727c",
        "userId": "9b0dadf5-10fd-41d1-8355-80e67c85727t",
        "foundItemId": "9b0dadf5-10fd-41d1-8345-80e67c85727c",
        "distinguishingFeatures": "My phone has a dent in the back",
        "lostDate": "2024-03-23T12:00:00Z",
        "status": "pending",
        "createdAt": "2024-03-23T12:05:00Z",
        "updatedAt": "2024-03-23T12:05:00Z"
    }
}

```

### **7. Get Claims**

-  **Endpoint:** **`GET /api/claims`**
-  **Request Headers:**
   -  `Authorization: <JWT_TOKEN>`
-  **Response:**

```json
jsonCopy code
{
    "success": true,
    "statusCode": 200,
    "message": "Claims retrieved successfully",
    "data": [
        {
            "id": "9b0dadf5-10fd-41d1-8355-80e67c85727c",
            "userId": "9b0dadf5-10fd-41d1-8355-80e67c85727t",
            "foundItemId": "9b0dadf5-10fd-41d1-8345-80e67c85727c",
            "distinguishingFeatures": "My phone has a dent in the back",
            "lostDate": "2024-03-23T12:00:00Z",
            "status": "pending",
            "createdAt": "2024-03-23T12:05:00Z",
            "updatedAt": "2024-03-23T12:05:00Z"
        },
        // More claims
    ]
}

```

### **8. Update Claim Status**

-  **Endpoint:** **`PUT /api/claims/:claimId`**
-  Only the the person who post the found item can update the claim status for that item
-  **Request Headers:**
   -  `Authorization: <JWT_TOKEN>`
-  **Request Body:**

```json
{
   "status": "approved"
}
```

-  **Response:**

```json
jsonCopy code
{
    "success": true,
    "statusCode": 200,
    "message": "Claim updated successfully",
    "data": {
            "id": "9b0dadf5-10fd-41d1-8355-80e67c85727c",
            "userId": "9b0dadf5-10fd-41d1-8355-80e67c85727t",
            "foundItemId": "9b0dadf5-10fd-41d1-8345-80e67c85727c",
            "distinguishingFeatures": "My phone has a dent in the back",
            "lostDate": "2024-03-23T12:00:00Z",
            "status": "approved",
            "createdAt": "2024-03-23T12:05:00Z",
            "updatedAt": "2024-03-23T12:05:00Z"
        },
}

```

### **9. Get Profile**

-  **Endpoint:** **`GET /api/my-profile`**
-  **Request Headers:**
   -  `Authorization: <JWT_TOKEN>`
-  **Response:**

```json
{
   "success": true,
   "statusCode": 200,
   "message": "Profile retrieved successfully",
   "data": {
      "id": "b9964127-2924-42bb-9970-60f93c016bfr",
      "userId": "b9964127-2924-42bb-9970-60f93c016b3f",
      "bio": "Passionate about helping people find their lost items.",
      "age": 30,
      "createdAt": "2024-03-23T12:00:00Z",
      "updatedAt": "2024-03-23T12:05:00Z"
   }
}
```

### **10. Update My Profile**

-  **Endpoint:** **`PUT /api/my-profile`**
-  **Request Headers:**
   -  `Authorization: <JWT_TOKEN>`
-  **Request Body:**

```json
{
   "bio": "Updated bio text",
   "age": 35
}
```

-  **Response:**

```json
{
   "success": true,
   "statusCode": 200,
   "message": "User profile updated successfully",
   "data": {
      "id": "b9964127-2924-42bb-9970-60f93c016bfr",
      "userId": "b9964127-2924-42bb-9970-60f93c016b3f",
      "bio": "Updated bio text",
      "age": 35,
      "createdAt": "2024-03-23T12:00:00Z",
      "updatedAt": "2024-03-24T10:00:00Z"
   }
}
```

This endpoint allows users to update their profile information such as bio and age.
