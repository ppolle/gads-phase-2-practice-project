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

6. Task 6: Configure an application in a Compute Engine instance to use a Cloud Storage object