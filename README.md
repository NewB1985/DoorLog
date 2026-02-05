# DoorLog - Field Estimating App

A mobile-first Progressive Web App for locksmith estimators to capture door-by-door documentation during job walks.

## Quick Start

### Option 1: Direct Use (No Google Drive)
1. Open `index.html` in Safari on your iPhone/iPad
2. Tap the Share button → "Add to Home Screen"
3. The app now works offline like a native app
4. Use "Download ZIP" export option

### Option 2: With Google Drive Integration
Follow the Google Drive Setup below, then deploy to a web server.

---

## Google Drive Setup (One-Time)

To enable direct uploads to Google Drive, you need to create a Google Cloud project. This takes about 10 minutes.

### Step 1: Create a Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Click **Select a Project** → **New Project**
3. Name it "DoorLog" → Click **Create**
4. Wait for it to be created, then select it

### Step 2: Enable the Google Drive API

1. Go to **APIs & Services** → **Library**
2. Search for "Google Drive API"
3. Click on it → Click **Enable**

### Step 3: Configure OAuth Consent Screen

1. Go to **APIs & Services** → **OAuth consent screen**
2. Select **Internal** (if using Google Workspace) or **External**
3. Fill in:
   - App name: `DoorLog`
   - User support email: your email
   - Developer contact: your email
4. Click **Save and Continue**
5. On Scopes page, click **Add or Remove Scopes**
6. Find and select: `https://www.googleapis.com/auth/drive.file`
7. Click **Update** → **Save and Continue**
8. If External: Add your test users' emails
9. Click **Save and Continue** → **Back to Dashboard**

### Step 4: Create Credentials

1. Go to **APIs & Services** → **Credentials**
2. Click **Create Credentials** → **OAuth client ID**
3. Application type: **Web application**
4. Name: `DoorLog Web`
5. Under **Authorized JavaScript origins**, add:
   - `http://localhost:8080` (for testing)
   - Your production URL (e.g., `https://doorlog.yourcompany.com`)
6. Click **Create**
7. **Copy the Client ID** (looks like: `123456789-abc.apps.googleusercontent.com`)

### Step 5: Create API Key

1. Still in **Credentials**, click **Create Credentials** → **API Key**
2. **Copy the API Key**
3. (Optional but recommended) Click **Edit API Key**:
   - Under **API restrictions**, select **Restrict key**
   - Select only **Google Drive API**
   - Click **Save**

### Step 6: Update the App

1. Open `index.html` in a text editor
2. Find these lines near the top:
   ```javascript
   const GOOGLE_CONFIG = {
       CLIENT_ID: 'YOUR_CLIENT_ID.apps.googleusercontent.com', // Replace this
       API_KEY: 'YOUR_API_KEY', // Replace this
   ```
3. Replace `YOUR_CLIENT_ID.apps.googleusercontent.com` with your Client ID
4. Replace `YOUR_API_KEY` with your API Key
5. Save the file

### Step 7: Deploy

Google OAuth requires HTTPS in production. Options:

**For Testing (localhost):**
```bash
# Python
python3 -m http.server 8080

# Node.js
npx serve -p 8080
```

**For Production:**
- **GitHub Pages**: Free, automatic HTTPS
- **Netlify**: Drag & drop deployment
- **Firebase Hosting**: `firebase deploy`
- **Any HTTPS web server**

---

## Features

### ✅ Implemented

| Feature | Description |
|---------|-------------|
| **Job Management** | Create, view, delete jobs with auto-generated codes |
| **Auto Door IDs** | Format: `[JOB-CODE]-D01`, `D02`, etc. |
| **In-App Camera** | Photos captured inside app, auto-linked to doors |
| **Photo Watermarks** | Door ID burned into every photo |
| **Manual Notes** | Full text input with auto-save |
| **Voice Dictation** | Tap to start/stop, real-time transcription |
| **Quick Chips** | One-tap common phrases (Needs rekey, ADA issue, etc.) |
| **Door Labels** | Optional location labels (Front Entry, Suite 201) |
| **Offline Storage** | IndexedDB - works without internet |
| **Export to ZIP** | CSV + organized photo folders (download to device) |
| **Google Drive Export** | Upload directly to shared Drive folder |

