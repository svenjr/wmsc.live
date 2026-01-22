# Setting up wmsc.live Domain with GitHub Pages

This guide will help you configure your wmsc.live domain to point to the GitHub Pages landing page.

## Step 1: Enable GitHub Pages

1. Go to your GitHub repository settings
2. Navigate to **Pages** (under "Code and automation")
3. Under "Build and deployment":
   - **Source**: Deploy from a branch
   - **Branch**: `main`
   - **Folder**: `/docs`
4. Click **Save**

The site will deploy automatically. The workflow file `.github/workflows/pages.yml` will handle deployments on future commits.

## Step 2: Add Custom Domain in GitHub

1. Still in the GitHub Pages settings, scroll to **Custom domain**
2. Enter: `wmsc.live`
3. Click **Save**
4. Check **Enforce HTTPS** (after DNS propagates)

This will create a `CNAME` file in the `/docs` folder.

## Step 3: Configure DNS at Your Domain Registrar

You need to add DNS records where you purchased wmsc.live. The configuration depends on whether you want `www.wmsc.live` or just `wmsc.live`:

### Option A: Apex Domain (wmsc.live)

Add these **A records** pointing to GitHub's IP addresses:

```
Type: A
Name: @
Value: 185.199.108.153

Type: A
Name: @
Value: 185.199.109.153

Type: A
Name: @
Value: 185.199.110.153

Type: A
Name: @
Value: 185.199.111.153
```

### Option B: With www subdomain (Recommended)

Add both the A records above PLUS:

```
Type: CNAME
Name: www
Value: svenjr.github.io
```

### Verify DNS Configuration

Add a `CNAME` record to verify your domain:

```
Type: CNAME
Name: _github-pages-challenge-svenjr
Value: (GitHub will provide this when you add the custom domain)
```

## Step 4: Wait for DNS Propagation

DNS changes can take anywhere from a few minutes to 48 hours to propagate worldwide. You can check the status with:

```bash
dig wmsc.live +noall +answer
```

You should see the GitHub IP addresses listed.

## Step 5: Enable HTTPS

Once DNS has propagated:

1. Go back to GitHub Pages settings
2. Check **Enforce HTTPS**
3. Wait a few minutes for the SSL certificate to be issued

## Verification

Once everything is set up, visiting `wmsc.live` should show your landing page with links to all your race tracking sites!

## Common Issues

### DNS not propagating
- Wait longer (up to 48 hours)
- Clear your browser cache
- Try accessing from a different device/network

### HTTPS not available
- Make sure DNS has fully propagated first
- Uncheck and re-check "Enforce HTTPS"
- Wait up to 24 hours for certificate issuance

### 404 errors
- Verify the custom domain is correctly set in GitHub Pages settings
- Make sure the CNAME file exists in `/docs`
- Check that the workflow has successfully deployed

## Reference

- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Managing a custom domain for GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)
