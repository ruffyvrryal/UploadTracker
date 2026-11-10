# Supabase Setup Guide for Upload Tracker

Your app now syncs data across devices using Supabase (PostgreSQL + Real-time). Follow these steps:

## Step 1: Create a Supabase Project
1. Go to https://supabase.com
2. Click "Sign In" and create an account (or log in)
3. Click "New Project"
4. Enter project name: `upload-tracker`
5. Create a strong password
6. Choose your region (closest to you)
7. Click "Create new project" and wait for it to initialize

## Step 2: Create the Database Table
1. In Supabase dashboard, go to "SQL Editor" (left sidebar)
2. Click "New Query"
3. Copy and paste this SQL:

```sql
CREATE TABLE tracker_data (
  device_id TEXT PRIMARY KEY,
  data_state JSONB NOT NULL,
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Enable Row Level Security (RLS)
ALTER TABLE tracker_data ENABLE ROW LEVEL SECURITY;

-- Allow anyone to read and write (for development)
CREATE POLICY "Allow public access" ON tracker_data
  FOR ALL USING (true) WITH CHECK (true);

-- Enable Realtime
ALTER TABLE tracker_data REPLICA IDENTITY FULL;
```

4. Click "Run"
5. Verify the table was created (you should see it in the "Tables" section)

## Step 3: Enable Realtime
1. Go to "Database" → "Tables" → "tracker_data"
2. Click "Realtime" button (top right)
3. Toggle it ON

## Step 4: Get Your Supabase Keys
1. Click the gear icon (⚙️) → "API Settings"
2. Copy your:
   - **Project URL** (looks like: `https://your-project.supabase.co`)
   - **Anon Public Key** (under "Project API keys")

## Step 5: Update the HTML File
1. Open `Upload Tracker.html`
2. Find this at the top of the script:
```javascript
const SUPABASE_URL = "https://your-project.supabase.co";
const SUPABASE_KEY = "your-anon-key";
```

3. Replace with your actual values from Step 4

## Step 6: Deploy to GitHub Pages
1. Save the updated HTML file
2. Commit and push:
```bash
git add .
git commit -m "Update Supabase configuration"
git push
```
3. Enable GitHub Pages in repository settings (if not already done)
4. Your site will be available at: `https://ruffyvrryal.github.io/UploadTracker`

## How It Works
- Each device gets a unique ID stored locally
- Your app data syncs to Supabase under the `tracker_data` table
- Changes on one device instantly appear on others in real-time
- Data resets daily automatically

## Features
✅ Real-time sync across devices  
✅ PostgreSQL database (reliable & scalable)  
✅ Free tier: 500MB storage + real-time features  
✅ Works offline (syncs when back online)  

## Troubleshooting
- **Data not syncing**: Check if URL and key are correct in the HTML file
- **Realtime not working**: Make sure you enabled Realtime for the table
- **Permission errors**: Ensure RLS policy allows public access
- **Check console errors**: Press F12 in browser to see detailed error messages

## Security (Optional)
For production, replace the RLS policy with user authentication. See Supabase docs: https://supabase.com/docs/guides/auth
