# Deployment Guide for belenveraxyz-web

This guide walks you through deploying your new cloned website with a separate repository, domain, and Sanity management dashboard.

## Prerequisites

- GitHub account
- Vercel account
- Sanity account
- Domain name (optional, can use Vercel's free subdomain initially)

## Step 1: Create New Sanity Project

1. **Go to Sanity.io**
   - Visit https://www.sanity.io/manage
   - Click "Create project"
   - Choose a project name (e.g., "belenveraxyz")
   - Select a dataset name (use "production" or create a custom name)
   - Note your **Project ID** - you'll need this later

2. **Deploy Schemas**
   ```bash
   cd sanity
   npm install
   # Update sanity/.env with your new project ID:
   # SANITY_STUDIO_PROJECT_ID=your-new-project-id
   # SANITY_STUDIO_DATASET=production

   # Deploy the schemas
   npx sanity deploy
   ```

   This will deploy your simplified schemas (article, author, revista, featuredGallery) to the new Sanity project.

3. **Set up Sanity Studio**
   ```bash
   # Start Sanity Studio locally to test
   npm run dev
   ```

   Visit http://localhost:3333 to access your Sanity Studio interface.

## Step 2: Push to GitHub

1. **Create a new repository on GitHub**
   - Go to https://github.com/new
   - Name it `belenveraxyz-web` (or your preferred name)
   - **Do not** initialize with README, .gitignore, or license
   - Click "Create repository"

2. **Push your local repository**
   ```bash
   cd /Users/andybell/Downloads/efimera-site/belenveraxyz-web
   git remote add origin https://github.com/YOUR-USERNAME/belenveraxyz-web.git
   git branch -M main
   git push -u origin main
   ```

## Step 3: Deploy to Vercel

1. **Connect GitHub to Vercel**
   - Go to https://vercel.com/new
   - Import your `belenveraxyz-web` repository
   - Vercel will auto-detect it as an Astro project

2. **Configure Environment Variables**

   Add these environment variables in Vercel:

   - `SANITY_PROJECT_ID`: Your new Sanity project ID from Step 1
   - `SANITY_DATASET`: `production` (or your custom dataset name)

   Optional:
   - `PUBLIC_STAGING`: Set to `true` for staging deployments

3. **Deploy**
   - Click "Deploy"
   - Wait for the build to complete
   - You'll get a URL like `https://belenveraxyz-web.vercel.app`

## Step 4: Configure Custom Domain (Optional)

1. **In Vercel Dashboard**
   - Go to your project settings
   - Click "Domains"
   - Add your custom domain
   - Follow Vercel's DNS configuration instructions

2. **Update DNS Settings**
   - Add the DNS records provided by Vercel to your domain registrar
   - Wait for DNS propagation (can take up to 48 hours)

## Step 5: Configure Sanity CORS

1. **Go to Sanity Dashboard**
   - Visit https://www.sanity.io/manage
   - Select your project
   - Go to "API" settings

2. **Add CORS Origins**
   Add these origins:
   - `http://localhost:4321` (for local development)
   - `https://your-vercel-domain.vercel.app` (your Vercel URL)
   - `https://yourdomain.com` (your custom domain if you have one)

## Step 6: Set Up Sanity Studio Hosting

You have two options for hosting Sanity Studio:

### Option A: Deploy to Sanity's hosting
```bash
cd sanity
npx sanity deploy
```

This creates a URL like `https://your-project.sanity.studio`

### Option B: Host as part of your Astro site
Create a new Astro page that embeds Sanity Studio (more advanced setup).

## Step 7: Update Branding

Don't forget to customize these files for your new brand:

1. **Update site metadata**
   - Edit `src/layouts/Layout.astro`:
     - Change default title (line 12)
     - Change description (line 12)
     - Change footer copyright (line 263)

2. **Replace logos**
   - Replace `public/efimera-logo.svg` with your logo
   - Replace `public/favicon.svg` with your favicon

3. **Update social links**
   - Edit `src/layouts/Layout.astro` (lines 73, 88-134, 179-246)
   - Update email addresses
   - Update Instagram/LinkedIn URLs

4. **Update About/Contact pages**
   - Edit `src/pages/about.astro`
   - Edit `src/pages/contact.astro`
   - Edit `src/pages/privacy.astro`

## Step 8: Add Content

1. **Access Sanity Studio**
   - Go to your deployed Sanity Studio URL or run locally
   - Sign in with your Sanity credentials

2. **Create your first content**
   - Add authors
   - Create articles (remember: only 3 categories - Artículos, Entrevistas, Proyectos)
   - Upload revista PDFs if needed
   - Configure featured gallery for homepage

3. **Verify on your site**
   - Visit your deployed Vercel URL
   - Content should appear automatically

## Environment Variables Reference

### Root `.env` (for Astro)
```env
SANITY_PROJECT_ID=your-project-id-here
SANITY_DATASET=production
PUBLIC_STAGING=false
```

### `sanity/.env` (for Sanity Studio)
```env
SANITY_STUDIO_PROJECT_ID=your-project-id-here
SANITY_STUDIO_DATASET=production
```

## Continuous Deployment

Every time you push to the `main` branch on GitHub:
- Vercel automatically rebuilds and deploys your site
- Changes to content in Sanity are immediately reflected (with SSR)

## Troubleshooting

### Content not showing up
- Check that `SANITY_PROJECT_ID` is correct in Vercel
- Verify CORS settings in Sanity dashboard
- Check that you're using the correct dataset name

### Build errors on Vercel
- Check the build logs in Vercel dashboard
- Ensure all dependencies are in `package.json`
- Verify Node.js version compatibility

### Sanity Studio not loading
- Check that schemas were deployed: `cd sanity && npx sanity deploy`
- Verify project ID in `sanity/.env`
- Check browser console for CORS errors

## Key Differences from Original

This clone has been simplified with:
- ✅ 3 categories instead of 8 (Artículos, Entrevistas, Proyectos)
- ✅ No Prensa section (only Revista for PDFs)
- ✅ No related articles feature
- ✅ Same core features: articles, search, featured gallery, revista PDFs
- ✅ Same visual design and styling

## Next Steps

1. Customize branding and colors in `src/styles/global.css`
2. Add your content via Sanity Studio
3. Set up Google Analytics or other tracking (if needed)
4. Configure email newsletter (if needed)
5. Test thoroughly before announcing launch

## Support

For issues with:
- **Astro**: https://docs.astro.build
- **Sanity**: https://www.sanity.io/docs
- **Vercel**: https://vercel.com/docs
