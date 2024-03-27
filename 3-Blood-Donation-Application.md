## Blood Donation Application

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
    - **bloodType (String):** The type of blood a user has.
    - **location (String):** The location of the user.
    - **availability(Boolean):** The status will be false by default.
    - **createdAt (DateTime):** The timestamp indicating when the user was created.
    - **updatedAt (DateTime):** The timestamp indicating when the user was last updated.
    

### **2. Request Model:**

- **Fields:**
    - **id (String):** A distinctive identifier for each request.
    - **donorId(String):** Id of a user. The user id will be that of a donor. The user you want to request for blood.
    - **requesterId(String):** Id of a user. The user id will be that of a requester. The user who will send requests.
    - **phoneNumber (String):** The phone number of the requester.
    - **dateOfDonation (Date):** The date of donation
    - **hospitalName(String):** The name of the hospital.
    - **hospitalAddress (String):** The address of the hospital.
    - **reason(String)**: The reason for donation.
    - **requestStatus (String):** The status will be PENDING by default. **Use enum: `PENDING`, `APPROVED`, `REJECTED`.**
    - **createdAt (DateTime):** The timestamp indicating when the found item was reported.
    - **updatedAt (DateTime):** The timestamp indicating when the found item was last updated.

### **3. UserProfile Model:**

- **Fields:**
    - **id (String):** A distinctive identifier for each user profile.
    - **userId (String):** A reference to the user associated with the profile.
    - **bio (String):** A brief bio or description of the user.
    - **age (Integer):** The age of the user.
    - **lastDonationDate (Date):** The last date of donation
    - **createdAt (DateTime):** The timestamp indicating when the user profile was created.
    - **updatedAt (DateTime):** The timestamp indicating when the user profile was last updated.

## Relations Description:

1. **Request Model - Donor (User) Relation:**
    - Each request is associated with one donor user.
    - A donor can have multiple associated requests.
2. **Request Model - Requester (User) Relation:**
    - Each request is associated with one requester user.
    - A requester can have multiple associated requests.
3. **UserProfile Model - User Relation:**
    - Each user profile corresponds to one user.
    - There is a one-to-one relationship between a user and their profile.

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

This error may occur under the following circumstances:

- **JWT Expiry:** The provided JWT (JSON Web Token) has expired.
- **Invalid JWT:** The JWT provided is invalid or malformed.
- **Undefined JWT:** No JWT is provided in the request headers.
- **Not Authorized User:** The user does not possess the required permissions for the requested action or resource.
- **Access Denied:** The user is attempting to access a resource without the necessary authorization.

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
  "email": "johndoe@example.com",
  "password": "securepassword",
  "bloodType": "A+",
  "location": "New York",
  "age": 30,
  "bio": "A regular blood donor",
  "lastDonationDate": "2024-03-25"
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
        "bloodType": "A+",
        "location": "Chittagong",
        "availability": false,
        "createdAt": "2024-03-24T12:00:00Z",
        "updatedAt": "2024-03-24T12:00:00Z",
        "userProfile": {
            "id": "b9964127-2924-42bb-9970-60f93c016gkp",
            "userId": "b9964127-2924-42bb-9970-60f93c016bvf",
            "bio": "A regular blood donor",
            "age": 30,
            "lastDonationDate": "2024-03-25",
            "createdAt": "2024-03-24T12:00:00Z",
            "updatedAt": "2024-03-24T12:00:00Z"
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

### **3. Get Paginated and Filtered Found Items**

- **Endpoint:** **`GET /api/donor-list`**

**Query Parameters for API Requests:**

When interacting with the API, you can utilize the following query parameters to customize and filter the results according to your preferences.

- searchTerm: (Optional) Searches for items based on a keyword or phrase. Only applicable to the following fields: `bloodType`, `location`, `name`, `email`
- page: (Optional) Specifies the page number for paginated results. Default is 1. Example: ?page=1
- limit: (Optional) Sets the number of items per page. Default is a predefined limit. Example: ?limit=10
- sortBy: (Optional) Specifies the field by which the results should be sorted. Only applicable to the following fields: `lastDonationDate`, `age`, `name`. Example: ?sortBy=lastDonationDate
- sortOrder: (Optional) Determines the sorting order, either 'asc' (ascending) or 'desc' (descending). Example: ?sortOrder=desc
- bloodType: (Optional) Filters results by the blood type of the user you want to request. Example: ?bloodType=A+
- availability:(Optional) Filters results by the availability of the user you want to request. Example: ?availability=true

- **Response:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "Donors successfully found",
    "meta": {
        "total": 20,
        "page": 1,
        "limit": 10
    },
    "data": [
        {
            "id": "b9964127-2924-42bb-9970-60f93c016ghi",
            "name": "John Doe",
            "email": "john@example.com",
            "bloodType": "A+",
            "location": "Chittagong",
            "availability": true,
            "createdAt": "2024-03-24T12:00:00Z",
            "updatedAt": "2024-03-24T12:00:00Z",
            "userProfile": {
                "id": "b9964127-2924-42bb-9970-60f93c016gkp",
                "userId": "b9964127-2924-42bb-9970-60f93c016ghi",
                "bio": "I have donated 5 times.",
                "age": 30,
                "lastDonationDate": "2024-03-21T12:00:00Z",
                "createdAt": "2024-03-22T12:00:00Z",
                "updatedAt": "2024-03-22T12:00:00Z"
            }
        },
        // More users who will donate
    ]
}

