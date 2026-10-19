# Assignment 1: Host a Static Website on Amazon S3

## Objective
Host a static website in an Amazon S3 bucket and verify that it is accessible through a public website endpoint.

## Requirements
- AWS account and access to the AWS Management Console
- A static website file, such as `index.html`
- Optional: `error.html`

## Instructions

### 1. Sign in to AWS
1. Open the AWS Management Console and sign in.
2. Select the AWS Region you intend to use. The source practical uses **Asia Pacific (Mumbai), `ap-south-1`**.

### 2. Create an S3 bucket
1. Search for **S3** and open Amazon S3.
2. Select **Create bucket**.
3. Enter a globally unique bucket name, for example `mcoe-mca-aws-assign1` if it is available.
4. Keep the bucket in your chosen Region.
5. Under **Block Public Access**, configure the setting only if public access is required for this practical. A public website requires public read access, so review the warning and acknowledge it only if you understand the exposure.
6. Create the bucket.

### 3. Upload the website files
1. Open the bucket.
2. Choose **Upload**.
3. Select **Add files** or **Add folder** and choose `index.html` and any other website assets.
4. Select **Upload**.
5. Confirm that the files appear in the bucket's **Objects** tab.

### 4. Enable static website hosting
1. Open the bucket's **Properties** tab.
2. Find **Static website hosting** and select **Edit**.
3. Enable static website hosting.
4. Choose **Host a static website**.
5. Set **Index document** to `index.html`.
6. Optionally set **Error document** to `error.html`.
7. Save the changes.

### 5. Allow public read access
For the public website endpoint used in this practical, the website objects must be publicly readable. If your account permits a public bucket policy:
1. Open **Permissions → Bucket policy**.
2. Replace `YOUR_BUCKET_NAME` in the policy below with the exact bucket name.
3. Save the policy.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
    }
  ]
}
```

If AWS Block Public Access settings or an organization policy prevent public access, the policy may not work until the applicable settings are reviewed. Do not make private or sensitive files public. For production websites, consider a private S3 bucket behind Amazon CloudFront with Origin Access Control.

### 6. Test the website
1. Return to **Properties → Static website hosting**.
2. Copy the **Bucket website endpoint**.
3. Open the endpoint in a browser using the URL shown by AWS.
4. Confirm that the page from `index.html` loads.

## Expected result
The static website loads through the S3 website endpoint.

## Troubleshooting
- **403 Access Denied:** Check the bucket policy, Block Public Access settings, and object names.
- **404 Not Found:** Confirm `index.html` exists at the bucket root and the index document name matches exactly.
- **Website does not update:** Verify the uploaded files and refresh the browser.

## Cleanup and security
After the practical, remove public access if it is no longer needed and delete test objects/buckets when appropriate. Public bucket policies expose objects to anyone who has the endpoint.
