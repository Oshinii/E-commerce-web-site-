# 📘 Bloom & Co. Complete Backend Documentation

## 🌸 Project Overview

Bloom & Co. is a modern e-commerce website with a fully functional backend that includes:
- User registration with automated welcome emails
- Secure password reset using OTP (One-Time Password)
- Email notifications
- RESTful API architecture
- User data persistence

---

## 🚀 Getting Started (5 Minutes)

### 1. Install Node.js (if not already installed)
Download from: https://nodejs.org/

### 2. Install Dependencies
```bash
cd path/to/oshini
npm install
```

### 3. Start the Server
```bash
npm start
```

**Expected output:**
```
🌸 Bloom & Co. Website running at http://127.0.0.1:3000
📧 Email service configured for user registration and password reset
```

### 4. Open in Browser
Visit: `http://127.0.0.1:3000`

---

## 📊 Architecture

### Frontend
- HTML: `account.html`, `index.html`, `cart.html`, etc.
- CSS: `styles.css` with pink/rose theme
- JavaScript: `script.js` with API integration

### Backend
- **Server**: `server.js` - Node.js HTTP server with API endpoints
- **Database**: 
  - `users.json` - User accounts
  - `otps.json` - Temporary OTP storage
- **Email**: Nodemailer with Ethereal Email (development)

### API Endpoints
```
POST /api/register        - Create new user account
POST /api/forgot-password - Request password reset OTP
POST /api/verify-otp      - Verify OTP and reset password
```

---

## 📧 Complete User Workflows

### Registration Workflow

#### Frontend (User Action)
1. User visits http://127.0.0.1:3000
2. Clicks "Account" tab → "Register" tab
3. Fills form:
   ```
   Full Name: John Doe
   Email: john@example.com
   Phone: +1-555-0123
   Postal Code: 10001
   Country: United States
   Password: SecurePass123
   ```
4. Clicks "Register" button

#### Backend (Server Processing)
1. Server receives POST request to `/api/register`
2. Validates all fields are provided
3. Checks if email already registered
4. Creates user object:
   ```json
   {
     "id": "1704099600123",
     "name": "John Doe",
     "email": "john@example.com",
     "phone": "+1-555-0123",
     "country": "United States",
     "postalCode": "10001",
     "password": "SecurePass123",
     "createdAt": "2024-01-01T12:30:00.123Z"
   }
   ```
5. Saves user to `users.json`
6. Generates welcome email with HTML template
7. Sends email via Nodemailer
8. Returns success response

#### Email Content
```
Subject: Welcome to Bloom & Co.™
From: noreply@bloomco.com
To: john@example.com

Body:
- Welcome message
- Account confirmation
- Features available
- Company signature
```

#### Frontend Result
- Shows "Registration successful!" message
- Clears form fields
- Logs user in (sets localStorage)
- Redirects to home page after 1.5 seconds

---

### Password Reset Workflow

#### Step 1: Request OTP

**Frontend (User Action)**
1. User visits Account page (logged out)
2. Clicks "Forgot Password?" link
3. Enters email: `john@example.com`
4. Clicks "Send Reset OTP"

**Backend Processing**
1. Receives POST to `/api/forgot-password`
2. Finds user by email in `users.json`
3. Generates OTP: `123456` (6 random digits)
4. Calculates expiry: Current time + 10 minutes
5. Stores in `otps.json`:
   ```json
   {
     "john@example.com": {
       "code": "123456",
       "createdAt": 1704099600000,
       "expiresAt": 1704100200000
     }
   }
   ```
6. Creates OTP email with security warnings
7. Sends email to `john@example.com`
8. Returns success message

**Email Content**
```
Subject: Password Reset OTP - Bloom & Co.™
From: noreply@bloomco.com
To: john@example.com

Body:
- Password reset request confirmation
- 6-digit OTP in large box
- 10-minute expiry warning
- Security warnings
- Instructions
```

**Frontend Result**
- Shows "OTP sent to your email" message
- Switches to OTP verification panel
- User sees fields for OTP and new password

#### Step 2: Verify OTP & Reset Password

**Frontend (User Action)**
1. User receives email with OTP: `123456`
2. Enters OTP: `123456`
3. Enters new password: `NewSecurePass456`
4. Clicks "Reset Password"

**Backend Processing**
1. Receives POST to `/api/verify-otp` with:
   - email: `john@example.com`
   - otp: `123456`
   - newPassword: `NewSecurePass456`

2. Validates:
   - OTP exists for email ✓
   - OTP not expired ✓
   - OTP matches stored value ✓

3. Updates user in `users.json`:
   - Changes password to `NewSecurePass456`

4. Deletes OTP from `otps.json` (cleanup)

5. Returns success response

