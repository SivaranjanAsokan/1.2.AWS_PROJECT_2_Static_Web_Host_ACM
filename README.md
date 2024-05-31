# 🚀 Step-by-Step Guide to Hosting a Website on AWS

![image](https://github.com/SivaranjanAsokan/AWS-PROJECT_STATIC_WEB_HOST_ACM/assets/163242501/263249c4-65fc-4431-bacc-6fdc3dd95a2e)


## 🛠️ Prerequisites
- AWS Account
- Domain name registered (can be with AWS or another provider)

### 1. 🎒 Upload Your Website to S3

1. **Create an S3 Bucket**:
   - Go to the S3 console.
   - Click on "Create bucket".
   - Enter a unique bucket name (this should match your domain name for ease).
   - Choose the region closest to your audience.
   - Click "Create bucket".

2. **Upload Your Website Files**:
   - Click on your newly created bucket.
   - Go to the "Permissions" tab, scroll to the "Bucket Policy", and edit it with the following policy to make the bucket public:
     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Sid": "PublicReadGetObject",
           "Effect": "Allow",
           "Principal": "*",
           "Action": "s3:GetObject",
           "Resource": "arn:aws:s3:::your-bucket-name/*"
         }
       ]
     }
     ```
   - Upload your website files to this bucket.

### 2. 🌐 Set Up CloudFront

1. **Create a CloudFront Distribution**:
   - Go to the CloudFront console.
   - Click "Create Distribution".
   - Select "Web" for delivery method.
   - In the "Origin Domain Name", choose your S3 bucket from the dropdown.
   - In "Default Cache Behavior Settings", set the Viewer Protocol Policy to "Redirect HTTP to HTTPS".
   - Under "Distribution Settings", enter your domain name in the "Alternate Domain Names (CNAMEs)" section.
   - Click "Create Distribution".

2. **Attach ACM Certificate to CloudFront**:
   - Go to the ACM (AWS Certificate Manager) console.
   - Click "Request a certificate" and select "Request a public certificate".
   - Enter your domain name and any subdomains you want to include.
   - Choose DNS validation and follow the instructions to add a CNAME record to your DNS provider to validate the certificate.
   - Once validated, go back to your CloudFront distribution and attach the ACM certificate in the SSL/TLS settings.

### 3. 📍 Configure Route 53

1. **Create a Hosted Zone**:
   - Go to the Route 53 console.
   - Click "Create hosted zone".
   - Enter your domain name and click "Create".

2. **Add Alias Record for CloudFront**:
   - Inside your hosted zone, click "Create record".
   - Choose "A – IPv4 address" as the type.
   - Check "Alias" and then choose your CloudFront distribution from the dropdown.
   - Click "Create records".

3. **Add CNAME Record for ACM Validation**:
   - If you haven’t already added the CNAME record for ACM validation, add it now by clicking "Create record", choosing "CNAME" as the type, and entering the details provided by ACM.

### 4. 🌐 Update Nameservers with Your DNS Provider

- Go to your domain registrar's website.
- Find the DNS settings section.
- Update the nameservers to the ones provided by Route 53 (you'll find them listed in your hosted zone).

### 5. 🎉 Final Steps

- Wait for the DNS changes to propagate (this can take up to 48 hours, but usually much faster).
- Once propagated, navigate to your domain and you should see your website served over HTTPS via CloudFront.

---

By following these steps, you've successfully hosted your website on a public domain name using AWS S3, CloudFront, Route 53, and ACM! Enjoy your new, fast, and secure website! 🌟
