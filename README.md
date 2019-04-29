# Worker Safety  v2
## 1. Register the AWS DeepLens Device
### 1.1 Configure Your AWS Account to Use AWS DeepLens 
If you recycle a device from another user, make sure that the previous user has deregistered the device before registering it again.

To configure your AWS account for AWS DeepLens

- Sign in to the AWS Management Console for AWS DeepLens at https://console.aws.amazon.com/deeplens/home?region=us-east-1#firstrun.

- Choose Register device. If you don't see a Register device button, choose Devices on the main navigation pane.

- In the Name your device section on the Configure your AWS account page, type a name (e.g., abdi-deepLens-1) for your AWS DeepLens device in the Device name text field .

- The device name can have up to 100 characters. Valid characters are a-z, A-Z, 0-9, and - (hyphen) only. 
- In the Permissions section, choose Create roles for the AWS DeepLens console to create the required IAM roles with relevant permissions on your behalf.
- After the roles are successfully created, you'll be informed that you have the necessary permissions for setting the AWS DeepLens device. If the roles already exist in your account, the same message will be displayed.
- In the Certificate section, choose Download certificate to save the device certificate.

    **Important**
    
    The downloaded device certificate is a .zip file. Don’t unzip it.
    
    Certificates aren't reusable. You must generate a new certificate every time you register your device.

- After the certificate is downloaded, choose Next to proceed to joining your computer to your device's (AMDC-NNNN) Wi-Fi network in order to start the device setup application hosted on the device. 
- Plug in your AWS DeepLens device to an AC power outlet. Press the power button on the front of the device to turn the device on. 
- Wait until the device has entered into setup mode when the Wi-Fi indicator (middle LED) on the front of the device starts to flash. 
- Open the network management tool on your computer. Choose your device's SSID from the list of available Wi-Fi networks and type the password for the device's network. The SSID and password are printed on the bottom of your device. The device's Wi-Fi network's SSID has the AMDC-NNNN format
- After successfully connecting your computer to the device's Wi-Fi network, you're now ready to launch the device setup application to configure your device. 

- For updating the device settings after the initial registration, open a web browser tab and enter http://deeplens.amazon.net or http://deeplens.config in the address bar.

    **Note**

    If the above URL doesn't work, your AWS DeepLens device may have the awscam software version 1.3.5 or earlier installed. In this case, update the device software and try it again.

    Alternatively, instead of http://deeplens.amazon.net or http://deeplens.config, you can open the device setup page by using one of the following URLs, depending on the software version on your AWS DeepLens device.

    http://192.168.0.1, if the AWS DeepLens software package (awscam) version is less than 1.2.4

    http://10.105.168.217, if the AWS DeepLens software package (awscam) version is greater than or equal to 1.2.4


### 1.2 Connect Your AWS DeepLens to the Workshop Wi-Fi Network 
- Look at the bottom of your device and make note of the device's AMDC-NNNN network SSID and password. 

- Plug in your AWS DeepLens device to an AC power outlet. Press the power button

- Wait until the device has entered into setup mode when the Wi-Fi indicator (middle LED) on the front of the device starts to flash. power button on the front of the device to turn the device on. 

- Open the network management tool on your computer. Choose your device's SSID from the list of available Wi-Fi networks and type the password for the device's network. The SSID and password are printed on the bottom of your device. The device's Wi-Fi network's SSID has the AMDC-NNNN format

- After successfully connecting your computer to the device's Wi-Fi network, you're now ready to launch the device setup application to configure your device. 

