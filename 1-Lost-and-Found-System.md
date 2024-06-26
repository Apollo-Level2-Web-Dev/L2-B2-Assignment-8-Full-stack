# **Lost and Found System Assignment**

## **Assignment Requirements:**

### **Technology Stack:**

- **Programming Language:** TypeScript
- **Web Framework:** Express.js
- **Object Relational Mapping  (ORM):** Prisma for PostgreSQL
- **Authentication:** JWT (JSON Web Tokens)

## **Models:**

### **1. User Model:**

- **Fields:**
    - **id (String):** A distinctive identifier for each user.
    - **name (String):** The name of the user.
    - **email (String):** The email address of the user.
    - **password (String):** The hashed password of the user.
    - **createdAt (DateTime):** The timestamp indicating when the user was created.
    - **updatedAt (DateTime):** The timestamp indicating when the user was last updated.

### **2. FoundItemCategory Model:**

- **Fields:**
    - **id (String):** A distinctive identifier for each found item category.
    - **name (String):** The name of the found item category.
    - **createdAt (DateTime):** The timestamp indicating when the category was created.
    - **updatedAt (DateTime):** The timestamp indicating when the category was last updated.

### **3. FoundItem Model:**

- **Fields:**
    - **id (String):** A distinctive identifier for each found item.
    - **userId (String):** A reference to the user who found the item.
    - **categoryId (String):** A reference to the category of the found item.
    - **foundItemName (String):** Name of the found item, ex: **iPhone 15 pro**
    - **description (String):** A description of the found item.
    - **location (String):** The location where the item was found.
    - **createdAt (DateTime):** The timestamp indicating when the found item was reported.
    - **updatedAt (DateTime):** The timestamp indicating when the found item was last updated.

### **4. Claim Model:**

- **Fields:**
    - **id (String):** A distinctive identifier for each claim.
    - **userId (String):** A reference to the user who claims the found item.
    - **foundItemId (String):** A reference to the found item being claimed.
    - **status (String):** The status of the claim (e.g., PENDING, APPROVED, REJECTED) default will be **PENDING** while creating a claim.
    - **distinguishingFeatures (String):** Any distinguishing features provided by the user regarding the claimed item, which can be used to identify the claimant as the owner.
    - **lostDate (DateTime):** The timestamp indicating when the item was lost.
    - **createdAt (DateTime):** The timestamp indicating when the claim was made.
    - **updatedAt (DateTime):** The timestamp indicating when the claim was last updated.

### **5. UserProfile Model:**

- **Fields:**
    - **id (String):** A distinctive identifier for each user profile.
    - **userId (String):** A reference to the user associated with the profile.
    - **bio (String):** A brief bio or description of the user.
    - **age (Integer):** Age of the user.
    - **createdAt (DateTime):** The timestamp indicating when the user profile was created.
    - **updatedAt (DateTime):** The timestamp indicating when the user profile was last updated.

### Relational Description 
1. **User Model:**
   - One-to-Many with FoundItem.
   - One-to-Many with Claim.
   - One-to-One with UserProfile.

2. **FoundItemCategory Model:**
   - Indirectly related to FoundItem.

3. **FoundItem Model:**
   - Belongs to one User.
   - Belongs to one FoundItemCategory.

4. **Claim Model:**
   - Belongs to one User.
   - Belongs to one FoundItem.

5. **UserProfile Model:**
   - Belongs to one User.

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
    "message": "error mesage",
    "errorDetails": unauthorized error
}
```

## **Endpoints:**

**N.B.** For now, no role is required, allowing anyone to perform any operation without restrictions.

**`POST /api/register:`  The request method like GET, PUT, PATCH, DELETE, POST should not be included in the route path. Follow the pattern as shown in the example for every endpoint: `"/api/register"`**

### **1. User Registration**
This endpoint handles user registration, creating both the user account and corresponding user profile simultaneously using a transactional approach.

- **Endpoint:** **`POST /api/register`**
- **Request Body:**

```json
{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password",
    "profile": {
        "bio": "Passionate about helping people find their lost items.",
        "age": 30
		}
 }
```

- **Response:** (Response should not include the password)

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
        "updatedAt": "2024-03-23T12:00:00Z",
        "profile": {
            "id": "c1982e1a-4f01-4b6b-a313-54f78cbb8e55",
            "userId": "b9964127-2924-42bb-9970-60f93c016bvf",
            "bio": "Passionate about helping people find their lost items.",
            "age": 30,
            "createdAt": "2024-03-23T12:00:00Z",
            "updatedAt": "2024-03-23T12:00:00Z"
        }
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

### **3. Create Found Item Category**

- **Endpoint:** **`POST /api/found-item-categories`**
- **Request Headers:**
    - **`Authorization: <JWT_TOKEN>`**
- **Request Body:**

```json
{
    "name": "Electronics"
}
```

- **Response:**

```json
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
Creates a Found Item using the user's details extracted from the authorization token.
- **Endpoint:** **`POST /api/found-items`**
- **Request Headers:**
    - `Authorization: <JWT_TOKEN>`
