# Email Setup Guide - How to Send Emails from Django

Currently, your Django app is configured to **print emails to the console** instead of sending them. To actually send emails, follow these steps:

## Step 1: Enable 2-Step Verification on Gmail

1. Go to: https://myaccount.google.com/security
2. Sign in with your Gmail account (`kenyanhomes1@gmail.com`)
3. Find "2-Step Verification" and click **Enable**
4. Follow the setup process (you'll need your phone)

## Step 2: Generate a Gmail App Password

1. Go to: https://myaccount.google.com/apppasswords
2. Sign in with: `kenyanhomes1@gmail.com`
3. Under "Select app", choose **"Mail"**
4. Under "Select device", choose **"Other (Custom name)"**
5. Enter: `Django App`
6. Click **"Generate"**
7. You'll see a 16-character password like: `abcd efgh ijkl mnop`
8. **Copy this password** (you can't see it again!)

## Step 3: Update Django Settings

1. Open `kenyan_homes/settings.py`
2. Find the line: `EMAIL_HOST_PASSWORD = 'your-app-password-here'`
3. Replace `'your-app-password-here'` with your App Password (remove spaces)
   - Example: `EMAIL_HOST_PASSWORD = 'abcdefghijklmnop'`

## Step 4: Restart Django Server

After updating the settings, restart your Django development server:

```bash
# Stop the server (Ctrl+C)
# Then restart:
python manage.py runserver
```

## Step 5: Test Email Sending

1. Go to your website
2. Try to sign up a new user
3. Check the email inbox of the email address you used
4. You should receive a verification email!

## Troubleshooting

### If emails still don't send:

1. **Check the App Password**: Make sure there are no spaces and it's exactly 16 characters
2. **Check 2-Step Verification**: Make sure it's enabled on your Google account
3. **Check Django logs**: Look at your terminal/console for error messages
4. **Check spam folder**: Sometimes verification emails go to spam

### To test without sending emails (development):

If you want to see emails in the console instead of sending them:

1. In `settings.py`, comment out the SMTP settings:
   ```python
   # EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
   # EMAIL_HOST = 'smtp.gmail.com'
   # ... (comment out all SMTP lines)
   ```

2. Uncomment the console backend:
   ```python
   EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'
   ```

3. Restart the server

## Current Settings

Your current email configuration in `settings.py`:

```python
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_PORT = 587
EMAIL_USE_TLS = True
EMAIL_HOST_USER = 'kenyanhomes1@gmail.com'
EMAIL_HOST_PASSWORD = 'your-app-password-here'  # ← Replace this!
DEFAULT_FROM_EMAIL = 'kenyanhomes1@gmail.com'
```

## Security Note

⚠️ **Never commit your App Password to Git!** 

Consider using environment variables for production:
```python
import os
EMAIL_HOST_PASSWORD = os.environ.get('GMAIL_APP_PASSWORD')
```

