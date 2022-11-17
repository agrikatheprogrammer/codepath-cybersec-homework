# Honeypot Assignment

**Time spent:** **7** hours spent in total

**Objective:** Create a honeynet using MHN-Admin. Present your findings as if you were requested to give a brief report of the current state of Internet security. Assume that your audience is a current employer who is questioning why the company should allocate anymore resources to the IT security team.

### MHN-Admin Deployment (Required)

**Summary:** How did you deploy it? Did you use GCP, AWS, Azure, Vagrant, VirtualBox, etc.?

I used GCP and CLI. Milestone 0: To the Cloud!

To complete this assignment, you'll need access to a cloud hosting provider to provision the VMs. Many providers offer time-limited free trial accounts with resource limitations, and you should easily be able to complete the requirements for this assignment within these limitations -- though you may need to ensure you cleanup before your trial period expires. The setup we'll walkthrough below has been tested to work with micro-sized VMs with < 1GB memory and < 10GB disk space, so you may often be able to use the smallest possible option when provisioning VMs. All servers in this setup use Ubuntu 14.04 (trusty) -- and most cloud providers offer this as an option. Note that Ubuntu 16.04 and 17.04 will not work.

You can use any cloud provider to which you already have access or that offers a free trial, though you'll need to be familiar with its usage and / or limitations. If you're not sure where to start, we recommend Google Cloud Platform's Free Tier, and while we'll provide general guidelines that should work with most cloud providers, the instructions below will also show insets labeled GCP Users with commands and settings specific to Google Cloud Platform. If you are confident about working with an alternate cloud provider such as AWS feel free to adapt the below instructions accordingly. If this is your first foray into the world of cloud computing, consider starting a GCP trial so you can follow the more specific instructions below.

So to get started, make sure you have authenticated access to your cloud provider. You can provision the VMs any way you like, but you will need to be able to access the VMs via SSH.

GCP Users To begin, download and install the GCP SDK on your local machine and initialize it such that you can use gcloud from the command line. Be sure to set a default region and zone (these instructions were tested against the us-central1 region and us-central1-c zone). Once you've initialized, you should be able to run gcloud config list and see the project, region, and zone configured in the output.

Milestone 1: Create MHN Admin VM

Start by creating the MHN Admin VM via your cloud provider. The VM needs to have an internet-facing IP and accessible to you via ssh (or a similar protocol). As specified above, you can use a small or micro-sized VM for this, but the following attributes are required:

Ubuntu 14.04 (trusty) HTTP traffic allowed (port 80) TCP ports 3000 and 10000 need to be open to allow incoming (aka 'ingress') traffic That last requirement is generally the only one that may require a specific firewall rule to configure properly, because those ports are non-standard and specific to MHN. Some cloud providers may require you to create the firewall rules separately and then apply them to the VM. Either way, make sure when you create the VM that you can access it via SSH.

GCP Users First, create a firewall rule to allow ingress traffic on TCP ports 3000 and 10000. The following command can be used to do this via the command line:

$ gcloud beta compute firewall-rules create mhn-allow-admin --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:3000,tcp:10000 --source-ranges=0.0.0.0/0 --target-tags=mhn-admin

This will prompt you to install the beta SDK; after doing so, the command should complete successfully. You can verify the mhn-allow-admin rule was created via the browser by looking at the VPC Network Firewall Ingress Rules.

Next, create the VM itself, which we'll call mhn-admin:

gcloud compute instances create "mhn-admin" \
    --machine-type "n1-standard-1" \
    --subnet "default" \
    --maintenance-policy "MIGRATE" \
    --tags "mhn-admin" \
    --image-family "ubuntu-minimal-1804-lts" \
    --image-project "ubuntu-os-cloud" \
    --boot-disk-size "10" \
    --boot-disk-type "pd-standard" \
    --boot-disk-device-name "mhn-admin"
    
Note the tags value, which controls the applicable firewall rules. The output should show both internal and external IP addresses...make note of the external IP.

Finally, establish SSH access to the VM via gcloud compute ssh mhn-admin (which is similar to ssh). You'll be asked to add the fingerprint the first time you connect, and you'll see the Ubuntu welcome message and a command prompt.