```

### **4. Request A Donor (user) For Blood**

Creates a blood donation request using the requester's details extracted from the `authorization token.`

- **Endpoint:** **`POST /api/donation-request`**
- **Request Headers:**
    - `Authorization: <JWT_TOKEN>`
- **Request Body:**

```json
{
    "donorId": "b9964127-2924-42bb-9970-60f93c016bvj",
    "name": "Harry Potter",
    "blood_type": "A+",
    "phone_number": "012345678591",
    "date": "2024-03-26",
    "hospital_name": "Chevron",
    "hospital_address": "Panchlaish",
    "reason": "Anemia",
}
```

- **Response:**

```json
{
    "success": true,
    "statusCode": 201,
    "message": "Request successfully made",
    "data": {
        "id": "b9964127-2924-42bb-9970-60f93c016ghi",
        "donorId": "b9964127-2924-42bb-9970-60f93c016bvj",
        "name": "Harry Potter",
        "blood_type": "A+",
        "phone_number": "012345678591",
        "date": "2024-03-26",
        "hospital_name": "Chevron",
        "hospital_address": "Panchlaish",
        "reason": "Anemia",
        "request_status": "PENDING",
        "createdAt": "2024-03-24T12:00:00Z",
        "updatedAt": "2024-03-24T12:00:00Z",
        "donor": {
            "id": "b9964127-2924-42bb-9970-60f93c016bvj",
            "name": "John Doe",
            "email": "john@gmail.com",
            "bloodType": "A+",
            "location": "Chittagong",
            "availability": true,
            "createdAt": "2024-03-24T12:00:00Z",
            "updatedAt": "2024-03-24T12:00:00Z",
            "userProfile": {
                "id": "b9964127-2924-42bb-9970-60f93c016gkp",
                "userId": "b9964127-2924-42bb-9970-60f93c016bvj",
                "bio": "I have donated 5 times.",
                "age": 30,
                "lastDonationDate": "2024-03-21T12:00:00Z",
                "createdAt": "2024-03-22T12:00:00Z",
                "updatedAt": "2024-03-22T12:00:00Z"
            }
        }
    }
}

