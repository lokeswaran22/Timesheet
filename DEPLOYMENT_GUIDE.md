# Complete Deployment Guide - GitHub + Render

## 🎯 Overview
This guide covers deploying your Timesheet application to:
1. **GitHub** (code repository)
2. **Render.com** (live hosting)

---

# Part 1: Deploy to GitHub (Git Desktop)

## Step 1: Open Git Desktop
1. Open **GitHub Desktop** application
2. Select the **Timesheet** repository
3. Confirm you're on the `main` branch

## Step 2: Review Changes
You should see these modified files:
- ✅ `MERGE_CONFLICTS_RESOLVED.md`
- ✅ `GITHUB_DEPLOYMENT_GUIDE.md`
- ✅ `script.js`, `style.css`, `server-sqlite.js` (conflict resolutions)
- ✅ `Timeslot/` directory (optional - can exclude)

## Step 3: Commit Your Changes

**In Git Desktop:**

1. **Summary** (required):
   ```
   Fix merge conflicts and prepare for Render deployment
   ```

2. **Description** (optional):
   ```
   - Resolved all merge conflicts in CSS, JS, and HTML
   - Updated PORT to use process.env.PORT for Render
   - Fixed employee view centering
   - Updated package.json start script
   - Ready for production deployment
   ```

3. Click **"Commit to main"**

## Step 4: Push to GitHub

1. After commit, click **"Push origin"** button
2. Wait for upload to complete
3. You'll see "Successfully pushed" message

## Step 5: Verify on GitHub

1. Go to: `https://github.com/lokeswaran22/Timesheet`
2. Verify latest commit appears
3. Check timestamp matches your commit

✅ **GitHub deployment complete!**

---

# Part 2: Deploy to Render.com

## Prerequisites Check

Before deploying to Render, verify these files are correct:

### ✅ `package.json` - Start Script
```json
{
  "scripts": {
    "start": "node server-sqlite.js"
  }
}
```

### ✅ `server-sqlite.js` - PORT Configuration
```javascript
const PORT = process.env.PORT || 3005;
```

### ✅ Database Warning
⚠️ **Important**: SQLite on Render's free tier will reset on every deployment. For persistent data, you need:
- Render's paid persistent disk ($7/month)
- OR migrate to PostgreSQL (free on Render)

---

## Render Deployment Steps

### Option A: New Deployment (First Time)

#### Step 1: Create Render Account
1. Go to: https://render.com
2. Sign up or log in
3. Click **"New +"** → **"Web Service"**

#### Step 2: Connect GitHub Repository
1. Click **"Connect GitHub"**
2. Authorize Render to access your repositories
3. Select **"lokeswaran22/Timesheet"**
4. Click **"Connect"**

#### Step 3: Configure Web Service

Fill in these settings:

| Setting | Value |
|---------|-------|
| **Name** | `timesheet-app` (or your choice) |
| **Region** | Choose closest to you |
| **Branch** | `main` |
| **Root Directory** | Leave blank |
| **Runtime** | `Node` |
| **Build Command** | `npm install` |
| **Start Command** | `npm start` |
| **Instance Type** | `Free` |

#### Step 4: Environment Variables (Optional)
If you need any environment variables, add them in the "Environment" section:
- Click **"Add Environment Variable"**
- Example: `NODE_ENV` = `production`

#### Step 5: Deploy
1. Click **"Create Web Service"**
2. Render will start building your app
3. Watch the deployment logs
4. Wait for "Live" status (usually 2-5 minutes)

#### Step 6: Get Your Live URL
Once deployed, you'll get a URL like:
```
https://timesheet-app.onrender.com
```

---

### Option B: Existing Deployment (Update)

If you already have a Render service:

#### Automatic Deployment (Recommended)
1. Push to GitHub (already done in Part 1)
2. Render auto-detects the push
3. Automatically rebuilds and deploys
4. Check deployment logs in Render dashboard

#### Manual Deployment
1. Go to Render dashboard
2. Select your **Timesheet** service
3. Click **"Manual Deploy"** → **"Deploy latest commit"**
4. Wait for deployment to complete

---

## 🔍 Verify Render Deployment

