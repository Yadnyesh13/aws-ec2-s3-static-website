# EC2 + S3 Static Website Setup

ssh -i /path/to/your-key-pair.pem ec2-user@your-ec2-public-dns

sudo yum update -y
sudo yum install git -y
git --version
git config --global user.name "your-username"
git config --global user.email "your-email"

sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd

sudo usermod -a -G apache ec2-user
sudo chmod 755 /var/www/html

cd /var/www/html
sudo touch index.html
sudo nano index.html

# Go to AWS Console → S3 → Create Bucket
# Create a bucket named: your-bucket-name

# Go to S3 → your-bucket-name → Permissions → Bucket Policy
# Paste this policy (replace your-bucket-name with actual name):

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

# Upload image to S3 bucket (e.g., Coffee.png)
# After upload, copy the Object URL (e.g. https://your-bucket-name.s3.amazonaws.com/Coffee.png)

# Then, edit index.html:

<html>
<head>
    <title>My Static Website</title>
</head>
<body>
    <h1 style="text-align: center;">Welcome to My Static Website!</h1>

    <div style="text-align: center;">
        <img src="https://your-bucket-name.s3.amazonaws.com/Coffee.png" alt="Coffee Image" width="300">
    </div>
</body>
</html>

# Access website via browser:
# http://your-ec2-public-dns