```

This endpoint allows users with appropriate permissions to view the profile of a requester/donor.

### **5. Get My Donation Request as Donor (user)**

Retrieves donation requests directed to the authenticated donor based on the provided JWT token.

- **Endpoint:** **`GET /api/donation-request`**
- **Request Headers:**
    - **`Authorization: <JWT_TOKEN>`**
- **Response:**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "Donation requests retrieved successfully",
    "data": [
        {
            "id": "b9964127-2924-42bb-9970-60f93c016ghi",
            "donorId": "b9964127-2924-42bb-9970-60f93c016bvj",
            "requesterId": "b9964127-2924-42bb-9970-60f93c016xyz",
            "phoneNumber": "012345678591",
            "dateOfDonation": "2024-03-26",
            "hospitalName": "Chevron",
            "hospitalAddress": "Panchlaish",
            "reason": "Anemia",
            "requestStatus": "PENDING",
            "createdAt": "2024-03-24T12:00:00Z",
            "updatedAt": "2024-03-24T12:00:00Z",
            "requester": {
                "id": "b9964127-2924-42bb-9970-60f93c016xyz",
                "name": "Jane Doe",
                "email": "jane@example.com",
                "location":"Chittagong",
                "blood_type":"A+",
                "availability":true
            }
        },
        {
            "id": "b9964127-2924-42bb-9970-60f93c016gkj",
            "donorId": "b9964127-2924-42bb-9970-60f93c016bvj",
            "requesterId": "b9964127-2924-42bb-9970-60f93c016abc",
            "phoneNumber": "012345678592",
            "dateOfDonation": "2024-03-28",
            "hospitalName": "St. Mungo's Hospital",
            "hospitalAddress": "Diagon Alley",
            "reason": "Emergency surgery",
            "requestStatus": "PENDING",
            "createdAt": "2024-03-25T09:00:00Z",
            "updatedAt": "2024-03-25T09:00:00Z",
            "requester": {
                "id": "b9964127-2924-42bb-9970-60f93c016abc",
                "name": "John Smith",
                "email": "john@example.com",
                "location":"Chittagong",
                "blood_type":"A+",
                "availability":true
                
                
            }
        }
    ]
}
```

### **6. Update Request Application Status**

 **Endpoint:** **`PUT /api/donation-request/:requestId`**

This endpoint allows a donor to update the status of their donation request.

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
    "message": "Donation request status successfully updated",
    "data": {
        "id": "b9964127-2924-42bb-9970-60f93c016ghi",
        "donorId": "b9964127-2924-42bb-9970-60f93c016bvj",
        "requesterId": "b9964127-2924-42bb-9970-60f93c016xyz",
        "phoneNumber": "012345678591",
        "dateOfDonation": "2024-03-26",
        "hospitalName": "Chevron",
        "hospitalAddress": "Panchlaish",
        "reason": "Anemia",
        "requestStatus": "APPROVED",
        "createdAt": "2024-03-24T12:00:00Z",
        "updatedAt": "2024-03-26T10:00:00Z"
    }
}
```

### **7. Get My Profile**

This endpoint retrieves the profile of the authenticated user based on the provided JWT token.

- **Endpoint:** **`GET /api/my-profile`**
- **Request Headers:**
    - **`Authorization: <JWT_TOKEN>`**
- **Response (Donor):**

```json
{
    "success": true,
    "statusCode": 200,
    "message": "Profile retrieved successfully",
    "data": {
        "id": "b9964127-2924-42bb-9970-60f93c016bfr",
        "name": "John Doe",
        "email": "john@example.com",
        "bloodType": "A+",
        "location": "Chittagong",
        "bio": "I have donated 5 times.",
        "age": 30,
        "lastDonationDate": "2023-12-15",
        "createdAt": "2024-03-23T12:00:00Z",
        "updatedAt": "2024-03-24T10:00:00Z",
        "userProfile": {
            "id": "b9964127-2924-42bb-9970-60f93c016bfr",
            "userId": "b9964127-2924-42bb-9970-60f93c016bfr",
            "bio": "I have donated 5 times. I'm always ready to help others in need.",
            "age": 30,
            "lastDonationDate": "2023-12-15",
            "createdAt": "2024-03-23T12:00:00Z",
            "updatedAt": "2024-03-24T10:00:00Z"
        }
    }
}
```

### **8. Update My Profile**

This endpoint allows the authenticated user to update their profile information using the provided JWT token for authorization.

- **Endpoint:** **`PUT /api/my-profile`**
- **Request Headers:**
    - `Authorization: <JWT_TOKEN>`
- **Request Body:**

```json

{
    "bio": "Updated bio text. I have donated 5 times.",
    "age": 35,
    "location": "Chittagong",
    "lastDonationDate": "2023-12-15"
}
```

- **Response :**

```json

{
    "success": true,
    "statusCode": 200,
    "message": "User profile updated successfully",
    "data": {
        "id": "b9964127-2924-42bb-9970-60f93c016bfr",
        "name": "John Doe",
        "email": "john@example.com",
        "bloodType": "A+",
        "location": "Chittagong",
        "bio": "Updated bio text. I have donated 5 times.",
        "age": 35,
        "lastDonationDate": "2023-12-15",
        "createdAt": "2024-03-23T12:00:00Z",
        "updatedAt": "2024-03-24T10:00:00Z"
    }
}
```

This endpoint allows users to update their profile information