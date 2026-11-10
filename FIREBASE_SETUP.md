# Firebase Setup Guide for Upload Tracker

Your app now syncs data across devices using Firebase Realtime Database. Follow these steps to enable it:

## Step 1: Create a Firebase Project
1. Go to https://firebase.google.com
2. Click "Go to console" (top right)
3. Click "Create a project"
4. Enter project name: `upload-tracker-ruffy`
5. Accept terms and create

## Step 2: Set Up Realtime Database
1. In the Firebase console, click "Build" → "Realtime Database"
2. Click "Create Database"
3. Choose your region (closest to you)
4. Start in **Test mode** (allows anyone to read/write for development)
5. Copy your Database URL (looks like: `https://project-name-default-rtdb.firebaseio.com`)

## Step 3: Get Firebase Config
1. In Firebase console, click the gear icon (⚙️) → "Project settings"
2. Scroll to "Your apps" section
3. Click "</>" to create a web app
4. Enter app nickname: `UploadTracker`
5. Copy the Firebase config object

## Step 4: Update the HTML File
1. Open `Upload Tracker.html`
2. Find the `firebaseConfig` object at the top of the script
3. Replace it with your actual Firebase config from step 3

Example config (replace with your actual values):
```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "your-project.firebaseapp.com",
  databaseURL: "https://your-project-default-rtdb.firebaseio.com",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

## Step 5: Deploy to GitHub Pages
1. Commit and push the updated HTML file to GitHub
2. Enable GitHub Pages in repository settings
3. Your site will be available at: `https://ruffyvrryal.github.io/UploadTracker`

## Step 6: Set Up Security Rules (Optional but Recommended)
Once you're comfortable, update the database rules for better security:

1. In Firebase console → "Realtime Database" → "Rules"
2. Replace with:
```json
{
  "rules": {
    "data": {
      "$uid": {
        ".read": true,
        ".write": "root.child('data').child($uid).child('day').val() === now / 86400 || !root.child('data').child($uid).exists()",
        ".validate": "newData.hasChildren(['apps', 'done', 'day'])"
      }
    }
  }
}
```

## How It Works
- Each device gets a unique ID stored locally
- Your app data syncs to Firebase under `data/[device-id]`
- Changes on one device instantly appear on others
- Data resets daily automatically

## Troubleshooting
- **Data not syncing**: Check Firebase config is correct
- **Authentication errors**: Ensure database is in Test mode or rules are configured
- **Changes not appearing**: Check browser console for errors (F12)
