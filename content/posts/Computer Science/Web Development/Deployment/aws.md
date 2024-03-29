+++
title = 'Deploying with AWS'
date = 2024-03-23T10:36:51+10:00
draft = false
+++

# Front End
Easily host the any front-end applications as static files on an S3 bucket. A BucketDeployment construct can be used to help you automate uploading your files.

Then a Cloudfront distribution can be used to serve your files.

If you want a custom URL, you may want to use Route53. https://dev.to/paulallies/deploy-your-static-react-app-to-aws-cloudfront-using-cdk-44hm


