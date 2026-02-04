# Firebase Hosting Deployment Guide

This guide explains how to deploy the Sydnie Hutchinson NC House website to Firebase Hosting.

## Prerequisites

1. A Firebase project (create one at https://console.firebase.google.com/)
2. Firebase CLI installed locally (for manual deployments)
3. GitHub repository secrets configured (for automatic deployments)

## Firebase Project Setup

### 1. Create a Firebase Project

1. Go to https://console.firebase.google.com/
2. Click "Add project" or select an existing project
3. Name your project `sydniehutchinsonnchouse` (or update `.firebaserc` with your project ID)
4. Follow the setup wizard to complete project creation

### 2. Enable Firebase Hosting

1. In the Firebase Console, navigate to your project
2. Click on "Hosting" in the left sidebar
3. Click "Get Started" and follow the setup instructions

## Manual Deployment

### Install Firebase CLI

```bash
npm install -g firebase-tools
```

### Login to Firebase

```bash
firebase login
```

### Deploy to Firebase Hosting

```bash
firebase deploy --only hosting
```

## Automatic Deployment with GitHub Actions

The repository includes a GitHub Actions workflow that automatically deploys to Firebase Hosting when changes are pushed to the `main` branch.

### Setup GitHub Secrets

1. **Generate a Firebase Service Account Key:**
   
   ```bash
   firebase login:ci
   ```
   
   This will generate a token. However, for GitHub Actions, it's recommended to use a service account:
   
   a. Go to Firebase Console > Project Settings > Service Accounts
   b. Click "Generate New Private Key"
   c. Save the JSON file securely

2. **Add the Service Account to GitHub Secrets:**
   
   a. Go to your GitHub repository
   b. Navigate to Settings > Secrets and variables > Actions
   c. Click "New repository secret"
   d. Name: `FIREBASE_SERVICE_ACCOUNT`
   e. Value: Paste the entire contents of the service account JSON file
   f. Click "Add secret"

### Workflow Trigger

The workflow is triggered:
- Automatically on every push to the `main` branch
- Manually via the "Actions" tab in GitHub (using `workflow_dispatch`)

## Configuration Files

### `firebase.json`

This file configures Firebase Hosting settings:
- **public**: Root directory for hosting files (set to `.` for the repository root)
- **ignore**: Files and directories to exclude from deployment
- **rewrites**: URL rewriting rules (currently set to serve `index.html` for all routes)
- **headers**: Cache-Control headers for static assets

### `.firebaserc`

This file specifies the Firebase project ID for deployment. Update the `default` project if you're using a different project name.

## Custom Domain Setup (Optional)

If you want to use the custom domain `sydniehutchinsonnchouse.com`:

1. In Firebase Console, go to Hosting > Add custom domain
2. Enter your domain name
3. Follow the instructions to verify domain ownership
4. Update your domain's DNS records with the provided values
5. Wait for SSL certificate provisioning (can take up to 24 hours)

## Testing the Deployment

After deployment, your site will be available at:
- Firebase Hosting URL: `https://sydniehutchinsonnchouse.web.app`
- Custom domain (if configured): `https://sydniehutchinsonnchouse.com`

## Troubleshooting

### Deployment fails with authentication error

- Verify that the `FIREBASE_SERVICE_ACCOUNT` secret is correctly set in GitHub
- Ensure the service account has the necessary permissions in Firebase

### Custom domain not working

- Verify DNS records are correctly configured
- Wait for DNS propagation (can take up to 48 hours)
- Check SSL certificate status in Firebase Console

### Files not updating after deployment

- Clear your browser cache
- Use an incognito/private browsing window
- Check that the correct files are being deployed (not ignored in `firebase.json`)

## Additional Resources

- [Firebase Hosting Documentation](https://firebase.google.com/docs/hosting)
- [GitHub Actions for Firebase](https://github.com/FirebaseExtended/action-hosting-deploy)
- [Firebase CLI Reference](https://firebase.google.com/docs/cli)
