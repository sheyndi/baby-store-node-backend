
# 🌍 Focus - Backend Server

שרת Backend של מערכת **Focus** – מערכת לניהול מידע תיירותי והצגת המלצות אמינות בתחום האירוח. נבנה באמצעות Node.js, Express, TypeScript ו-MySQL.

---

## 🚀 טכנולוגיות בשימוש

- **Node.js**
- **Express**
- **TypeScript**
- **MySQL**
- **dotenv**
- **JWT**
- **REST API**

---

## ⚙️ התקנה והרצה

```bash
# התקנת תלויות
npm install

# יצירת קובץ סביבה
cp .env.example .env
```

### דוגמה לקובץ `.env`:

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
# הרצת שרת בפיתוח
npm run dev
```

---

## 📁 מבנה תיקיות

```
src/
├── routes/         # ניתובים
├── controllers/    # לוגיקת הבקשות
├── models/         # שאילתות למסד הנתונים
├── middleware/     # הרשאות, טיפול שגיאות
├── config/         # חיבור למסד נתונים
├── types/          # טיפוסים ל-TypeScript
└── index.ts        # נקודת כניסה
```

---

## 📡 REST API

### 👤 משתמשים

| CRUD     | כתובת                  | מתודה | פרמטרים | Headers | Body | תיאור |
|----------|------------------------|--------|----------|---------|------|--------|
| יצירה    | `/api/users/register`  | POST   |          |         | `{ name, email, password }` | הרשמת משתמש |
| התחברות | `/api/users/login`     | POST   |          |         | `{ email, password }` | התחברות עם טוקן |
| שליפה   | `/api/users/profile`   | GET    |          | `Authorization: Bearer <token>` | - | פרטי משתמש מחובר |

---

### 🏨 מלונות

| CRUD     | כתובת              | מתודה | פרמטרים | Headers | Body | תיאור |
|----------|--------------------|--------|----------|---------|------|--------|
| שליפה    | `/api/hotels`      | GET    |          |         |      | כל המלונות |
| לפי ID   | `/api/hotels/:id`  | GET    | `id`     |         |      | מלון מסוים |
| חיפוש    | `/api/hotels/search` | GET  | Query Params | |      | חיפוש לפי פילטרים |

---

### 📝 ביקורות

| CRUD     | כתובת                      | מתודה | פרמטרים | Headers | Body | תיאור |
|----------|----------------------------|--------|----------|---------|------|--------|
| יצירה    | `/api/reviews/`            | POST   |          | `Authorization` | `{ hotelId, rating, comment }` | הוספת ביקורת |
| לפי מלון | `/api/reviews/hotel/:id`   | GET    | `id`     |         |      | כל הביקורות של מלון |

---

### ❤️ מלונות אהובים

| CRUD     | כתובת                          | מתודה | פרמטרים | Headers | Body | תיאור |
|----------|--------------------------------|--------|----------|---------|------|--------|
| עדכון    | `/api/users/updateLovedHotels` | PATCH  |          | `Authorization` | `{ hotelId }` | הוספה/הסרה של מלון אהוב |

---

### 📢 הודעות מערכת

| CRUD     | כתובת                      | מתודה | פרמטרים | Headers | Body | תיאור |
|----------|----------------------------|--------|----------|---------|------|--------|
| שליפה    | `/api/systemMessages`      | GET    |          |         |      | כל ההודעות הפעילות |
| יצירה    | `/api/systemMessages`      | POST   |          |         | `{ title, content, expires_at }` | יצירת הודעה חדשה |

---

## 🔒 הרשאות

- קריאות מסוימות דורשות JWT Token.
- יש להעביר Header מסוג:  
  `Authorization: Bearer <your_token>`

---

## 🧪 בדיקות

- ניתן לבדוק את המערכת בעזרת:
  - Postman
  - Thunder Client
  - curl

---

## 👤 קריאות API - משתמש

<table>
    <thead>
        <tr>
            <th>CRUD</th>
            <th>כתובת</th>
            <th>פרמטרים</th>
            <th>Headers</th>
            <th>Body</th>
            <th>מה עושה</th>
            <th>מה מוחזר</th>
            <th>שגיאות אפשריות</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>GET</td>
            <td>/api/user/getUserDetails</td>
            <td>-</td>
            <td>userId: string</td>
            <td>-</td>
            <td>מחזירה את פרטי המשתמש לפי מזהה</td>
            <td>
<pre>
{ 
  title: "Success",
  message: "User details fetched successfully.",
  data: {
    id: string,
    first_name: string,
    last_name: string,
    phone: string,
    address: string,
    profilePicture: string,
    role: string,
    authMethod: string,
    location: {
      latitude: number, 
      longitude: number
    },
    loved_Hotels: [...],
    created_at: date
  }
}
</pre>
            </td>
            <td>
                <ul>
                    <li>400 - חסר מזהה משתמש</li>
                    <li>404 - משתמש לא נמצא</li>
                    <li>500 - שגיאת שרת</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>POST</td>
            <td>/api/user/addOrUpdateUser</td>
            <td>-</td>
            <td>userId: string</td>
            <td>
<pre>
{ 
  first_name: string,
  last_name: string,
  phone?: string,
  address?: string,
  profile_picture?: string
}
</pre>
            </td>
            <td>יוצרת משתמש חדש או מעדכנת משתמש קיים לפי id</td>
            <td>
<pre>
1. { message: "User added successfully", userId: string }
OR
2. { message: "User updated successfully" }
</pre>
            </td>
            <td>
                <ul>
                    <li>400 - שדות חובה חסרים</li>
                    <li>400 - שגיאה בהוספת/עדכון המשתמש</li>
                    <li>500 - שגיאת שרת</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>PATCH</td>
            <td>/api/user/favorites/</td>
            <td>-</td>
            <td>userId: string</td>
            <td>
<pre>
{
  hotelId: string,
  action: "add" | "remove"
}
</pre>
            </td>
            <td>מעדכנת את רשימת המלונות האהובים של המשתמש</td>
            <td>
<pre>
{
  title: "Success",
  message: "Loved hotels updated successfully.",
  hotelId: string,
  action: "add" | "remove",
  updatedList: [string],
  userId: string
}
</pre>
            </td>
            <td>
                <ul>
                    <li>400 - חסר hotelId או action</li>
                    <li>400 - פעולה לא חוקית</li>
                    <li>404 - משתמש או מלון לא נמצא</li>
                    <li>400/500 - שגיאה כללית</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>DELETE</td>
            <td>/api/user/:userId</td>
            <td>userId: string</td>
            <td>userId: string</td>
            <td>-</td>
            <td>מוחק משתמש לפי מזהה</td>
            <td>
<pre>
{
  title: "Success",
  message: "User deleted successfully."
}
</pre>
            </td>
            <td>
                <ul>
                    <li>400 - חסר מזהה משתמש</li>
                    <li>404 - משתמש לא נמצא</li>
                    <li>430 - לא מורשה למחוק</li>
                    <li>500 - שגיאת שרת</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

## Auth

| CRUD | Endpoint                  | Params         | Headers | Body | Description | Response | Errors |
|------|---------------------------|----------------|---------|------|-------------|----------|--------|
| GET  | /api/auth/getOTP/:email   | email: string  | -       | -    | Sends a one-time password to the email | `{ "title": "OTP Sent", "message": "A one-time password has been sent to your email address." }` | 400 - Missing email<br>500 - OTP save failed<br>500 - Email send error<br>500 - Server error |
| POST | /api/auth/verifyOTP       | -              | -       | `{ "email": string, "otp": string }` | Verifies the OTP | `{ "title": "OTP Verified", "message": "Verification successful. You may continue." }` | 400 - Missing params<br>404 - Code not found<br>401 - Incorrect code<br>410 - Expired code<br>500 - Deletion failed<br>500 - Server error |

## Search History

| CRUD | Endpoint                              | Params | Headers | Body | Description | Response | Errors |
|------|---------------------------------------|--------|---------|------|-------------|----------|--------|
| POST | /api/searchHistory/addSearch          | -      | userId  | `{ "query": string }` | Adds a new search to the user (if not already exists) | `{ "title": "Success", "message": "Search saved successfully." }` | 400 - Missing fields<br>409 - Search already exists<br>500 - Server error |
| GET  | /api/searchHistory/getUserSearches    | -      | userId  | -    | Retrieves all user searches by descending time | `{ "title": "Success", "message": "User searches fetched successfully.", "data": [query, ...] }` | 400 - Missing userId<br>404 - No searches found<br>500 - Server error |
| GET  | /api/searchHistory/getSearchSuggestions | `{ "query": string }` | - | - | Suggests searches based on partial text | `{ "title": "Success", "message": "User searches fetched successfully.", "data": [query, ...] }` | 400 - Missing query<br>404 - No suggestions found<br>500 - Server error |
