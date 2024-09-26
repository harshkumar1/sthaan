### API Summary

1. **`GET /api/v1/user-id/check-availability`**: Check if the User ID is available.
2. **`POST /api/v1/user-profile/create`**: Create a new user profile with contact info and delivery preferences.
3. **`POST /api/v1/user-profile/validate-contact`**: Send an OTP for contact info validation.
4. **`POST /api/v1/user-profile/verify-otp`**: Verify the OTP for user validation.
5. **`POST /api/v1/sthaan-id/create`**: Create a unique Sthaan ID based on the user's address.
6. **`POST /api/v1/user-profile/associate-sthaan-id`**: Associate the created Sthaan ID with the User ID.

__________________________________


### 1. **Check User ID Availability**

**Description**: This API checks whether a user-provided User ID can be claimed.

- **Method**: `GET`
- **Endpoint**: `/api/v1/user-id/check-availability`
- **Request**:
  ```json
  {
    "user_id": "sita@lsp_provider"
  }
  ```
- **Response**:
  - **200 OK** (User ID Available):
    ```json
    {
      "available": true,
      "message": "User ID is available."
    }
    ```
  - **409 Conflict** (User ID Already Taken):
    ```json
    {
      "available": false,
      "message": "User ID is already taken."
    }
    ```

---

### 2. **Create User Profile**

**Description**: This API handles the initial profile creation for a user, capturing contact details, delivery preferences and delivery preferenceswith the User ID.

- **Method**: `POST`
- **Endpoint**: `/api/v1/user-profile/create`
- **Request**:
  ```json
  {
    "user_id": "sita@lsp_provider",
    "contact_info": {
      "phone_number": "+911234567890",
      "email": "sita@example.com"
    },
    "delivery_preferences": {
      "availability": "weekdays",
      "instructions": "Leave at the door"
    }
  }
  ```
- **Response**:
  - **201 Created**:
    ```json
    {
      "message": "User profile created successfully",
      "user_id": "sita@lsp_provider"
    }
    ```
  - **400 Bad Request** (Invalid data):
    ```json
    {
      "error": "Invalid contact information."
    }
    ```

---

### 3. **Validate Contact Info (OTP)**

**Description**: This API initiates an OTP to be sent to the user for phone/email validation.

- **Method**: `POST`
- **Endpoint**: `/api/v1/user-profile/validate-contact`
- **Request**:
  ```json
  {
    "user_id": "sita@lsp_provider",
    "phone_number": "+911234567890"
  }
  ```
- **Response**:
  - **200 OK**:
    ```json
    {
      "message": "OTP sent to the provided phone number."
    }
    ```
  - **400 Bad Request** (Phone number/email invalid):
    ```json
    {
      "error": "Invalid phone number."
    }
    ```

---

### 4. **Verify OTP**

**Description**: This API verifies the OTP entered by the user.

- **Method**: `POST`
- **Endpoint**: `/api/v1/user-profile/verify-otp`
- **Request**:
  ```json
  {
    "user_id": "sita@lsp_provider",
    "otp": "123456"
  }
  ```
- **Response**:
  - **200 OK**:
    ```json
    {
      "message": "OTP verified successfully.",
      "user_id": "sita@lsp_provider"
    }
    ```
  - **401 Unauthorized** (Invalid OTP):
    ```json
    {
      "error": "Invalid OTP."
    }
    ```

---

### 5. **Create Sthaan ID**

**Description**: This API creates the unique **Sthaan ID** based on the provided address and links it to the User ID.

- **Method**: `POST`
- **Endpoint**: `/api/v1/sthaan-id/create`
- **Request**:
  ```json
  {
    "user_id": "sita@lsp_provider",
    "address_info": {
      "address_line1": "1234 India Gate",
      "city": "New Delhi",
      "state": "Delhi",
      "postal_code": "110001",
      "country": "India"
    },
    "address_metadata": {
      "landmark": "Near National Museum",
      "geocode": "28.6129, 77.2295"
    }
  }
  ```
- **Response**:
  - **201 Created**:
    ```json
    {
      "message": "Sthaan ID created successfully.",
      "sthaan_id": "indiagate@lsp_provider"
    }
    ```
  - **400 Bad Request** (Invalid address data):
    ```json
    {
      "error": "Invalid address data provided."
    }
    ```

---

### 6. **Associate Sthaan ID with User ID**

**Description**: This API links the generated Sthaan ID to the User ID for future address retrieval.

- **Method**: `POST`
- **Endpoint**: `/api/v1/user-profile/associate-sthaan-id`
- **Request**:
  ```json
  {
    "user_id": "sita@lsp_provider",
    "sthaan_id": "indiagate@lsp_provider"
  }
  ```
- **Response**:
  - **200 OK**:
    ```json
    {
      "message": "Sthaan ID associated with User ID successfully.",
      "user_id": "sita@lsp_provider",
      "sthaan_id": "indiagate@lsp_provider"
    }
    ```

---
