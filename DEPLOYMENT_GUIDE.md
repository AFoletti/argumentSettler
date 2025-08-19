# 🚀 GitHub Pages Deployment Guide

## Quick Start

### 1. Repository Setup
```bash
# Clone or fork this repository
git clone https://github.com/yourusername/argumentsettler-dashboard.git
cd argumentsettler-dashboard

# Update homepage in package.json to match your repository
# Replace "yourusername" with your actual GitHub username
```

### 2. Configure GitHub Secrets
1. Go to your repository **Settings** → **Secrets and Variables** → **Actions**
2. Click **New repository secret**
3. Name: `TOGGL_API_TOKEN`
4. Value: Your Toggl Track API token (get it from [Toggl Profile Settings](https://track.toggl.com/profile))

### 3. Enable GitHub Pages
1. Go to repository **Settings** → **Pages**
2. Source: **Deploy from a branch**
3. Branch: `gh-pages` (will be created automatically on first deploy)

### 4. Deploy to GitHub Pages
```bash
# Install dependencies
npm install

# Update the homepage URL in package.json
# Change: "homepage": "https://yourusername.github.io/argumentsettler-dashboard"
# To:     "homepage": "https://YOUR-ACTUAL-USERNAME.github.io/argumentsettler-dashboard"

# Build the project
npm run build

# Deploy to GitHub Pages (creates/updates gh-pages branch)
npm run deploy
```

## 🔧 Troubleshooting

### Issue 1: Favicon Not Found
**Fixed** ✅ - We've added a proper favicon.ico file to the public folder.

### Issue 2: GitHub Action 403 Error
**Fixed** ✅ - The workflow now includes proper permissions:
```yaml
permissions:
  contents: write  # Required to push to repository
```

### Common Issues:

#### GitHub Action Still Failing?
1. **Check Repository Visibility**: If your repo is private, make sure GitHub Actions are enabled
2. **Verify API Token**: Ensure `TOGGL_API_TOKEN` secret is set correctly in repository settings
3. **Branch Permissions**: Make sure the default branch allows Actions to write

#### Data Not Updating?
1. Check the **Actions** tab for workflow run status
2. Verify the API token has access to your Toggl workspace
3. Check if the workflow is scheduled properly (should run daily at 6 AM UTC)

#### Deployment Issues?
1. Update the `homepage` field in `package.json` to match your repository URL exactly
2. Make sure GitHub Pages is enabled and set to deploy from `gh-pages` branch
3. Check the **Actions** tab to see if deployment succeeded

## 🔄 Manual Testing

### Test GitHub Action Locally:
```bash
# Set environment variables
export TOGGL_API_TOKEN="your_token_here"
export TOGGL_WORKSPACE="DRE-P"

# Run the data fetch script
python scripts/fetch-toggl-data.py
```

### Test Static Site Locally:
```bash
# Build the project
npm run build

# Copy data to build folder
cp -r data build/

# Serve locally
cd build && python3 -m http.server 8080
# Visit: http://localhost:8080
```

## 🔄 Automatic Data Updates

### GitHub Action Configuration
The workflow file `.github/workflows/fetch-toggl-data.yml` automatically:
- Runs daily at 6:00 AM UTC
- Fetches your latest Toggl data (last 30 days)
- Updates `data/metrics.json`
- Commits the changes to your repository

### Manual Data Update
You can trigger the data fetch manually:
1. Go to **Actions** tab in your repository
2. Click **Fetch Toggl Data Daily**
3. Click **Run workflow**

## 📁 Project Structure
```
argumentsettler-dashboard/
├── .github/workflows/          # GitHub Actions
│   └── fetch-toggl-data.yml   # Daily data fetch workflow
├── data/                       # Data directory (auto-updated)
│   └── metrics.json           # Toggl metrics (updated daily)
├── scripts/                   # Utility scripts
│   └── fetch-toggl-data.py   # Python script to fetch Toggl data
├── src/                      # React source code
│   ├── App.js               # Main dashboard component
│   ├── App.css              # Component styles
│   └── index.js             # App entry point
├── public/                   # Static assets
└── package.json             # Dependencies and scripts
```

## 🔧 Local Development
```bash
# Start development server
npm start

# Test data fetching (requires environment variables)
TOGGL_API_TOKEN=your_token TOGGL_WORKSPACE=your_workspace python3 scripts/fetch-toggl-data.py

# Build for production
npm run build

# Test production build locally
cd build && python3 -m http.server 8080
```

## 🏷️ Customization

### Update Workspace Name
Edit `fetch-toggl-data.yml` and `fetch-toggl-data.py`:
```yaml
env:
  TOGGL_WORKSPACE: "YOUR-WORKSPACE-NAME"  # Change this
```

### Modify Metrics
Edit `scripts/fetch-toggl-data.py` to add/remove metrics or change the calculation logic.

### UI Customization
- Edit `src/App.js` for layout and functionality changes
- Edit `src/App.css` and `tailwind.config.js` for styling
- Update `public/index.html` for meta tags and title

## 🎯 Dashboard Features

✅ **Total Billable Hours** - All billable time in last 30 days  
✅ **Time Away from Home** - Hours without "HomeOffice" tag  
✅ **Commute Statistics** - Back home times from "Commuting" entries  
✅ **HomeOffice End Times** - When you finish working from home  
✅ **Late Work Frequency** - Work sessions after 20:00  
✅ **Responsive Design** - Works on all devices  
✅ **Auto-refresh** - Data updates daily via GitHub Actions  

## 🔒 Security Notes

- API token is stored securely in GitHub Secrets
- No sensitive data in the codebase
- Static site with no backend vulnerabilities
- Data is fetched server-side via GitHub Actions

## 📱 Access Your Dashboard

Once deployed, your dashboard will be available at:
`https://yourusername.github.io/argumentsettler-dashboard`

## 🐛 Troubleshooting

### Data Not Loading
1. Check if `data/metrics.json` exists in your repository
2. Verify GitHub Action ran successfully (Actions tab)
3. Ensure `TOGGL_API_TOKEN` secret is set correctly

### Deployment Issues
1. Make sure `homepage` in `package.json` matches your repository URL
2. Check if `gh-pages` branch was created
3. Verify GitHub Pages is enabled in repository settings

### GitHub Action Failing
1. Check if `TOGGL_API_TOKEN` secret exists
2. Verify the token has access to your workspace
3. Review Action logs for specific error messages

---

**Ready to settle some arguments with data? 📊**