### Check Deployment Logs
1. In Render dashboard, click your service
2. Go to **"Logs"** tab
3. Look for:
   ```
   🚀 Server is running on SQLite!
   📍 Local access:
      http://localhost:XXXX
   ```

### Test Your Live App
1. Click the URL at top of Render dashboard
2. Test login with:
   - Admin: `admin` / `admin123`
   - Employee: `autotest` / `password`
3. Verify all features work

---

## ⚠️ Important Render Considerations

### 1. SQLite Data Persistence
**Problem**: Free tier resets database on every deploy

**Solutions**:
- **Option A**: Add persistent disk ($7/month)
  - Go to service → Storage
  - Add persistent disk at `/opt/render/project/src`
  
- **Option B**: Migrate to PostgreSQL (free)
  - Create PostgreSQL database on Render
  - Update code to use PostgreSQL
  - More reliable for production

### 2. Cold Starts
Free tier services "spin down" after 15 minutes of inactivity:
- First request after idle takes 30-60 seconds
- Subsequent requests are fast
- Upgrade to paid tier for always-on

### 3. Build Time
- First deployment: 3-5 minutes
- Subsequent deployments: 1-3 minutes
- Render caches dependencies for faster builds

---

## 🎯 Complete Deployment Checklist

### GitHub ✅
- [ ] All conflicts resolved
- [ ] Changes committed in Git Desktop
- [ ] Pushed to GitHub successfully
- [ ] Latest commit visible on GitHub.com

### Render ✅
- [ ] Render account created
- [ ] GitHub repository connected
- [ ] Web service configured correctly
- [ ] Environment variables set (if needed)
- [ ] Deployment successful
- [ ] Live URL accessible
- [ ] Login works for admin and employee
- [ ] All features functional

---

## 🔧 Troubleshooting

### GitHub Push Fails
- Check internet connection
- Verify GitHub login in Git Desktop
- Try: Repository → Repository Settings → Remote

### Render Build Fails
**Check logs for:**
- Missing dependencies → Add to `package.json`
- Wrong start command → Verify `npm start` works locally
- Port issues → Ensure `process.env.PORT` is used

**Common fixes:**
```bash
# If build fails, check package.json
{
  "engines": {
    "node": ">=18.0.0"
  }
}
```

### Render App Crashes
1. Check logs in Render dashboard
2. Look for error messages
3. Common issues:
   - Database connection errors
   - Missing environment variables
   - Port binding issues

### Database Resets on Deploy
- This is expected on free tier
- Add persistent disk or migrate to PostgreSQL
- See "SQLite Data Persistence" section above

---

## 📊 Monitoring Your Deployment

### Render Dashboard
Monitor these metrics:
- **Status**: Should show "Live" (green)
- **CPU/Memory**: Check resource usage
- **Logs**: Real-time application logs
- **Events**: Deployment history

### GitHub
- **Commits**: Track deployment history
- **Actions**: CI/CD workflows (if configured)

---

## 🚀 Post-Deployment

### Share Your App
Your app is now live at:
```
https://timesheet-app.onrender.com
```

### Test Thoroughly
- [ ] Admin login and features
- [ ] Employee login and centering
- [ ] Excel export
- [ ] Logout functionality
- [ ] Clear history (admin only)
- [ ] Mobile responsiveness

### Monitor Performance
- Check Render logs regularly
- Monitor for errors
- Track user feedback

---

## 🎉 Success!

Your Timesheet application is now:
- ✅ Deployed to GitHub (version control)
- ✅ Live on Render.com (accessible worldwide)
- ✅ Auto-deploys on GitHub push
- ✅ Ready for production use

---

## 📞 Need Help?

- **Git Desktop**: https://docs.github.com/en/desktop
- **Render Docs**: https://render.com/docs
- **Render Support**: https://render.com/support
- **Community**: Render Discord or GitHub Discussions

---

## 🔄 Future Updates

To deploy updates:
1. Make changes locally
2. Test thoroughly
3. Commit in Git Desktop
4. Push to GitHub
5. Render auto-deploys (or manual deploy)
6. Verify on live URL

**That's it!** Your deployment workflow is now automated! 🎊