- **Request Body:**

```json
{
    "categoryId": "9deaf54e-3f4f-4b50-9902-6c272f73db4a",
    "foundItemName": "iPhone 12 Pro",
    "description": "Lost iPhone 12 Pro, some other information about the item",
    "location": "Rampura, Banssree"
}
```

- **Response:**

```json
{
    "success": true,
    "statusCode": 201,
    "message": "Found item reported successfully",
    "data": {
        "id": "b9964127-2924-42bb-9970-60f93c0162lm",
        "userId": "b9964127-2924-42bb-9970-60f93c016bvf", 
        "user": {
            "id": "b9964127-2924-42bb-9970-60f93c016bvf",
            "name": "John Doe",
            "email": "john@example.com",
            "createdAt": "2024-03-23T12:00:00Z",
            "updatedAt": "2024-03-23T12:00:00Z"
        },
        "categoryId": "9deaf54e-3f4f-4b50-9902-6c272f73db4a",
        "category": {
            "id": "9deaf54e-3f4f-4b50-9902-6c272f73db4a",
            "name": "Electronics",
            "createdAt": "2024-03-26T12:00:00Z",
            "updatedAt": "2024-03-26T12:00:00Z"
        },
        "foundItemName": "iPhone 12 Pro",
        "description": "Lost iPhone 12 Pro, some other information about the item",
        "location": "Rampura, Banssree",
        "createdAt": "2024-03-23T12:00:00Z",
        "updatedAt": "2024-03-23T12:00:00Z"
    }
}
```

### **5. Get Paginated and Filtered Found Items**

- **Endpoint:** **`GET /api/found-items`**

**Query Parameters for API Requests:**

When interacting with the API, you can utilize the following query parameters to customize and filter the results according to your preferences.

- searchTerm: (Optional) Searches for items based on a keyword or phrase. Only applicable to the following fields: `foundItemName`, `location`, `description`  (searching mode insensitive)
- page: (Optional) Specifies the page number for paginated results. Default is 1. Example: ?page=1
- limit: (Optional) Sets the number of items per page. Default is a predefined limit. Example: ?limit=10
- sortBy: (Optional) Specifies the field by which the results should be sorted. Only applicable to the following fields: `foundItemName`, `category`, `foundDate`. Example: ?sortBy=foundItemName
- sortOrder: (Optional) Determines the sorting order, either 'asc' (ascending) or 'desc' (descending). Example: ?sortOrder=desc
- foundItemName: (Optional) Filters results by the name of the found item. Example: ?foundItemName=iphone 15 pro
- **Response:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "Found items retrieved successfully",
    "meta": {
        "total": 20,
        "page": 1,
        "limit": 10
    },
    "data": [
        {
            "id": "9deaf54e-3f4f-4b50-9902-6c272f73db4a",
            "foundItemName": "iPhone 12 Pro",
            "description": "Lost iPhone 12 Pro, some other information about the item",
            "location": "Rampura, Banssree",
            "createdAt": "2024-03-23T12:00:00Z",
            "updatedAt": "2024-03-23T12:00:00Z",
            "user": {
                "id": "b9964127-2924-42bb-9970-60f93c016bvf",
                "name": "John Doe",
                "email": "john@example.com",
                "createdAt": "2024-03-23T12:00:00Z",
                "updatedAt": "2024-03-23T12:00:00Z"
            },
            "category": {
                "id": "9deaf54e-3f4f-4b50-9902-6c272f73db4a",
                "name": "Electronics",
                "createdAt": "2024-03-26T12:00:00Z",
                "updatedAt": "2024-03-26T12:00:00Z"
            }
        },
        {
            "id": "7b3ad54e-3f4f-4b50-9902-6c272f73db4b",
            "foundItemName": "Laptop",
            "description": "Found a laptop at the bus stop.",
            "location": "Dhanmondi, Dhaka",
            "createdAt": "2024-03-24T08:00:00Z",
            "updatedAt": "2024-03-24T08:00:00Z",
            "user": {
                "id": "b9964127-2924-42bb-9970-60f93c016bvf",
                "name": "John Doe",
                "email": "john@example.com",
                "createdAt": "2024-03-23T12:00:00Z",
                "updatedAt": "2024-03-23T12:00:00Z"
            },
            "category": {
                "id": "9deaf54e-3f4f-4b50-9902-6c272f73db4a",
                "name": "Electronics",
                "createdAt": "2024-03-26T12:00:00Z",
                "updatedAt": "2024-03-26T12:00:00Z"
            }
        },
        // More found items
    ]
}

