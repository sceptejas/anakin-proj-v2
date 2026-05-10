# 🔧 Gemini API Error - Troubleshooting

## What's Happening

The Gemini API call is failing with an error. This could be due to:
1. Invalid API key
2. API key lacks permissions
3. Rate limit exceeded
4. Wrong API endpoint

## ✅ What I Fixed

### 1. **Better Error Logging**
Now shows:
```
📡 [GEMINI REQUEST] Sending prompt...
🔑 [GEMINI KEY] Present (AIzaSyA95x...)
📥 [GEMINI RESPONSE] 400 Bad Request
❌ [GEMINI ERROR] 400 {error details}
💡 [HINT] Bad request - check API key or request format
```

### 2. **Improved Fallback**
If Gemini fails, the app now:
- Shows a basic assessment from the raw data
- Displays a helpful message about the API error
- Still provides a usable (though less witty) result
- Tells you to check your API key

### 3. **Specific Error Messages**
- **400**: Bad request - check API key or request format
- **403**: API key invalid or lacks permissions
- **429**: Rate limit exceeded - wait a moment

---

## 🔍 Debugging Steps

### Step 1: Check Console Logs

After running a scan, look for:
```
📡 [GEMINI REQUEST] Sending prompt...
🔑 [GEMINI KEY] Present (AIzaSyA95x...)
📥 [GEMINI RESPONSE] 400 Bad Request
❌ [GEMINI ERROR] 400 {full error message}
```

The error message will tell you what's wrong.

### Step 2: Common Issues

#### Issue 1: "API key not valid"
**Solution**: Get a new API key
1. Go to: https://aistudio.google.com/apikey
2. Delete old key
3. Create new key
4. Copy to `.env.local`:
   ```bash
   NEXT_PUBLIC_GEMINI_API_KEY=your_new_key_here
   ```
5. Restart dev server

#### Issue 2: "API key lacks permissions"
**Solution**: Enable Gemini API
1. Go to: https://console.cloud.google.com/apis/library/generativelanguage.googleapis.com
2. Click "Enable"
3. Wait a few minutes
4. Try again

#### Issue 3: "Rate limit exceeded"
**Solution**: Wait a moment
- Free tier: 15 requests per minute
- Wait 1 minute and try again

#### Issue 4: "Quota exceeded"
**Solution**: Check your quota
1. Go to: https://aistudio.google.com/
2. Check "API usage" or "Quota"
3. Free tier: 1,500 requests per day
4. If exceeded, wait until tomorrow or upgrade

---

## 🧪 Test Your API Key

### Quick Test in Browser Console

1. Open browser console (F12)
2. Paste this code:

```javascript
const GEMINI_KEY = 'AIzaSyA95xMtZ_22w7He4UtFlIDXC1EQ69SAI_g';
const GEMINI_URL = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${GEMINI_KEY}`;

fetch(GEMINI_URL, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    contents: [{ parts: [{ text: 'Say hello in JSON format: {"message": "your message"}' }] }]
  })
})
.then(r => r.json())
.then(d => console.log('✅ SUCCESS:', d))
.catch(e => console.error('❌ ERROR:', e));
```

3. Check the result:
   - ✅ **SUCCESS**: API key works!
   - ❌ **ERROR**: API key has issues

---

## 🎯 What Happens Now

### If Gemini Works:
```
📡 [GEMINI REQUEST] Sending prompt...
📥 [GEMINI RESPONSE] 200 OK
📦 [GEMINI RESPONSE] {candidates: [...]}
📝 [RAW TEXT] {"vibe_title": "..."}
✅ [GEMINI PARSED] {vibe_title: "...", vibe_score: 67, ...}
→ Shows witty vibe dossier
```

### If Gemini Fails (Fallback):
```
📡 [GEMINI REQUEST] Sending prompt...
📥 [GEMINI RESPONSE] 400 Bad Request
❌ [GEMINI ERROR] 400 API key not valid
💡 [HINT] Bad request - check API key
❌ [GEMINI FAILED] Error: Gemini API error: 400
🔄 [USING FALLBACK] Generating fallback result...
→ Shows basic assessment with message about API error
```

---

## 📊 Fallback Result Example

If Gemini fails, you'll see:

```
// VIBE SIGNATURE //
"Digital Presence Detected"

Their whole thing in one line:
"Exists online but personality unclear"

// VIBE SCORE //
DATE COMPATIBILITY: 55%
████████████░░░░░░░░░░░░  55%
🟡 Proceed with caution

// FULL VIBE BREAKDOWN //
This person maintains an online presence across the 
platforms we scanned. Based on the available data, 
they appear to be a real human with actual interests 
and opinions. The Gemini AI analysis failed, so this 
is a basic assessment from the raw data. For a full 
personality breakdown, check your Gemini API key and 
try again.

// FLAGS REPORT //
🚩 RED FLAGS              ✅ GREEN FLAGS
• Gemini analysis         • Has an actual online
  unavailable (API error)   presence
• Limited personality     • Real person confirmed
  insights from raw data

// THE VERDICT //
SWIPE DECISION:
The data shows they're a real person, but without 
Gemini's analysis, it's hard to say if you'd vibe. 
Check your API key and try again for the full verdict.
```

---

## 🔑 Get a Fresh API Key

### Step-by-Step:

1. **Go to Google AI Studio**
   - URL: https://aistudio.google.com/apikey

2. **Sign in with Google**

3. **Create API Key**
   - Click "Create API Key"
   - Select "Create API key in new project" (or use existing)
   - Copy the key

4. **Add to .env.local**
   ```bash
   NEXT_PUBLIC_GEMINI_API_KEY=paste_your_new_key_here
   ```

5. **Restart Dev Server**
   ```bash
   cd ex-ray-vision
   npm run dev
   ```

6. **Test Again**
   - Run a scan
   - Check console for `✅ [GEMINI PARSED]`

---

## 💡 Pro Tips

### Tip 1: Check API Key Format
Valid format: `AIzaSy...` (starts with `AIzaSy`)

### Tip 2: Enable Gemini API
Some Google accounts need to explicitly enable the API:
- Go to: https://console.cloud.google.com/
- Search for "Generative Language API"
- Click "Enable"

### Tip 3: Check Quota
Free tier limits:
- 15 requests per minute
- 1,500 requests per day
- 1 million tokens per day

### Tip 4: Use Fallback
Even if Gemini fails, the app still works! You'll get a basic assessment instead of a witty one.

---

## 🚀 Next Steps

1. **Check console logs** - See the exact error
2. **Get fresh API key** - From https://aistudio.google.com/apikey
3. **Update .env.local** - Paste new key
4. **Restart server** - `npm run dev`
5. **Test again** - Run a scan and check console

**The app now has a working fallback, so you'll always get results even if Gemini fails!** 🎯
