# Cloud Week 1 – Static Site on AWS

## Live URL

* CloudFront: https://d167kq4tbo9bxw.cloudfront.net/

## Architecture

* Static HTML/CSS site stored in an Amazon S3 bucket with static website hosting enabled.
* S3 bucket policy grants public read (s3:GetObject) access so anyone can load the pages.
* Amazon CloudFront distribution uses the S3 website endpoint as origin and serves the site over HTTPS with caching at edge locations.

## Steps I followed

* Created an S3 bucket and enabled static website hosting with index.html as the root document.
* Disabled “Block all public access” for this bucket and added a bucket policy to allow public reads.
* Uploaded index.html, about.html, projects.html, and style.css to the bucket.
* Created a CloudFront distribution pointing at the S3 website endpoint and set index.html as the default root object.
* Fixed common issues: 404 NoSuchKey (wrong filenames), 504 Gateway Timeout (wrong origin settings), and CloudFront caching old content.





* Week 2 – EC2 Nginx web server (\[notes](WEEK2-EC2.md))
