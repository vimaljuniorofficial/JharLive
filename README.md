# 🎓 झार लाइव (Jhar Live) — Education Updates Portal

A fast, lightweight, and real-time education portal for Jharkhand students, job aspirants, and teachers. Featuring government job notifications (JSSC, JPSC), JAC board updates, admit cards, syllabus PDFs, model question papers, and an integrated in-browser Admin CMS with instant Firebase Realtime Database synchronization.

---

## 🚀 Quick Deploy to Netlify

You can deploy this portal to **Netlify** using any of the 3 simple methods below.

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy)

---

## 📋 Table of Contents

1. [Key Features](#-key-features)
2. [Method 1: Deploy with Git (Recommended)](#-method-1-deploy-with-git-recommended)
3. [Method 2: Deploy with Netlify Drop (Drag & Drop)](#-method-2-deploy-with-netlify-drop-no-git-required)
4. [Method 3: Deploy with Netlify CLI](#-method-3-deploy-with-netlify-cli)
5. [SPA Client-Side Routing & 404 Prevention](#-spa-client-side-routing--404-prevention)
6. [Firebase Realtime Database Setup & Security Rules](#-firebase-realtime-database-setup--security-rules)
7. [Admin Panel Access & Credentials](#-admin-panel-access--credentials)
8. [Custom Domain & SSL Setup](#-custom-domain--ssl-setup-on-netlify)
9. [Troubleshooting & FAQ](#-troubleshooting--faq)

---

## ✨ Key Features

- **⚡ Instant Real-Time Sync**: Pushing updates in Admin broadcasts changes to every connected visitor via WebSockets without page reload.
- **📱 Responsive & Native Look**: Optimized for mobile screens, tablets, and desktop with Hindi Devanagari typography.
- **🛠️ In-Browser Admin CMS**: Manage posts, categories, ads, custom blocks, links, and FAQs directly at `/admin`.
- **📊 Rich Post Formats**: Multi-column tables (2-col, 3-col), bullet points, action link buttons, callout boxes, and accordion FAQs.
- **💬 Social Channels Integration**: Direct WhatsApp Channel & Telegram Channel join badges and floating share buttons.
- **🔍 Fast Search & Filter**: Instant client-side search across all updates.
- **🎯 100% Jamstack & Netlify Compatible**: Pure static asset output with zero backend server overhead.

---

## 🌐 Method 1: Deploy with Git (Recommended)

This is the recommended continuous deployment method. Any push to your Git branch automatically triggers a fresh production build on Netlify.

### Step 1: Push your code to GitHub / GitLab / Bitbucket
Ensure your repository contains:
- `index.html` (portal application)
- `package.json` (build scripts & dependencies)
- `netlify.toml` (automated configuration)
- `public/_redirects` (fallback routing rule)

### Step 2: Connect to Netlify
1. Log in to [Netlify](https://app.netlify.com/).
2. Click **Add new site** → **Import an existing project**.
3. Select your Git provider (e.g., **GitHub**) and authorize Netlify.
4. Pick your repository from the list.

### Step 3: Configure Build Settings
Netlify will automatically detect the settings from `netlify.toml`. Verify the following values:

| Setting | Value |
| :--- | :--- |
| **Base directory** | *(leave empty / root)* |
| **Build command** | `npm run build` |
| **Publish directory** | `dist` |
| **Node.js Version** | `20` (or `18.x`+) |

> **Note:** Under **Site configuration** → **Environment variables**, you can set `NODE_VERSION` = `20` if it is not already set.

### Step 4: Click Deploy Site
Click **Deploy site**. Within 30–60 seconds, your portal will be live at `https://<random-name>.netlify.app`!

---

## 📦 Method 2: Deploy with Netlify Drop (No Git Required)

If you prefer deploying without connecting a Git repository:

### Option A: Build Locally and Drop `dist`
1. Open your terminal in the project folder.
2. Install dependencies (if not already done):
   ```bash
   npm install
   ```
3. Run the production build:
   ```bash
   npm run build
   ```
   This generates the optimized `dist` folder containing `index.html`, `_redirects`, and all assets.
4. Open [app.netlify.com/drop](https://app.netlify.com/drop) in your browser.
5. Drag and drop the **`dist`** folder into the browser window.
6. Your site goes live instantly!

---

## 💻 Method 3: Deploy with Netlify CLI

For developers who prefer terminal commands:

1. Install the Netlify CLI globally:
   ```bash
   npm install -g netlify-cli
   ```

2. Log in to your Netlify account:
   ```bash
   netlify login
   ```

3. Initialize the site in your project directory:
   ```bash
   netlify init
   ```
   Follow the prompts to link with an existing site or create a new one.

4. Build and deploy to production:
   ```bash
   netlify deploy --build --prod
   ```

---

## 🔀 SPA Client-Side Routing & 404 Prevention

This portal uses the browser's HTML5 History API (`history.pushState`) for smooth navigation between:
- Homepage: `/`
- Static Pages: `/about/`, `/terms/`, `/privacy/`
- Category Pages: `/category-slug/`
- Dynamic Post Pages: `/:post-slug/`
- Admin Panel: `/admin`

### The Problem without SPA Redirects
On static hosts like Netlify, refreshing the page while at `/about/` or `/admin` causes Netlify to look for a physical file named `about/index.html`. If that file does not exist, Netlify displays a **404 Page Not Found** error.

### The Solution (Already Included)
This repository includes **two fail-safe configuration files**:

1. **`netlify.toml`**:
   ```toml
   [[redirects]]
     from = "/*"
     to = "/index.html"
     status = 200
   ```

2. **`public/_redirects`**:
   ```
   /*    /index.html   200
   ```
   *Vite automatically copies the `public/_redirects` file into `dist/_redirects` during `npm run build`.*

With these rules in place, all page refreshes, direct links, and shared URLs load `index.html` with an HTTP 200 status, allowing the internal router to render the correct content.

---

## 🔥 Firebase Realtime Database Setup & Security Rules

The application uses Firebase Realtime Database to store and synchronize posts, categories, site settings, and ads across all devices in real-time.

### Default Built-in Firebase Config
The project comes pre-configured with a default Firebase project:
- **Project ID**: `jharlive-7fec3`
- **Database URL**: `https://jharlive-7fec3-default-rtdb.asia-southeast1.firebasedatabase.app`
- **Default Publish Key**: `Dumka@Jhar#2026xYz`

---

### Connecting Your Own Firebase Project (Optional)

If you wish to host the database on your own Firebase account:

1. Go to the [Firebase Console](https://console.firebase.google.com/) and create a project.
2. Create a **Realtime Database** (choose your preferred region, e.g., `asia-southeast1`).
3. Click on the **Rules** tab in Realtime Database and paste the following security rule:

```json
{
  "rules": {
    "content": {
      ".read": true,
      ".write": "newData.child('_pub_key').val() === 'Dumka@Jhar#2026xYz'"
    }
  }
}
```
*(Replace `Dumka@Jhar#2026xYz` with any secure key you choose).*

4. Go to **Project settings** (`⚙️`) → **General** → **Your apps** → **Web app (`</>`)**.
5. Copy the configuration credentials:
   - `apiKey`
   - `authDomain`
   - `databaseURL`
   - `projectId`
   - `storageBucket`
   - `messagingSenderId`
   - `appId`
6. Log in to your portal's Admin Panel (`https://your-site.netlify.app/admin`), navigate to **Settings** → **Firebase Configuration**, paste your values, set the matching **Publish Key**, and click **Save & Connect Firebase**.

---

## 🔐 Admin Panel Access & Credentials

The portal features a built-in admin dashboard:

### 1. Accessing Admin
Navigate to:
```
https://your-site.netlify.app/admin
```
*(Alternative URL: `https://your-site.netlify.app/#admin`)*

### 2. Default Login Credentials
- **Username**: `admin`
- **Password**: `admin123`

### 3. How to Publish Updates
1. Add or edit any Post, Category, or Static Page.
2. Click **Save Changes** (saves locally in browser cache).
3. Click the glowing **Publish** button with cloud icon in the top admin bar.
4. All visitors currently on your site will instantly see the update without refreshing!

### 4. Changing Admin Credentials
Inside the Admin Panel, go to **Settings** → **Admin Login Credentials**, enter your new username and password (minimum 4 characters), and click **Update Credentials** followed by **Publish**.

---

## 🏷️ Custom Domain & SSL Setup on Netlify

To point your custom domain (e.g. `jharlive.in` or `www.jharlive.in`) to Netlify:

1. In Netlify, open your site dashboard → **Domain management** → **Add a domain**.
2. Enter your domain name (e.g., `jharlive.in`).
3. Set up the DNS records at your domain registrar (GoDaddy, Namecheap, Cloudflare, etc.):
   - **For Apex Domain (`jharlive.in`)**:
     - Type: `A`
     - Name: `@`
     - Value: `75.2.60.5` *(Netlify Load Balancer)*
   - **For Subdomain (`www.jharlive.in`)**:
     - Type: `CNAME`
     - Name: `www`
     - Value: `<your-site-name>.netlify.app`
4. Netlify will automatically provision a free **Let's Encrypt SSL/TLS Certificate** for HTTPS security.

---

## 📂 Project Structure

```
├── netlify.toml               # Netlify build, routing, and header configuration
├── public/
│   └── _redirects             # Fallback SPA rewrite rule (/* -> /index.html 200)
├── index.html                 # Complete portal application (HTML + CSS + JS)
├── metadata.json              # Applet metadata and configuration
├── package.json               # Dependencies and build scripts
├── vite.config.ts             # Vite configuration
└── README.md                  # This documentation
```

---

## ❓ Troubleshooting & FAQ

### Q1: When I refresh `/about/` or `/admin`, I get a 404 error on Netlify.
**Fix**: Ensure `netlify.toml` is at the root of your repository and `public/_redirects` contains:
```
/*    /index.html   200
```
Then trigger a redeploy on Netlify.

### Q2: Clicking "Publish" in Admin returns "Permission Denied".
**Fix**: Ensure your Firebase Realtime Database rules match your Publish Key:
```json
{
  "rules": {
    "content": {
      ".read": true,
      ".write": "newData.child('_pub_key').val() === 'Dumka@Jhar#2026xYz'"
    }
  }
}
```
Check that the key in **Settings → Firebase Configuration → Publish Key** matches the rule exactly.

### Q3: How do I export or backup my website data?
**Fix**: Go to `/admin` → **Backup** tab → Click **Export Backup**. This downloads a full `.json` file containing all your posts, categories, settings, and pages. You can restore this data anytime using **Import Backup**.

### Q4: Build failed on Netlify with Node version error.
**Fix**: In Netlify under **Site configuration** → **Environment variables**, add:
- Key: `NODE_VERSION`
- Value: `20`

---

## 📄 License
This project is open-source and released under the **Apache-2.0 License**.
