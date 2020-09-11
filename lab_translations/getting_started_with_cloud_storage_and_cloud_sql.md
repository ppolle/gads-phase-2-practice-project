# LAB: Google Cloud Fundamentals: Getting Started with Compute Engine

## Objectives

In this lab, you will learn how to perform the following tasks:

	* Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.
	* Create a Compute Engine virtual machine using the gcloud command-line interface.
	* Connect between the two instances.


## Tasks
1. Task 1: Create a virtual machine using the GCP Console
	- Login into google with the following command
		`gcloud auth login`
	- Set the default project with the following command
		`gcloud config set project PROJECT_ID`

2. Task 2: Create a virtual machine using the GCP Console
	- Create a Vm called my-vm-1 using the follwoing commands
		`gcloud compute instances create "my-vm-1" --machine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default" --tags http`

		`gcloud compute firewall-rules create allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http`

3. Task 3: Create a virtual machine using the gcloud command line
	- Change you default zone to us-central1-b using the following command.
		`gcloud config set compute/zone us-central1-b`

	- Create a VM instance called my-vm-2 using the following command:
		`gcloud compute instances create "my-vm-2" --machine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default"`

4. Task 4: Connect between VM instances
	- SSH into my-vm-2 with the follwoing command
		`ssh my-vm-2`

	- Use the ping command to confirm that my-vm-2 can reach my-vm-1 over the network
		`ping my-vm-1`

	- Press `CTRL+C` to abort the ping command

	- SSH into my-vm-1 eith the follwing command
		`ssh my-vm-1`

	- Install Nginx web server
		`sudo apt-get install nginx-light -y`

	- Use the nano editor to add a custom message to the homepage of the web server
		`sudo nano /var/www/html/index.nginx-debian.html`

	- Use the arrow keys to move the cursor to the line just below the h1 header. Add text like this, and replace YOUR_NAME with your name
		`Hi from YOUR_NAME`

	- Press `Ctrl+O` and then press `Enter` to save your edited file, and then press `Ctrl+X` to exit the nano text editor

	- Confirm that the web server is serving your new page. At the command prompt on my-vm-1, execute this command
		`curl http://localhost/`

	- To exit the command prompt on my-vm-1, execute this command
		`exit`

	- To confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2, execute this command
		`curl http://my-vm-1/`