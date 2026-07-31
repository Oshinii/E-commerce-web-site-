# Bloom & Co. - Backend API Quick Reference

## 🚀 Quick Start

### Installation
```bash
cd oshini
npm install
npm start
```

Server will run at: `http://127.0.0.1:3000`

---

## 📧 Email Features

### 1. User Registration with Welcome Email
- User fills registration form
- Account is created in `users.json`
- Welcome email is automatically sent
- Frontend redirects to home page

### 2. Password Reset with OTP
- User enters email → OTP is generated and sent
- User receives 6-digit OTP via email
- User enters OTP + new password → Password is reset
- OTP expires after 10 minutes

---

## 🔌 API Endpoints

### Register User
```http
POST /api/register
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "1234567890",
  "country": "United States",
  "postalCode": "12345",
  "password": "securePassword123"
}
```

### Request Password Reset OTP
```http
POST /api/forgot-password
Content-Type: application/json

{
  "email": "john@example.com"
}
```

### Verify OTP & Reset Password
```http
POST /api/verify-otp
Content-Type: application/json

{
  "email": "john@example.com",
  "otp": "123456",
  "newPassword": "newSecurePassword123"
}
```

---

## 📂 Files Created/Modified

### New Files
- ✅ `SETUP_GUIDE.md` - Detailed setup instructions
- ✅ `API_REFERENCE.md` - This file
- ✅ `users.json` - User database (auto-created)
- ✅ `otps.json` - OTP storage (auto-created)

### Modified Files
- ✅ `server.js` - Added API endpoints and email service
- ✅ `package.json` - Added nodemailer dependency
- ✅ `script.js` - Updated forms to use API calls
- ✅ `account.html` - Added OTP verification panel

---

## 💾 Data Storage

### Users Database Structure
```json
{
  "id": "1234567890",
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "1234567890",
  "country": "United States",
  "postalCode": "12345",
  "password": "securePassword123",
  "createdAt": "2024-01-01T10:00:00.000Z"
}
```

### OTP Structure
```json
{
  "email@example.com": {
    "code": "123456",
    "createdAt": 1704099600000,
    "expiresAt": 1704100200000
  }
}
```

---

## 🔐 Security Notes

⚠️ **Current Development Setup**
- Passwords stored in plain text
- Using Ethereal Email (test service)
- Data stored in JSON files

✅ **For Production**
1. Hash passwords with `bcrypt`
2. Use real email service (Gmail, SendGrid)
3. Move to proper database (MongoDB, PostgreSQL)
4. Enable HTTPS
5. Add rate limiting
6. Verify email addresses
7. Store secrets in environment variables

---

## 📧 Email Configuration

### Current (Development)
- Service: Ethereal Email
- Host: smtp.ethereal.email
- Credentials: Built-in test account

### For Production
Update `server.js` with your email service:

**Gmail:**
```javascript
const transporter = nodemailer.createTransport({
  service: 'gmail',
  auth: {
    user: 'your-email@gmail.com',
    pass: 'your-app-password'
  }
});
```

**SendGrid:**
```javascript
const transporter = nodemailer.createTransport({
  host: 'smtp.sendgrid.net',
  port: 587,
  auth: {
    user: 'apikey',
    pass: 'your-api-key'
  }
});
```

---

## 🧪 Testing

### Using Browser DevTools Console
```javascript
// Test registration
fetch('/api/register', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    name: 'Test User',
    email: 'test@example.com',
    phone: '1234567890',
    country: 'United States',
    password: 'Test@123'
  })
}).then(r => r.json()).then(console.log);
```

### Using cURL
```bash
curl -X POST http://127.0.0.1:3000/api/register \
  -H "Content-Type: application/json" \
  -d '{
    "name":"Test User",
    "email":"test@example.com",
    "phone":"1234567890",
    "country":"United States",
    "password":"Test@123"
  }'
```

---

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| `npm install` fails | Install Node.js first |
| Port 3000 in use | Change port in `server.js` or close other apps |
| Emails not sending | Check internet, verify email config |
| OTP not received | Check spam folder, re-request OTP |
| 404 errors | Ensure server is running and URL is correct |

---

## 📞 Support

For issues:
1. Check browser console for errors
2. Check server console for logs
3. Review `SETUP_GUIDE.md`
4. Verify network connectivity
5. Test API endpoints with cURL/Postman

---

© 2024 Bloom & Co.™ All rights reserved.
