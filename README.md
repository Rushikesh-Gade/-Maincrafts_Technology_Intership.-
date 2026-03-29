# Static Portfolio — AWS S3 + CloudFront (HTTPS)

**Maincrafts Technology Internship | Task 1**

A responsive single-page portfolio hosted on AWS S3 and served via CloudFront with HTTPS.

---

## Live URL

> Replace with your CloudFront URL after deployment  
> `https://xxxxxxxx.cloudfront.net`

---

## Project Structure

```
├── index.html       # Main portfolio page
├── styles.css       # All styles
├── assets/          # Images and static assets
└── README.md
```

---

## Steps Taken

### 1. Build the Site Locally
- Created `index.html` with sections: Hero, About, Projects, Contact, Footer
- Created `styles.css` with dark theme, responsive layout, Google Fonts (Inter + Space Grotesk), FontAwesome icons
- Committed and pushed each file to GitHub (`main` branch)

### 2. Create S3 Bucket
```bash
# Via AWS Console or CLI
aws s3 mb s3://rushikesh-portfolio --region us-east-1
```
- Bucket name: `rushikesh-portfolio`
- Region: `us-east-1`
- Enabled **Static website hosting** → Index document: `index.html`

### 3. Disable Block Public Access
- In S3 bucket settings → Permissions → Block Public Access → **Uncheck all** → Save

### 4. Add Bucket Policy (Allow Public Read)
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::rushikesh-portfolio/*"
    }
  ]
}
```

### 5. Upload Files to S3
```bash
aws s3 sync . s3://rushikesh-portfolio --exclude ".git/*" --exclude "README.md"
```

### 6. Test S3 Endpoint
- S3 website URL format:  
  `http://rushikesh-portfolio.s3-website-us-east-1.amazonaws.com`

### 7. Create CloudFront Distribution
- Origin: S3 bucket website endpoint
- Viewer Protocol Policy: **Redirect HTTP to HTTPS**
- Minimum TLS Version: **TLSv1.2_2021**
- Default Root Object: `index.html`
- Wait ~10–15 min for deployment

### 8. Verify HTTPS
- Visit the CloudFront domain: `https://xxxxxxxx.cloudfront.net`
- Confirm padlock (HTTPS) in browser

### 9. (Optional) Custom Domain via Route 53
- Create a CNAME or Alias record pointing your domain to the CloudFront distribution URL

---

## Skills Gained
- S3 static website hosting & bucket policies
- IAM public access configuration
- CloudFront CDN setup with HTTPS
- AWS CLI for file sync
- Git + GitHub version control

---

## Screenshots
> Add screenshots of:
> - S3 bucket with static hosting enabled
> - Bucket policy applied
> - CloudFront distribution (Enabled status)
> - Live site in browser (HTTPS)
