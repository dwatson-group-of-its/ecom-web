# Heroku Login Troubleshooting Guide

## Issues Fixed

✅ **JavaScript Error**: Added Bootstrap JS bundle to login page  
✅ **Alert Function**: Fixed Bootstrap 5 alert dismissal  
✅ **Error Handling**: Improved backend error messages  
✅ **Admin User**: Auto-creates admin user on server startup  

## Required Heroku Environment Variables

Make sure these are set in your Heroku app:

```bash
heroku config:set MONGODB_URI="your_mongodb_connection_string"
heroku config:set JWT_SECRET="your_super_secret_jwt_key_min_32_characters"
heroku config:set ADMIN_EMAIL="admin@dwatson.pk"
heroku config:set ADMIN_PASSWORD="your_secure_password"
```

### Check Current Config Variables

```bash
heroku config
```

### Set Missing Variables

```bash
# MongoDB (required)
heroku config:set MONGODB_URI="mongodb+srv://username:password@cluster.mongodb.net/dwatson_pk"

# JWT Secret (required - min 32 characters)
heroku config:set JWT_SECRET="your-very-long-secret-key-at-least-32-characters"

# Admin Credentials (required)
heroku config:set ADMIN_EMAIL="admin@dwatson.pk"
heroku config:set ADMIN_PASSWORD="YourSecurePassword123"

# Optional
heroku config:set CONTACT_EMAIL="dwatsononline.co@gmail.com"
```

## Verify Deployment

1. **Check Logs:**
   ```bash
   heroku logs --tail
   ```

2. **Look for these messages:**
   - ✅ `MongoDB connected`
   - ✅ `Admin user created: admin@dwatson.pk` OR `Admin user already exists: admin@dwatson.pk`
   - ✅ `Server running on port 5000`

3. **If you see errors:**
   - `MongoDB connection error` → Check `MONGODB_URI`
   - `JWT_SECRET is not set` → Set `JWT_SECRET` environment variable
   - `Error ensuring admin user` → Check database connection

## Test Login

1. Go to: `https://your-app.herokuapp.com/login`
2. Use credentials from `ADMIN_EMAIL` and `ADMIN_PASSWORD`
3. If login fails, check Heroku logs for specific errors

## Common Issues

### 500 Internal Server Error

**Causes:**
- Missing `JWT_SECRET` environment variable
- Database connection failed
- Admin user doesn't exist (should auto-create now)

**Solution:**
1. Check Heroku logs: `heroku logs --tail`
2. Verify all environment variables are set
3. Restart the app: `heroku restart`

### "Invalid credentials" Error

**Causes:**
- Wrong email/password
- Admin user not created

**Solution:**
1. Verify `ADMIN_EMAIL` and `ADMIN_PASSWORD` in Heroku config
2. Check logs to see if admin user was created
3. If needed, manually create admin via database or run init script

### JavaScript Errors

**Fixed in latest update:**
- Bootstrap JS bundle now included
- Alert dismissal now works correctly

## Manual Admin Creation (if needed)

If admin user wasn't auto-created, you can create it manually:

```bash
heroku run node backend/scripts/database-init.js
```

Or connect to MongoDB and create user directly.

## Restart App After Config Changes

After setting environment variables:

```bash
heroku restart
```

## Still Having Issues?

1. Check Heroku logs: `heroku logs --tail`
2. Verify all environment variables are set
3. Ensure MongoDB Atlas allows connections from Heroku (IP whitelist: `0.0.0.0/0`)
4. Check that the app is running: `heroku ps`