COMMAND: gcloud compute ssh mhn-admin

Updating project ssh metadata...done. Waiting for SSH key to propagate. Warning: Permanently added 'compute.3246857753144767930' (ECDSA) to the list of known hosts.

Milestone 2: Install the MHN Admin Application

After having established SSH access the MHN Admin VM, the following instructions can be run on the remote VM to install the application. Note: this step may take 30-40 minutes overall. These instructions were adapted from the MHN README.

Do you wish to run in Debug mode? y/n : n Superuser email: You can use any email -- this will be your username to login to the admin console. Superuser password: Choose any password -- you'll be asked to confirm. You can accept the default values for the rest of the values, either by hitting enter or n for any y/n prompts:

Server base url ["http://#.#.#.#"]: Honeymap url ["http://#.#.#.#:3000"]: Mail server address ["localhost"]: Mail server port [25]: Use TLS for email?: y/n n Use SSL for email?: y/n n Mail server username [""]: Mail server password [""]: Mail default sender [""]: Path for log file ["/var/log/mhn/mhn.log"]: The script will churn for another 15 minutes or so, and near the end, there will be two more y/n prompts, both of which you can answer n:

Would you like to integrate with Splunk? (y/n) n Would you like to install ELK? (y/n) n Now you should be able to load the external IP in a browser and login to the admin console via the "superuser" values you chose above. Have a look around the UI to get oriented; there won't be any data available as we've not deployed any honeypots yet.


Milestone 3: Create a MHN Honeypot VM MHN supports multiple honeypots, each of which has a slightly different purpose you can read about. To start, we'll deploy Dionaea, a honeypot used to trap malware samples.

First, create a VM for your first honeypot via your cloud provider. As before, this VM also needs to have an internet-facing IP and accessible to you via ssh (or a similar protocol), and as before, you can use a small or micro-sized VM, but it should be running Ubuntu 14.04 (trusty).

This VM will require different ports open, though which ones depend on the specific honeypot being used. To keep things simple, for this VM (and any additional honeypot VMs you create), simply allow incoming traffic from all ports and protocols. Again, this will likely require a firewall rule.

Create the VM and establish an SSH connection to it before proceeding to the next step.
          
<img src="http://g.recordit.co/wMQ6AIfwYD.gif">

### Dionaea Honeypot Deployment (Required)

Now, create the VM for our honeypot, called honeypot-2 using wget "http://35.238.32.74/api/script/?text=true&script_id=2" -O deploy.sh && sudo bash deploy.sh http://35.238.32.74 chnQqKRy

<img src="http://g.recordit.co/PG4AvhXQpS.gif">

Dionea $ gcloud compute instances create "honeypot-2" --machine-type "f1-micro" --subnet "default" --maintenance-policy "MIGRATE" --scopes "https://www.googleapis.com/auth/devstorage.read_only","https://www.googleapis.com/auth/logging.write","https://www.googleapis.com/auth/monitoring.write","https://www.googleapis.com/auth/servicecontrol","https://www.googleapis.com/auth/service.management.readonly","https://www.googleapis.com/auth/trace.append" --tags "mhn-honeypot","http-server" --image "ubuntu-1404-trusty-v20171010" --image-project "ubuntu-os-cloud" --boot-disk-size "10" --boot-disk-type "pd-standard" --boot-disk-device-name "honeypot-2"

**Summary:** Briefly in your own words, what does dionaea do?

A honeypot designed to capture malware. The intention is to trap malware exploiting vulnerabilities exposed by services offered to a network. Dionaea aims to trap malware exploiting vulnerabilities exposed by services offered over a network, and ultimately obtain a copy of the malware.

<img src="http://g.recordit.co/vrwEcS14Ex.gif">
        
<img src="http://g.recordit.co/h2smVr5wp6.gif">

### Database Backup (Required) 

**Summary:** What is the RDBMS that MHN-Admin uses? What information does the exported JSON file record?



*Be sure to upload session.json directly to this GitHub repo/branch in order to get full credit.*


