# LAB: Google Cloud Fundamentals: Getting Started with Cloud Storage and Cloud SQL

## Objectives

In this lab, you will learn how to perform the following tasks:

	* Create a Cloud Storage bucket and place an image into it.
	* Create a Cloud SQL instance and configure it.
	* Connect to the Cloud SQL instance from a web server.
	* Use the image in the Cloud Storage bucket on a web page.


## Tasks
1. Task 1: Create a virtual machine using the GCP Console
	- Login into google with the following command
		`gcloud auth login`
	- Set the default project with the following command
		`gcloud config set project PROJECT_ID`

2. Task 2: Deploy a web server VM instance
	- Create a VM instance named bloghost with a default machine type, Debian GNU/Linux 9 (stretch) bootdisk and a custom startup script using the following commands
	`gcloud compute instances create bloghost --zone=us-central1-a --machine-type=e2-medium --subnet=default  --metadata=startup-script=apt-get\ update$'\n'apt-get\ install\ apache2\ php\ php-mysql\ -y$'\n'service\ apache2\ restart --tags=http-server --image=debian-9-stretch-v20200910 --image-project=debian-cloud`
	
	`gcloud compute  firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server`

3. Task 3: Create a Cloud Storage bucket using the gsutil command line
	- Create a location environmetn variable with you prefered location, which in this case is the US.
		`export LOCATION=US`

	- Create a bucket named after your project id.
		`gsutil mb -l $LOCATION gs://$DEVSHELL_PROJECT_ID`

	- Retrieve a banner image from a publicly accessible Cloud Storage location with the following command
		`gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png`

	- Copy the banner image to your newly created Cloud Storage bucket
		`gsutil cp my-excellent-blog.png gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png`

	- Modify the Access Control List of the object you just created so that it is readable by everyone
		`gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png`


4. Task 4: Create the Cloud SQL instance

5. Task 5: Configure an application in a Compute Engine instance to use Cloud SQL
	- ssh into your bloghost VM with the follwoing command
		`ssh bloghost`

	- In your ssh session on bloghost, change your working directory to the document root of the web server
		`cd /var/www/html`

	- Use the nano text editor to edit a file called index.php
		`sudo nano index.php`

	- Paste the content below into the file
		`<html>
		<head><title>Welcome to my excellent blog</title></head>
		<body>
		<h1>Welcome to my excellent blog</h1>
		<?php
		 $dbserver = "CLOUDSQLIP";
		$dbuser = "blogdbuser";
		$dbpassword = "DBPASSWORD";
		// In a production blog, we would not store the MySQL
		// password in the document root. Instead, we would store it in a
		// configuration file elsewhere on the web server VM instance.
		`
		`$conn = new mysqli($dbserver, $dbuser, $dbpassword);
		``
		if (mysqli_connect_error()) {
		        echo ("Database connection failed: " . mysqli_connect_error());
		} else {
		        echo ("Database connection succeeded.");
		}
		?>``
		</body></html>
		`

	- Press `Ctrl+O`, and then press `Enter` to save your edited file

	- Press `Ctrl+X` to exit the nano text editor

	- Restart the web server with the follwoing command
	 	`sudo service apache2 restart`

	- Use curl to get data from your bloghost VM external Ip adress followed by `/index.php`. It should look something like:
		`curl 35.192.208.2/index.php`

	- Return to your ssh session on bloghost. Use the nano text editor to edit index.php again
		`sudo nano index.php`
	
	- Retrieve the command below to retrieve the IP address of the sql instance
		`gcloud sql instances list`

	- In the nano text editor, replace `CLOUDSQLIP` with the Cloud SQL instance Public IP address that you noted above. Leave the quotation marks around the value in place.

	- In the nano text editor, replace `DBPASSWORD` with the Cloud SQL database password that you defined above. Leave the quotation marks around the value in place.

	- Press `Ctrl+O`, and then press `Enter` to save your edited file.

	- Press `Ctrl+X` to exit the nano text editor.

	- Restart the web server
		`sudo service apache2 restart`


6. Task 6: Configure an application in a Compute Engine instance to use a Cloud Storage object
	- List all objects in the bucket using the following command:
		`gsutil ls gs://$DEVSHELL_PROJECT_ID`

	- Retrieve the object details using the below command and get the public accessable url from it:
		`gsutil ls -l gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png`
	- Return to your ssh session on your bloghost VM instance
	- Enter this command to set your working directory to the document root of the web server
		`cd /var/www/html`
	- Use the nano text editor to edit index.php
		`sudo nano index.php`
	- Use the arrow keys to move the cursor to the line that contains the h1 element. Press Enter to open up a new, blank screen line, and then paste the URL you copied earlier into the line
	- Paste this HTML markup immediately before the URL
		`<img src='`
	- Place a closing single quotation mark and a closing angle bracket at the end of the URL
		`'>`
	- Press `Ctrl+O`, and then press `Enter` to save your edited file
	- Press `Ctrl+X` to exit the nano text editor
	- Restart the web server
		`sudo service apache2 restart`
	- An image should now be loaded as well when the app is accessed from a browser.