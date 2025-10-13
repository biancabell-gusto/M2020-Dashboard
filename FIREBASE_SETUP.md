# Firebase Setup Guide

## Overview
The dashboard now uses Firebase Realtime Database for true multi-user synchronization. This means when any user creates a partner meeting or joins a session, **all other users see it instantly**.

## Setup Instructions (5 minutes)

### 1. Create a Firebase Project
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click **"Add project"**
3. Name it: `M2020-Dashboard` (or anything you prefer)
4. Disable Google Analytics (optional)
5. Click **"Create project"**

### 2. Enable Realtime Database
1. In your Firebase project, click **"Build"** → **"Realtime Database"**
2. Click **"Create Database"**
3. Choose location: **United States** (or closest to your team)
4. Start in **"test mode"** (we'll secure it later)
5. Click **"Enable"**

### 3. Get Your Configuration
1. In Firebase Console, click the **gear icon** (⚙️) → **"Project settings"**
2. Scroll down to **"Your apps"**
3. Click the **Web icon** (`</>`)
4. Register your app with a nickname: `M2020 Dashboard`
5. Copy the `firebaseConfig` object

### 4. Update Your Dashboard
1. Open `index.html`
2. Find the `firebaseConfig` section (around line 343)
3. Replace the placeholder values with your actual Firebase config:

```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY_HERE",
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    databaseURL: "https://YOUR_PROJECT_ID-default-rtdb.firebaseio.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT_ID.appspot.com",
    messagingSenderId: "YOUR_MESSAGING_ID",
    appId: "YOUR_APP_ID"
};
```

### 5. Set Database Rules (Important!)
1. In Firebase Console, go to **"Realtime Database"** → **"Rules"** tab
2. Replace the rules with:

```json
{
  "rules": {
    "conference_data": {
      ".read": true,
      ".write": true
    }
  }
}
```

3. Click **"Publish"**

> ⚠️ **Note**: These rules allow anyone to read/write. For production, add authentication!

## Testing Real-Time Sync

1. Open your dashboard in **two different browsers** (Chrome + Firefox, or regular + incognito)
2. In Browser 1: Add a partner meeting
3. In Browser 2: **Watch it appear instantly!** ✨
4. Try joining sessions - both browsers stay in sync

## What You Get

✅ **True Real-Time Sync** - Changes appear instantly across all users  
✅ **Persistent Storage** - Data survives browser refresh and device changes  
✅ **Team Collaboration** - Everyone sees the same partner meetings  
✅ **Automatic Fallback** - If Firebase is down, uses localStorage  

## Deployment to GitHub Pages

After setting up Firebase:

```bash
git add .
git commit -m "Add Firebase real-time sync"
git push origin main
```

Your GitHub Pages site will automatically update with Firebase sync!

## Security (Optional - For Production)

For a production environment, add authentication:

1. Enable **Email/Password** authentication in Firebase
2. Update database rules:

```json
{
  "rules": {
    "conference_data": {
      ".read": "auth != null",
      ".write": "auth != null"
    }
  }
}
```

3. Add login UI to your dashboard

## Troubleshooting

### "Firebase is not defined" error
- Make sure the Firebase SDK scripts are loaded before your JavaScript code
- Check browser console for 404 errors on Firebase scripts

### Changes not syncing
- Check browser console for Firebase errors
- Verify your `databaseURL` is correct
- Make sure database rules allow read/write

### "Permission denied" error
- Your database rules are too restrictive
- Use the test mode rules provided above

## Cost

Firebase Realtime Database free tier includes:
- **1 GB stored**
- **10 GB/month downloaded**
- **100 simultaneous connections**

This is **more than enough** for a conference dashboard with hundreds of users!

## Questions?

Check the [Firebase Documentation](https://firebase.google.com/docs/database) or open an issue.

