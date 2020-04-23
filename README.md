# Onboarding F5 BIG-IP on IBM VPC Cloud (Gen 2)

### Link to postman collection (WIP)
https://www.getpostman.com/collections/0cfd433cc12f5c1cf7e4

## Create an object storage bucket and upload your BIG-IP image

1.	From your https://cloud.ibm.com dashboard, choose “Create Resource”.
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_1.PNG" width="800">

2.	Under “Storage”, choose the “Object Storage” tile.
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_2.PNG" width="800">

3.	Create your object storage.  You may choose any name for “Service Name” (I chose “USCloudDallas”), choose a resource group (I chose “default”) and associate any tags you want to organize your resources.  Press “Create”.
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_3.PNG" width="800">

4.	Now, create a bucket for this service.
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_4.PNG" width="800">

5.	Choose “Custom Bucket”.
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_5.PNG" width="800">

6.	Choose a name for your bucket and choose the region of your choice.  Note that I chose the “Standard” storage class.  Smart Tier may save money, but I haven’t tested it. 
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_6.PNG" width="800">

7.	After creating the bucket, it’s now time for uploading your BIG-IP image.  Choose “Upload” – “Files”.
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_7.PNG" width="800">

8.	Choose “Select Files” from the “Upload files using Aspera high-speed transfer” window.  A dialog will open on your PC for you to choose the qcow2 image you downloaded from downloads.f5.com.
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_8.PNG" width="800">

9.	Click “Allow” from the “Confirm – IBM Aspera Connect” window.  The image will now upload.  Depending on your internet connection this could take a while (it took about 60 minutes for me to upload).  Once completed, you will see something like this:
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_9.PNG" width="800">

## Create a VPC Instance (you may skip this if you already have one)

1.	From your https://cloud.ibm/com dashboard, choose “Create Resource”.
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_10.PNG" width="800">

2.	Under “VPC Infrastructure”, choose “Virtual Private Cloud”:
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_11.PNG" width="800">

3.	You will now create your VPC.  Make sure the screen says “Gen 2” compute.  It should default to Gen 2 and not Gen 1, but make sure it looks like this:
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_12.PNG" width="800">

4.	Name your VPC whatever you want.  Note that you MUST use only lowercase letters and hyphens.  That’s it.  So, “VPC-test” will not work, you must use a name like “vpc-test”.  Note also “vpc_test” or “vpc test” will not work.  Hyphens only!
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_13.PNG" width="800">

5.	Scroll down to continue to configure your VPC, including creation of your first subnet:
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_14.PNG" width="800">

6.	Continue to scroll down to see the IP address space created on your subnet.  Press the “Create virtual private cloud” button to start the process.
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_15.PNG" width="800">

7.	Within a second or two the VPC Gen 2 menu should appear along with your created VPC:
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_16.PNG" width="800">

## Create a Virtual Server Instance (BIG-IP)

1.	On the VPC Gen 2 menu, click on “Virtual Server Instances”
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_17.PNG" width="800">

2.	Click on “New Instance”
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_18.PNG" width="800">

3.	Choose the name for your instance (this is NOT the BIG-IP hostname) and make sure you deploy into the correct VPC (if you have more than one).
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_19.PNG" width="800">

4.	Continue to scroll down.  Make sure you’re choosing the correct data center.  For image, choose “Custom image”.  When you do, choose the BIG-IP image you uploaded to your object storage.  I only had one image uploaded as you can see here:
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_20.PNG" width="800">

5.	Now, choose what compute profile you wish to use.  First, click “All profiles” so you don’t only see the most popular ones.
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_21.PNG" width="800">

6.	Once “All profiles” is targeted, click the “Compute” tab.  Your compute profile will depend on the exact BIG-IP license you wish to deploy.  Please see here for CPU, RAM and disk size recommendations.  For simplicity, we recommend choosing from the ones highlighted, again, depending on which license you wish to deploy.
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_22.PNG" width="800">

7.	Now choose an ssh key you created for your account and then click “Create virtual server instance”.
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_23.PNG" width="800">

8.	You will now see your virtual instance starting
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_24.PNG" width="800">

9.	Once the instance has been created, we now need to assign a floating IP so we can communicate with it.  Click on your instance after it’s powered-up and scroll down to your network interfaces.  Click “Reserve +” to create the floating IP.
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_25.PNG" width="800">

10.	You will now have your new floating IP:
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_26.PNG" width="800">

11.	Click on your security group so we can open up some ports to be able to login:
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_27.PNG" width="800">

12.	Click on “new rule”:
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_28.PNG" width="800">

13.	Let’s open up port 8443 (since we’re single NIC) to be able to open the GUI:
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_29.PNG" width="800">
14.	Click “Save”.

15.	Test connectivity to your instance.  Open up a new tab in your browser and choose the floating IP of your instance.  In my case, https://169.48.93.51:8443
<img src="https://github.com/jgruberf5/ibm-vpc2-tmos/blob/master/pictures/vpc_30.PNG" width="800">
