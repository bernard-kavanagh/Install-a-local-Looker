Prerequisites
- A GCP project where you have admin access to create a Compute Engine VM instance

- A personal Looker license key

- You should not connect your personal Looker instance to any database that contains live customer data

Steps
- In your project, create a GCP VM instance by going to https://console.cloud.google.com/compute/instances and clicking "create instance"

- Select a machine configuration with enough RAM and disk space to run Looker (see https://docs.looker.com/setup-and-management/on-prem-install/installation for requirements)

- Use the default Boot Disk. If you need to change it, ensure it is a Drawfork image. Then create the instance. 

- Note the name, zone, and project ID of your VM instance

- In the GCP VM page, SSH into your instance from your browser (note this is different from SSHing via your local machine)

- Either download the latest jar files and upload to your VM (takes about 30' to upload) or use the API (fast) to download directly to the VM.

	- Download via API:
	
		- On your VM (in the browser SSH window), install java:
		sudo apt-get update
		sudo apt-get install default-jdk
		
		- Install brew
		
		- Put brew in your $PATH
		
		- Use brew to install wget and jq
		
	- Download then upload manually:
	
		- Download the files to your local machine, then in the browser SSH window select "Upload file" from the settings dropdown
		
- Follow the normal instructions on https://docs.looker.com/setup-and-management/on-prem-install/installation to create your looker user and sudo in as them on your VM. Continue the installation process normally and get Looker running on your VM (default port 9999) 

Accessing your Looker instance
Method 1: Firewall whitelisting

- install the cloud SDK. In your VM, get the gcloud link from the SSH menu and drop it in your local command line.
You may need to add a firewall rule to allow your IP address in. 
To get your machine’s IP address, simply Google it and take note!
https://www.google.com/search?q=what+is+my+ip+address&oq=what+is+my+ip+address

- We can now navigate back to the VM instances panel.
> Compute Engine
> VM Instances
On the right, click on the 3 dots and click View network details
Then click Firewall on the left navigation panel.

- We are now going to create a new firewall rule, so we can click that at the top.
> + CREATE NEW FIREWALL RULE
You can set the name as: looker-firewall
Set your Source IP ranges as your machine’s IP address.
Select the Targets as All instances in the network.
In the protocols and ports sections, select the check box for tcp and add 9999.
If you also want to use Looker’s API, then also add 19999.
Then click CREATE.

- We can now navigate back to the VM instances panel.
> Compute Engine
> VM Instances
And you can navigate to your External IP address. Click the link, and then add ‘:9999’ to the URL. You should then see Your connection is not private.
Simply type: thisisunsafe

- Don’t sign up for a trial. Click Already have a license?

Method 2: SSH tunneling

Depending on which folder your GCP project is located in, it may be subject to GCE Enforcer (ooooh). This will not let you create a firewall ingress rule to port 9999 (it will, but will delete it after an hour or so). If this is the case, you'll have to use this method

- Here you'll SSH into your VM from your local machine . and set up a port forward. Make sure you're set up to use your Google security key from your command line

- In your command line on your local machine, type gcert to renew your Google certificate - you'll have to enter your password and touch your gnubby key

- Then, type ssh nic0.[your-instance-name].[your-instance-region].c.[your-project-id].internal.gcpnode.com -l [your-google-id]_google_com -N -L 9999:localhost:9999 (replacing everything in square brackets) - you'll have to touch your gnubby key again (I had issues here with my Macbook touchID - you may have to use the physical gnubby)

- You should now be able to see your Looker instance on https://localhost:9999 on your browser
