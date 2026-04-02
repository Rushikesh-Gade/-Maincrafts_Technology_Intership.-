# Static Portfolio — AWS S3 + CloudFront (HTTPS)

**Maincrafts Technology Internship | Task 1**

A responsive single-page portfolio hosted on AWS S3 and served via CloudFront with HTTPS.

---

## Live URL

> `https://de2iprp8jlj33.cloudfront.net`

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
aws s3 mb s3://rushikesh-gade-portfolio --region ap-south-1
```
- Bucket name: `rushikesh-gade-portfolio`
- Region: `ap-south-1` (Mumbai)
- Enabled Static website hosting → Index document: `index.html`

### 3. Disable Block Public Access
- S3 bucket → Permissions → Block Public Access → Uncheck all → Save

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
      "Resource": "arn:aws:s3:::rushikesh-gade-portfolio/*"
    }
  ]
}
```

### 5. Upload Files to S3
```bash
aws s3 sync . s3://rushikesh-gade-portfolio --exclude ".git/*" --exclude "*.json" --region ap-south-1
```

### 6. Test S3 Endpoint
```
http://rushikesh-gade-portfolio.s3-website.ap-south-1.amazonaws.com
```

### 7. Create CloudFront Distribution
- Origin: S3 bucket website endpoint
- Viewer Protocol Policy: Redirect HTTP to HTTPS
- Minimum TLS Version: TLSv1.2_2021
- Default Root Object: `index.html`
- Wait ~10-15 min for deployment

### 8. Verify HTTPS
- Visit: `https://xxxxxxxx.cloudfront.net`
- Confirm padlock (HTTPS) in browser

### 9. (Optional) Custom Domain via Route 53
- Create a CNAME or Alias record pointing your domain to the CloudFront distribution URL

---

## Skills Gained
- S3 static website hosting and bucket policies
- IAM public access configuration
- CloudFront CDN setup with HTTPS
- AWS CLI for file sync
- Git + GitHub version control

---

## Screenshots

### S3 Bucket Static Hosting Configuration
![S3 Bucket Static Hosting](assets/S3%20Bucket%20Static%20Hosting%20Configuration.png)

### S3 Bucket Policy
![S3 Bucket Policy](assets/S3%20Bucket%20Policy.png)

### CloudFront Distribution (Enabled Status)
![CloudFront Distribution](assets/CloudFront%20Distribution%20(Enabled%20Status).png)

### Live Site in Browser (HTTPS)
![Live Site HTTPS](assets/Live%20site%20in%20browser%20(HTTPS).png)