**Frontend Result**
- Shows "Password reset successfully!" message
- Clears form fields
- Switches back to login panel
- User can now login with new password

---

## 🔌 API Detailed Reference

### 1. User Registration

**Endpoint**: `POST /api/register`

**Request**
```bash
curl -X POST http://127.0.0.1:3000/api/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "phone": "+1-555-0123",
    "country": "United States",
    "postalCode": "10001",
    "password": "SecurePass123"
  }'
```

**Response - Success (201)**
```json
{
  "success": true,
  "message": "Registration successful! Welcome email sent.",
  "user": {
    "id": "1704099600123",
    "name": "John Doe",
    "email": "john@example.com"
  }
}
```

**Response - Duplicate Email (409)**
```json
{
  "error": "Email already registered"
}
```

**Response - Missing Fields (400)**
```json
{
  "error": "Missing required fields"
}
```

**Response - Server Error (500)**
```json
{
  "error": "Registration failed",
  "details": "Error message here"
}
```

---

### 2. Request Password Reset

**Endpoint**: `POST /api/forgot-password`

**Request**
```bash
curl -X POST http://127.0.0.1:3000/api/forgot-password \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john@example.com"
  }'
```

**Response - Success (200)**
```json
{
  "success": true,
  "message": "OTP sent to email"
}
```

**Response - No Email Provided (400)**
```json
{
  "error": "Email is required"
}
```

**Response - Server Error (500)**
```json
{
  "error": "Failed to process request"
}
```

---

### 3. Verify OTP & Reset Password

**Endpoint**: `POST /api/verify-otp`

**Request**
```bash
curl -X POST http://127.0.0.1:3000/api/verify-otp \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john@example.com",
    "otp": "123456",
    "newPassword": "NewSecurePass456"
  }'
```

**Response - Success (200)**
```json
{
  "success": true,
  "message": "OTP verified successfully"
}
```

**Response - Invalid OTP (400)**
```json
{
  "error": "Invalid OTP"
}
```

**Response - Expired OTP (400)**
```json
{
  "error": "OTP has expired"
}
```

**Response - Missing Email/OTP (400)**
```json
{
  "error": "Email and OTP required"
}
```

---

## 💾 Data Structures

### User Object (in users.json)
```json
{
  "id": "1704099600123",
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "+1-555-0123",
  "country": "United States",
  "postalCode": "10001",
  "password": "SecurePass123",
  "createdAt": "2024-01-01T12:30:00.123Z"
}
```

**Fields**:
- `id`: Unique identifier (timestamp-based)
- `name`: User's full name
- `email`: Email address (unique)
- `phone`: Phone number
- `country`: Country of residence
- `postalCode`: Postal/ZIP code
- `password`: Password (plain text - hash in production!)
- `createdAt`: Account creation timestamp

### OTP Object (in otps.json)
```json
{
  "john@example.com": {
    "code": "123456",
    "createdAt": 1704099600000,
    "expiresAt": 1704100200000
  }
}
```

**Fields**:
- Key: User's email address
- `code`: 6-digit OTP
- `createdAt`: Unix timestamp when OTP created
- `expiresAt`: Unix timestamp when OTP expires (10 minutes)

---

## 🔐 Security Details

### Current Implementation (Development)
```javascript
// Passwords stored as-is
user.password = "SecurePass123"; // ❌ Not secure for production

// OTP stored in JSON file
otps.json // ❌ Plain text storage

// Email via test service
// ✅ Fine for development testing
```

### Production Implementation (Required)
```javascript
// Hash passwords with bcrypt
const bcrypt = require('bcrypt');
const hashedPassword = await bcrypt.hash(password, 10);
user.password = hashedPassword; // ✅ Secure

// Store OTP in encrypted database
// ✅ Use proper database with encryption

// Use production email service
// ✅ Gmail, SendGrid, Amazon SES, etc.

// Environment variables for secrets
// ✅ Use .env file for credentials
```

---

## 📁 File Structure

```
oshini/
├── server.js                    # Backend API server
├── script.js                    # Frontend JavaScript
├── styles.css                   # Styling
├── package.json                 # Dependencies
├── account.html                 # Registration/Login page
├── index.html                   # Home page
├── cart.html                    # Shopping cart
├── wishlist.html                # Wishlist
├── admin.html                   # Admin panel
├── accessories.html             # Products page
├── kidswear.html                # Products page
│
├── users.json                   # User database (auto-created)
├── otps.json                    # OTP storage (auto-created)
├── uploads/                     # Uploaded files
│
├── QUICK_START.md               # Quick start guide
├── SETUP_GUIDE.md               # Detailed setup instructions
├── API_REFERENCE.md             # API endpoints reference
└── IMPLEMENTATION_SUMMARY.md    # Implementation overview
```

---

## 🧪 Testing Examples