### 🎯 Field-Optimized UX

- **Large touch targets** (60px+ buttons)
- **One-hand operation** (thumb-reachable actions)
- **High contrast dark theme** (outdoor visibility)
- **Zero typing required** (voice + chips)
- **Instant auto-save** (no save button)
- **Camera-first workflow** (new door → camera opens)

---

## Workflow

```
1. TAP "New Job" → Enter address
2. TAP "+ Add Door" → Camera opens automatically
3. SNAP photos → Tap "Done"
4. SPEAK notes OR tap quick chips OR type manually
5. REPEAT for each door
6. TAP "Export" → Choose ZIP download or Google Drive
```

## Export Options

### Option A: Download ZIP
- Works offline, no setup required
- Downloads to device
- Transfer to PC via AirDrop, email, or cable

### Option B: Google Drive
- Requires one-time setup (see above)
- Uploads directly to your company's shared folder
- Accessible immediately on any PC
- Creates organized folder structure automatically

## Export Folder Structure

```
Job-Name_2025-02-04/
├── DoorLog_Job-Name.csv
└── Photos/
    ├── 450M-D01_Front-Entry/
    │   ├── 450M-D01_01_2025-02-04-143052.jpg
    │   └── 450M-D01_02_2025-02-04-143055.jpg
    └── 450M-D02_Suite-201/
        └── 450M-D02_01_2025-02-04-143120.jpg
```

### CSV Columns
- Door ID
- Label
- Photo Count
- Photo Folder Path
- Notes
- Timestamp

---

## Browser Requirements

- **iOS Safari 15+** (recommended)
- **Chrome 90+**
- **Firefox 90+**

Required APIs:
- IndexedDB (offline storage)
- MediaDevices (camera)
- Web Speech API (voice dictation)
- Google APIs (for Drive export)

---

## Voice Dictation Tips

1. **Speak in short bursts** (5-10 seconds)
2. **Pause between sentences** for auto-punctuation
3. **Tap stop** before moving to next thought
4. **Use quiet moments** when possible
5. **Quick chips** work great for common phrases

---

## Customization

### Adding Quick Chips
Edit the `quickChips` array in the code:
```javascript
const quickChips = [
    'Needs rekey',
    'Replace hardware',
    // Add your common phrases here
];
```

### Changing Door ID Format
Edit the `generateJobCode` function to customize job codes.

---

## Troubleshooting

### Camera not working
- Check browser permissions
- iOS: Settings → Safari → Camera → Allow
- Try reloading the page

### Voice input not working
- Check microphone permissions
- iOS: Settings → Safari → Microphone → Allow
- Speak clearly, closer to mic

### Google Drive not connecting
- Check that CLIENT_ID and API_KEY are correct
- Ensure your domain is in Authorized JavaScript origins
- Check browser console for errors
- Try clearing cookies and re-authenticating

### "Access blocked" error
- Your Google Cloud project may need verification for external users
- Add test users in OAuth consent screen
- Or switch to "Internal" if using Google Workspace

### Data not saving
- Check available storage space
- Try clearing browser cache
- Re-add to Home Screen

---

## Security Notes

- **Local data**: All job data stays on-device until exported
- **Google Drive**: Only uploads to YOUR Drive account
- **Permissions**: App only requests access to files it creates (drive.file scope)
- **No backend**: No data sent to any server (except Google when you choose Drive export)

---

## Technical Stack

- **React 18** (via CDN)
- **IndexedDB** (offline storage)
- **JSZip** (ZIP export generation)
- **Web Speech API** (dictation)
- **MediaDevices API** (camera)
- **Google Drive API** (cloud export)
- **Google Identity Services** (OAuth)

---

## License

Internal business tool - use as needed.
