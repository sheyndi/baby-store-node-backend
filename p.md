# 🌍 Focus - Backend Server

**Focus** is a backend server for a tourism information and recommendation platform, helping travelers find trustworthy accommodation deals. Built with **Node.js**, **Express**, **TypeScript**, and **MySQL**.

---

## 🚀 Technologies Used

- Node.js  
- Express  
- TypeScript  
- MySQL  
- dotenv  
- JWT  
- REST API  

---

## ⚙️ Installation & Running

```bash
# Install dependencies
npm install

# Create environment file
cp .env.example .env
```

### `.env` Example:

```
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=focus_db

PORT=5000
JWT_SECRET=your_jwt_secret
```

```bash
# Run development server
npm run dev
```

---

## 📁 Folder Structure

```
src/
├── routes/         # Routes
├── controllers/    # Request logic
├── models/         # Database queries
├── middleware/     # Authorization, error handling
├── config/         # DB connection
├── types/          # TypeScript types
└── index.ts        # Entry point
```

---

## 🔒 Authorization

- Some endpoints require a JWT token.
- Add this header:  
  `Authorization: Bearer <your_token>`

---

## 🧪 Testing

- Recommended tools:
  - Postman
  - Thunder Client
  - curl

---

## 📡 REST API Documentation

### 👤 Users

| Method | Endpoint                   | Params | Headers        | Body | Description                             | Response | Errors |
|--------|----------------------------|--------|----------------|------|-----------------------------------------|----------|--------|
| GET    | `/api/user/getUserDetails` | –      | `userId`       | –    | Get user details by ID                  | `{ message, data: user }` | 400, 404, 500 |
| POST   | `/api/user/addOrUpdateUser`| –      | `userId`       | `{ first_name, last_name, ... }` | Add or update user by ID | `{ message, userId? }` | 400, 500 |
| PATCH  | `/api/user/favorites`      | –      | `userId`       | `{ hotelId, action: "add" | "remove" }` | Update favorite hotels list | `{ message, updatedList }` | 400, 404, 500 |
| DELETE | `/api/user/:userId`        | URL ID | `userId`       | –    | Delete user by ID                       | `{ message }` | 400, 404, 430, 500 |

---

## 🔐 Authentication

| Method | Endpoint                 | Params         | Body                          | Description                 | Response | Errors |
|--------|--------------------------|----------------|-------------------------------|-----------------------------|----------|--------|
| GET    | `/api/auth/getOTP/:email`| `email`        | –                             | Send OTP to email           | `{ message }` | 400, 500 |
| POST   | `/api/auth/verifyOTP`    | –              | `{ email, otp }`              | Verify OTP                  | `{ message }` | 400, 404, 401, 410, 500 |

---

## 📖 Search History

| Method | Endpoint                              | Headers | Body                  | Description                             | Response | Errors |
|--------|---------------------------------------|---------|-----------------------|-----------------------------------------|----------|--------|
| POST   | `/api/searchHistory/addSearch`        | `userId`| `{ query }`           | Add user search                         | `{ message }` | 400, 409, 500 |
| GET    | `/api/searchHistory/getUserSearches`  | `userId`| –                     | Get user's previous searches            | `{ data: [query] }` | 400, 404, 500 |
| GET    | `/api/searchHistory/getSearchSuggestions` | –   | – or `{ query }`      | Suggest searches by query               | `{ data: [query] }` | 400, 404, 500 |

---

## 🏨 Hotels

| Method | Endpoint                        | Params / Query | Headers   | Body                         | Description                       | Response | Errors |
|--------|----------------------------------|----------------|-----------|------------------------------|-----------------------------------|----------|--------|
| POST   | `/api/hotel/add`                | –              | `adminId` | `{ hotelName, city, location, pictures? }` | Create new hotel                 | `{ hotelId }` | 400, 409, 500 |
| GET    | `/api/hotel/getHotelsBySearch`  | `{ query, limit?, page? }` | `userId?` | – | Search hotels by query         | `{ data: [Hotel], message }` | 400, 404, 500 |
| GET    | `/api/hotel/getFilteredHotelFeed`| `userLat, userLon, filtering, limit?, page?` | – | – | Filtered hotel feed            | `{ data: [Hotel] }` | 400, 404, 500 |
| GET    | `/api/hotel/sorted-by-distance` | `userLat, userLon, limit?, page?` | – | – | Hotels sorted by distance      | `{ data: [Hotel] }` | 400, 404, 500 |
| GET    | `/api/hotel/navigateToHotel`    | `userLat, userLon` | – | – | Get closest hotel             | `{ data: Hotel }` | 400, 404, 500 |

---

## 🧾 Objects

### 🧍‍♂️ User Object

```json
{
  "id": "string",
  "first_name": "string",
  "last_name": "string",
  "phone": "string?",
  "address": "string?",
  "profile_picture": "string?",
  "favorites": ["hotelId"]
}
```

### 🏨 Hotel Object

```json
{
  "id": "string",
  "name": "string",
  "city": "string",
  "country": "string",
  "location": {
    "latitude": number,
    "longitude": number
  },
  "pictures": ["string"],
  "totalReviews": number,
  "averageRating": number
}
```