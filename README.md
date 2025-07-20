# 🗂️ Cloud-Based File Storage System

This project uses an EC2 instance to host a Flask web app that uploads files to an Amazon S3 bucket. Users can interact with a simple HTML page to upload files directly to cloud storage.

---

## 🔧 Prerequisites

- AWS account (EC2 + S3)
- Git Bash (Windows)
- `.pem` key for EC2 SSH access
- Python 3 & pip
- Flask and Boto3 installed

---

## 🚀 Git Bash Setup & EC2 Configuration

### 🛫 1. Navigate to Key Directory & Connect via SSH

```bash
cd "C:/Users/ankit/OneDrive/Desktop/project/project key"
chmod 400 ankit.pem
ssh -i ankit.pem ec2-user@<YOUR_EC2_PUBLIC_IPV4>
sudo yum update -y  
sudo yum install python3-pip -y  
pip3 --version  
pip3 install Flask boto3  
mkdir Data_storage_app  
cd Data_storage_app  
nano app.py

this will open nano text editor where we have to paste a code   
Start -> 
from flask import Flask, request, redirect, url_for 
import boto3 
import os 
app = Flask(__name__) 
S3_BUCKET_NAME = 'globalfiles01' 
s3 = boto3.client('s3', region_name='ap-south-1') 
@app.route('/', methods=['GET', 'POST']) 
def upload_file(): 
if request.method == 'POST': 
if 'file' not in request.files: 
            return 'No file part' 
        file = request.files['file'] 
        if file.filename == '': 
            return 'No selected file' 
        if file: 
            try: 
                s3.upload_fileobj(file, S3_BUCKET_NAME, file.filename) 
                return 'File uploaded successfully!' 
            except Exception as e: 
                return f'Error uploading file: {e}' 
    return ''' 
    <!doctype html> 
    <html> 
    <head> 
        <title>Upload File</title> 
    </head> 
    <body> 
        <h1>Upload a new file</h1> 
        <form method=post enctype=multipart/form-data> 
            <input type=file name=file> 
            <input type=submit value=Upload> 
        </form> 
    </body> 
    </html> 
    ''' 
 
if __name__ == "__main__": 
    # 0.0.0.0 exposes the app externally (good for EC2) 
app.run(host="0.0.0.0", port=5000, debug=True) 
press ->  ctrl+o , enter , ctrl+x

python3 app.py  
http://3.110.119.77:5000/  // this is the public ipv4 of ec2and port 5000 to open website 


