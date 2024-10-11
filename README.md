# HHA504_assignment_storage


## 1. Upload Files Using the GUI

### Azure

1. Create a new Storage Account

![Image of Azure overview](https://github.com/zgiannuzzi/HHA504_assignment_storage/blob/main/images/Azure_bucket1.png)

2/3. Create a Blob container and upload a file to it.

![Image of Azure overview](https://github.com/zgiannuzzi/HHA504_assignment_storage/blob/main/images/Azure_bucket3.png)

### GCP

1. create a new Cloud Storage bucket

![Image of Azure overview](https://github.com/zgiannuzzi/HHA504_assignment_storage/blob/main/images/GCP_Bucket1.png)

2. Upload a sample file

![Image of Azure overview](https://github.com/zgiannuzzi/HHA504_assignment_storage/blob/main/images/GCP_Bucket2.png)

## 2. Upload files using python

### Azure

1. Used professors google colab and foollwed along to succesfully add a file to azure with python
   - created a new .env file
   - used key to access container
   - ran code to upload file

![Image of Azure overview](https://github.com/zgiannuzzi/HHA504_assignment_storage/blob/main/images/Azure_bucket5.png)

2. Uploaded files with python

![Image of Azure overview](https://github.com/zgiannuzzi/HHA504_assignment_storage/blob/main/images/Azure_bucket4.png)

### GCP

1. Used proffessors program the upload files to gcp and changed neccessary paths
   - I did use githgub code space and realized I had to install google client library using the following "pip install --upgrade google-cloud-storage"
```python
from google.cloud import storage
from PIL import Image
import io
import os

# Step 1: Set up your Google credentials
os.environ['GOOGLE_APPLICATION_CREDENTIALS'] = 'gcp_secret.json'

# Step 2: Create a Google Cloud Storage client
client = storage.Client()

# Step 3: Create a bucket or use an existing one
bucket_name = 'hha504-assignment'  # Change this to your bucket name
bucket = client.bucket(bucket_name)


## create a list of files in /images with path 
files_upload = []
for root, dirs, files in os.walk("images"):
    for file in files:
        files_upload.append(os.path.join(root, file))

for file in files_upload:
    ## print working on
    print("fWorking on {file}")
    ## load file using io into memory
    with open(file, 'rb') as f:
        file_byte_array = f.read()
    print(file)
    ## just keep file name 
    file = file.split("/")[-1]
    print('new file name: ', file)
    ## upload file to GCS
    try:
        blob = bucket.blob(file)
        blob.upload_from_string(file_byte_array, content_type='image/png')
        print(f"Image uploaded successfully to Google Cloud Storage!")
    except Exception as e:
        print(f"Error: {e}")



# Step 4: Create a fake image using Pillow
image = Image.new('RGB', (100, 100), color = (73, 109, 137))
image_byte_array = io.BytesIO()
image.save(image_byte_array, format='PNG')

# Step 5: Upload the fake image to Google Cloud Storage
blob = bucket.blob('fake_image.png')
blob.upload_from_string(image_byte_array.getvalue(), content_type='image/png')

print("Image uploaded successfully to Google Cloud Storage!")
```

![Image of Azure overview](https://github.com/zgiannuzzi/HHA504_assignment_storage/blob/main/images/GCP_Bucket4.png)

## 3.Explore Storage Features

### Azure
### Blob access tiers
1. As a default you can either set a hot or cool access tier
   - Hot - if you are frequently accessing data
   - cool - More used for backup scenarios
![Image of Azure overview](https://github.com/zgiannuzzi/HHA504_assignment_storage/blob/main/images/Azure_bucket6.png)

2. When uploading blobs you have the option to change access tier or leave default

![Image of Azure overview](https://github.com/zgiannuzzi/HHA504_assignment_storage/blob/main/images/Azure_bucket7.png)

### Access policies
1. You can add predetermined role assignments or create your own to control what blobs users have access to. 

![Image of Azure overview](https://github.com/zgiannuzzi/HHA504_assignment_storage/blob/main/images/Azure_bucket8.png)

### GCP
### IAM permissions
1. You have the abilty to create new roles and assign them to either specific users or groups fo users

![Image of Azure overview](https://github.com/zgiannuzzi/HHA504_assignment_storage/blob/main/images/GCP_Bucket5.png)

### Life cycle rules

1. With this you can create rules to manage your blobs
   - For example you want to move from Hot to cold
   - Can control many options as seen below

![Image of Azure overview](https://github.com/zgiannuzzi/HHA504_assignment_storage/blob/main/images/GCP_Bucket6.png)