```

### **6. Create a Claim**
Creates a Clain using the user's details extracted from the authorization token.

- **Endpoint:** **`POST /api/claims`**
- **Request Headers:**
    - `Authorization: <JWT_TOKEN>`
- **Request Body:**

```json
{
    "foundItemId": "9deaf54e-3f4f-4b50-9902-6c272f73db4a",
    "distinguishingFeatures": "My phone has a small scratch on the back cover.",
    "lostDate": "2024-03-25T10:00:00Z"
}
```

- **Response:**

```json
{
    "success": true,
    "statusCode": 201,
    "message": "Claim created successfully",
    "data": {
        "id": "a8c74a93-c76f-4f63-aa54-7dbcf584e437",
        "userId": "b9964127-2924-42bb-9970-60f93c016bvf",
        "foundItemId": "9deaf54e-3f4f-4b50-9902-6c272f73db4a",
        "distinguishingFeatures": "My phone has a small scratch on the back cover.",
        "lostDate": "2024-03-25T10:00:00Z",
        "status": "PENDING",
        "createdAt": "2024-03-26T12:05:00Z",
        "updatedAt": "2024-03-26T12:05:00Z"
    }
}
```

### **7. Get Claims**

- **Endpoint:** **`GET /api/claims`**
- **Request Headers:**
    - `Authorization: <JWT_TOKEN>`
- **Response:**

```json

{
    "success": true,
    "statusCode": 200,
    "message": "Claims retrieved successfully",
    "data": [
        {
            "id": "a8c74a93-c76f-4f63-aa54-7dbcf584e437",
            "userId": "b9964127-2924-42bb-9970-60f93c016bvf",
            "foundItemId": "9deaf54e-3f4f-4b50-9902-6c272f73db4a",
            "distinguishingFeatures": "My phone has a small scratch on the back cover.",
            "lostDate": "2024-03-25T10:00:00Z",
            "status": "PENDING",
            "createdAt": "2024-03-26T12:05:00Z",
            "updatedAt": "2024-03-26T12:05:00Z",
            "foundItem": {
                "id": "9deaf54e-3f4f-4b50-9902-6c272f73db4a",
                "userId": "b9964127-2924-42bb-9970-60f93c016bvf",
                "categoryId": "1b3d5083-4b41-47ff-8567-3829ee5cfb71",
                "foundItemName": "iPhone 12 Pro",
                "description": "Found iPhone 12 Pro in a black case.",
                "location": "123 Main Street, Cityville",
                "createdAt": "2024-03-25T08:00:00Z",
                "updatedAt": "2024-03-25T08:00:00Z",
                "user": {
                    "id": "b9964127-2924-42bb-9970-60f93c016bvf", 
                    "name": "John Doe",
                    "email": "john@example.com",
                    "createdAt": "2024-03-23T12:00:00Z",
                    "updatedAt": "2024-03-23T12:00:00Z"
                },
                "category": {
                    "id": "1b3d5083-4b41-47ff-8567-3829ee5cfb71",
                    "name": "Electronics",
                    "createdAt": "2024-03-24T09:00:00Z",
                    "updatedAt": "2024-03-24T09:00:00Z"
                }
            }
        },
        // More claims...
    ]
}
```

### **8. Update Claim Status**

- **Endpoint:** **`PUT /api/claims/:claimId`**
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
    "message": "Claim updated successfully",
    "data": {
        "id": "9b0dadf5-10fd-41d1-8355-80e67c85727c",
        "userId": "9b0dadf5-10fd-41d1-8355-80e67c85727t",
        "foundItemId": "9b0dadf5-10fd-41d1-8345-80e67c85727c",
        "distinguishingFeatures": "My phone has a dent in the back",
        "lostDate": "2024-03-23T12:00:00Z",
        "status": "APPROVED",
        "createdAt": "2024-03-23T12:05:00Z",
        "updatedAt": "2024-03-23T12:05:00Z"
    }
}
```

### **9. Get Profile**

- **Endpoint:** **`GET /api/my-profile`**
- **Request Headers:**
    - `Authorization: <JWT_TOKEN>`
- **Response:**

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
        "updatedAt": "2024-03-23T12:05:00Z",
        "user": {
            "id": "b9964127-2924-42bb-9970-60f93c016b3f",
            "name": "John Doe",
            "email": "john@example.com",
            "createdAt": "2024-03-23T12:00:00Z",
            "updatedAt": "2024-03-23T12:00:00Z"
        }
    }
}

```

### **10. Update My Profile**

- **Endpoint:** **`PUT /api/my-profile`**
- **Request Headers:**
    - `Authorization: <JWT_TOKEN>`
- **Request Body:**

```json

{
    "bio": "Updated bio text",
    "age": 35
}
```

- **Response:**

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
        "updatedAt": "2024-03-24T10:00:00Z",
        "user": {
            "id": "b9964127-2924-42bb-9970-60f93c016b3f",
            "name": "John Doe",
            "email": "john@example.com",
            "createdAt": "2024-03-23T12:00:00Z",
            "updatedAt": "2024-03-23T12:00:00Z"
        }
    }
}
```

This endpoint allows users to update their profile information such as bio and age.