- When your device has an updated software installed, starting the device setup application (e.g., http://deeplens.config) opens a browser page

- The Connect your device to your home or office network section shows the device's internet connection status as **Online**

- Under Upload security certificate to associate your AWS DeepLens to your AWS account on the updated Configure your AWS DeepLens page, do the following:

    - Choose Browse to open a file picker.

    - Locate and choose the security certificate that you downloaded when preparing your AWS account for AWS DeepLens,

    - Choose Upload zip file to attach the certificate to the device.

    **Note**

    The downloaded security certificate for the device is a .zip file. Upload the unzipped certificate file as-is.

- Under Set a password to control how you access and update the device, complete the following steps to configure device access.

- Review the settings. Then, choose Finish to complete setting up the device and to terminate your connection to the device's Wi-Fi network

**Note**

To ensure the setup completes successfully, make sure that AWS DeepLens device has access to ports 8883 and 8443 and is not blocked by your network firewall policy. 

## 2. Preperaing the data

### 2.1 Download the raw dataset

### 2.2 Store your data in Amazon S3

### 2.3 Create a labeling workforce,
### 2.4 Create a labeling job,
### 2.5 Get to work on labeling the data
### 2.6 Visualize results on the console
### 2.7 Split the resulting labeled dataset into training and validation data

## 3. Create the Amazon SageMaker training job
### 3.1 Validate that model has been successfuly created

## 4. Create and deploy Worker Safety detection project to DeepLens.
### 4.1 Setup IAM Role for Cloud Lambda
- Go to IAM in AWS Console at https://console.aws.amazon.com/iam
- Click on Roles
- Click create role
- Under AWS service, select Lambda and click Next: Permissions
- Under Attach permission policies
    - search S3 and select AmazonS3FullAccess
    - search Rekognition and select checkbox next to AmazonRekognitionReadOnlyAccess
    - search cloudwatch and select checkbox next to CloudWatchLogsFullAccess and CloudWatchFullAccess
    - search iot and select AWSIotDataAccess
    - search lambda and select checkbox next to AWSLambdaFullAccess
- Click Next: Tags and Next: Review
- Name is “RecognizeObjectLambdaRole”
- Click Create role

### 4.2 Setup IAM Role for DeepLens Lambda
- Click create role
- Under AWS service, select Lambda and click Next: Permissions
- Under Attach permission policies
    - search S3 and select AmazonS3FullAccess
    - search lambda and select checkbox next to AWSLambdaFullAccess
- Click Next: Tags and Next: Review
- Name is “DeepLensInferenceLambdaRole”
- Click Create role

### 4.3 Create S3 bucket
- Go to Amazon S3 in AWS Console at https://s3.console.aws.amazon.com/s3/
- Click on Create bucket.
- Under Name and region:
    - Bucket name: Enter a bucket name- your name-worker-safety (example: kashif-worker-safety)
    - Choose US East (N. Virginia)
    - Click Next
- Leave default values for Configure Options screen and click Next
- Under Set permissions, uncheck all four checkboxes. NOTE: This step would allow us to make objects in your S3 bucket public. We are doing this to reduce few steps in the module, but you should not do that for production workloads. Instead it is recommended to use S3 Pre-Signed URLs to give time limited access to objects in S3.
- Click Next, and click Create bucket.

### 4.4 Create Cloud Lambda
- Go to Lambda in AWS Console at https://console.aws.amazon.com/lambda/
- Click on Create function.
- Under Create function, Author from scratch should be selected as default.
- Under Author from scratch:
    - Name: worker-safety-cloud
    - Runtime: Python 3.7
    - Role: Choose and existing role
    - Existing role: RecognizeObjectLambdaRole
    - Click Create function
- Under Environment variables, add a variable:
    - Key: iot_topic
    - Value: worker-safety-demo-cloud
- Download lambda.zip.
- Under Function code:
    - Code entry type: Upload a zip file
    - Under Function package, click Upload and select the zip file you - downloaded in earlier step.
    - Click Save.
- Under Add triggers, select S3.
- Under Configure triggers:
    - Bucket: Select the S3 bucket you just created in earlier step.
    - Event type: Leave default Object Created (All)
    - Leave defaults for Prefix and Suffix and make sure Enable trigger checkbox is checked.
    - Click Add.
    - Click Save on the top right to save changed to Lambda function.

### 4.5 Create DeepLens Inference Lambda Function
- Go to Lambda in AWS Console at https://console.aws.amazon.com/lambda/
- Click on Create function.
- Under Create function, select Blueprints.
- Under Blueprints, type greengrass and hit enter to filter blueprint templates.
- Select greengrass-hello-world and click Configure.
- Under Basic information:
    - Name: name-worker-safety-deeplens (example: abdi-worker-safety-deeplens)
    - Role: Choose and existing role
    - Existing role: DeepLensInferenceLambdaRole
    - Click Create function.
- Copy the code from deeplens-lambda.py and paste under Function code for the lambda function. You can find the python file in your resources section.
- Go to line 34 and modify line below with the name of your S3 bucket created in the earlier step.
    - bucket_name = "REPLACE-WITH-NAME-OF-YOUR-S3-BUCKET"

- Click Save.
- Click on Actions, and then "Publish new version".
- For Version description enter: Detect person and push frame to S3 bucket. and click Publish.

### 4.6 Create DeepLens Project
- Using your browser, open the AWS DeepLens console at https://console.aws.amazon.com/deeplens/.
- Choose Projects, then choose Create new project.
- On the Choose project type screen
    - Choose Create a new blank project, and click Next.
    - On the Specify project details screen - Under Project information section:
        - Project name: your-user-name-worker-safety (example: abdi-worker-safety)
        - Under Project content:
            - Click on Add model, click on radio button for deeplens-object-detection and click Add model.
            - Click on Add function, click on radio button for your lambda function (example: abdi-worker-safety-deeplens) lambda function and click Add function.
- Click Create. This returns you to the Projects screen.

### 4.7 Deploy DeepLens Project
- From DeepLens console, On the Projects screen, choose the radio button to the left of your project name, then choose Deploy to device.
- On the Target device screen, from the list of AWS DeepLens devices, choose the radio button to the left of the device where you want to deploy this project.
- Choose Review. This will take you to the Review and deploy screen. If a project is already deployed to the device, you will see a warning message "There is an existing project on this device. Do you want to replace it? If you Deploy, AWS DeepLens will remove the current project before deploying the new project."
- On the Review and deploy screen, review your project and click Deploy to deploy the project. This will take you to to device screen, which shows the progress of your project deployment.

### 4.8 View Output in IoT
- Go to IoT Console at https://console.aws.amazon.com/iot/home
- Under Subscription topic enter topic name you entered as environment variable for Lambda in earlier step (example: worker-safety-demo-cloud) and click Subscribe to topic.
- You should now see JSON message with a list of people detected and whether they are wearing safety hats or not.

### 4.9 View Output in CloudWatch
- Go to CloudWatch Console at https://console.aws.amazon.com/cloudwatch
- Create a dashboard called “worker-safety-dashboard-your-name”
- Choose Line in the widget
- Under Custom Namespaces, select “string”, “Metrics with no dimensions”, and then select PersonsWithSafetyHat and PersonsWithoutSafetyHat.
- Next, set “Auto-refresh” to the smallest interval possible (1h), and change the “Period” to whatever works best for you (1 second or 5 seconds)

### 4.10 View Output in Web Dashboard
- Go to AWS Cognito console at https://console.aws.amazon.com/cognito
- Click on Manage Identity Pools
- Click on Create New Identity Pool
- Enter “awsworkersafety” for Identity pool name
- Select Enable access to unauthenticated identities
- We are using using Unauthenticated identity option to keep things simple in the demo. For real world application where you only want authorized users to access the app you should configure Authentication providers.
- Click Create Pool
- Expand View Details
- Under: Your unauthenticated identities would like access to Cognito, expand View Policy Document and click Edit.
- Click Ok for Edit Policy prompt.
- Copy JSON from cognitopolicy.json and paste in the text box.
- Click Allow
- Make note of the Identity Pool as you will need it in following steps.
- Got to IoT in AWS Console at: https://console.aws.amazon.com/iot
- Click on settings and make note of Endpoint, you will need this the following step.
- Download webdashboard.zip and unzip on your local drive.
- Edit aws-configuration.js and update poolId with Cognito Identity Pool Id and host with IoT EndPoint you got in earlier steps.
- From terminal go to the root of the unzipped folder and run “npm install”
- Next, run “./node_modules/.bin/webpack —config webpack.config.js”
- This will create the build we can easily deploy.
- Go to S3 bucket, and create a folder web
- From web folder in S3 bucket click upload and select bundle.js, index.html and style.css.
- From Set permission, Choose Grant public read access to the objects. and click Next
- Leave default settings for following screens and click upload.
- Click on index.html and click on the link to open the web page in browser.
- In the address URL append ?iottopic=NAME-OF-YOUR-IOT-TOPIC. This is the same value you added to Lambda environment variable and hit Enter.
- You should now see images coming from DeepLens with a green or red box around the person.

## 5 Clean Up

Delete Lambda functions, S3 bucket and IAM roles.