### Using JavaScript (Browser Console)

**Register a user**:
```javascript
fetch('/api/register', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    name: 'Test User',
    email: 'test@example.com',
    phone: '1234567890',
    country: 'United States',
    postalCode: '12345',
    password: 'Test@123'
  })
})
.then(r => r.json())
.then(console.log)
.catch(console.error);
```

**Request OTP**:
```javascript
fetch('/api/forgot-password', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    email: 'test@example.com'
  })
})
.then(r => r.json())
.then(console.log)
.catch(console.error);
```

**Verify OTP**:
```javascript
fetch('/api/verify-otp', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    email: 'test@example.com',
    otp: '123456',
    newPassword: 'NewPass@123'
  })
})
.then(r => r.json())
.then(console.log)
.catch(console.error);
```

### Using Postman

1. **New Request** → POST
2. **URL**: `http://127.0.0.1:3000/api/register`
3. **Headers** tab → Add `Content-Type: application/json`
4. **Body** tab → Select `raw` → Paste JSON data
5. **Send** → View response

### Using cURL

**In terminal/command prompt**:
```bash
curl -X POST http://127.0.0.1:3000/api/register \
  -H "Content-Type: application/json" \
  -d '{"name":"Test","email":"test@example.com","phone":"1234567890","country":"United States","postalCode":"12345","password":"Test@123"}'
```

---

## 🆘 Troubleshooting Guide

### "Cannot find module 'nodemailer'"
```bash
# Solution: Install dependencies
npm install
```

### "EADDRINUSE: address already in use :::3000"
```bash
# Solution 1: Change port in server.js
const port = 3001; // Change 3000 to 3001

# Solution 2: Find and kill process using port 3000
# On Windows:
netstat -ano | findstr :3000
taskkill /PID <PID> /F

# On Mac/Linux:
lsof -i :3000
kill -9 <PID>
```

### "Emails not sending"
1. Check internet connection
2. Verify SMTP credentials in server.js
3. Check firewall isn't blocking SMTP port 587
4. For Ethereal: Visit https://ethereal.email/messages to view test emails

### "OTP expired error"
- OTPs are only valid for 10 minutes
- User should request a new OTP
- Check that server time is correct

### "Email already registered"
- Use a different email address, OR
- Use password reset to regain access to existing account

---

## 🎯 Key Features Summary

| Feature | Status | Details |
|---------|--------|---------|
| User Registration | ✅ Active | Creates account, sends welcome email |
| Email Verification | ❌ Not included | Can be added |
| Password Reset | ✅ Active | OTP-based, 10-minute expiry |
| User Database | ✅ Active | JSON file storage |
| Email Service | ✅ Active | Ethereal (dev) or configurable (prod) |
| API Endpoints | ✅ Active | 3 main endpoints |
| CORS Support | ✅ Active | All requests allowed |
| Error Handling | ✅ Active | Comprehensive error messages |

---

## 📞 Additional Resources

- **Node.js Docs**: https://nodejs.org/docs/
- **Nodemailer**: https://nodemailer.com/
- **SMTP Providers**: 
  - Gmail: https://support.google.com/mail/answer/185833
  - SendGrid: https://sendgrid.com/
  - Ethereal: https://ethereal.email/
- **REST API Standards**: https://restfulapi.net/

---

## 🎓 Learning Path

1. **Understand the code**:
   - Read `server.js` to understand API structure
   - Review email configuration section
   - Check user registration endpoint

2. **Test the APIs**:
   - Use browser console to test endpoints
   - Check responses in DevTools Network tab
   - Verify data in `users.json` and `otps.json`

3. **Customize for production**:
   - Update email configuration
   - Add password hashing with bcrypt
   - Migrate to proper database
   - Implement additional features

4. **Deploy to production**:
   - Choose hosting (Heroku, AWS, DigitalOcean, etc.)
   - Set up environment variables
   - Configure production email service
   - Enable HTTPS/SSL
   - Set up monitoring and logging

---

## ✨ Best Practices

1. **Always validate input** - Check all data before processing
2. **Hash passwords** - Never store plain text passwords
3. **Use HTTPS** - Encrypt data in transit
4. **Handle errors gracefully** - Return meaningful error messages
5. **Log activities** - Track important events for debugging
6. **Rate limit** - Prevent abuse of APIs
7. **Secure emails** - Don't send sensitive data via email
8. **Test thoroughly** - Use automated tests for reliability

---

## 🚀 Ready to Go!

You now have a fully functional backend for Bloom & Co. with:
- User registration and welcome emails ✅
- Secure password reset with OTP ✅
- RESTful API architecture ✅
- Production-ready code structure ✅

Start with: `npm install && npm start`

---

© 2024 Bloom & Co.™ All rights reserved.
