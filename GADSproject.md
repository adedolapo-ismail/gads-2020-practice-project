# GADS-2020-Practice-Project
# Completed Labs.
1. App Dev: Setting up a Development Environment v1.1
2. App Dev: Storing Application Data in Cloud Datastore v1.1
3. Virtual Private Networks (VPN)
4. GCP Fundamentals: Getting Started with Cloud Marketplace
5. GCP Fundamentals: Getting Started with Compute Engine
6. GCP Fundamentals: Getting Started with Cloud Storage and Cloud SQL
7. GCP Fundamentals: Getting Started with Kubernetes Engine	
8. GCP Fundamentals: Getting Started with App Engine
9. GCP Fundamentals: Getting Started with Deployment Manager and Stackdriver
10. GCP Fundamentals: Getting Started with BigQuery


# Translated Labs
---
# 1. Google Cloud Fundamentals: Getting Started with Compute Engine
---

### Overview
In this lab, you will create virtual machines (VMs) and connect to them. You will also create connections between the instances.
### Objectives
In this lab, you will learn how to perform the following tasks:

- Create two Compute Engine virtual machines using the gcloud command-line interface.

- Connect between the two instances.

### Steps

1. Sign in to the Google Cloud Platform (GCP) Console with your qwiklabs credentials in an incognito window.

2. Click on the Cloud Shell icon to activate the Google Cloud Shell.
![Select the cloud shell icon](https://storage.googleapis.com/practise-test-cloud-shell-icon/personal%20shell%20stuff.png)
- Click continue.
![continue](https://cdn.qwiklabs.com/lr3PBRjWIrJ%2BMQnE8kCkOnRQQVgJnWSg4UWk16f0s%2FA%3D)

3. Create a virtual machine `my-vm-1` that allows HTTP traffic.
- Create a virtual machine in us-central1-a.
```
gcloud compute instances create my-vm-1 --zone=us-central1- a --machine-type=n1-standard-1 --subnet=default--network-tier=PREMIUM --tags=http-server --image=debian-9-stretch-v20200902 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=my-vm-1
```
- Create a firewall rule that allows HTTP ingress traffic for `my-vm-1`.
```
gcloud compute firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server
```

4. Create another vm in a zone different from us-central1-a.
- To display the zones available in the assigned region, execute the following command:
```
gcloud compute zones list | grep us-central1
```
- Change your default zone by executing the following command, replacing us-central1-b with any zone of your picking.
```
gcloud config set compute/zone us-central1-b
```
- To create the VM instance `my-vm-2`, execute the following command.
```
gcloud compute instances create "my-vm-2" --machine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213"--subnet "default"
```
4. Connect between VM instances.
- Launch the command prompt in `my-vm-2` by starting the SSH with the following command.Replace PROJECT_ID, ZONE, VM_NAME as applicable.
```
gcloud compute ssh --project PROJECT_ID --zone ZONE VM_NAME
```
- Use the ping command to confirm that my-vm-2 can reach my-vm-1 over the network:
```
ping my-vm-1
```
Notice that the output of the ping command reveals that the complete hostname of my-vm-1 is my-vm-1.c.PROJECT_ID.internal, where PROJECT_ID is the name of your Google Cloud Platform project. GCP automatically supplies Domain Name Service (DNS) resolution for the internal IP addresses of VM instances.

- Press Ctrl+C to abort the ping command.

- Use the ssh command to open a command prompt on my-vm-1:
```
ssh my-vm-1
```
- At the command prompt on my-vm-1, install the Nginx web server:
```
sudo apt-get install nginx-light -y
```
- Use the nano text editor to add a custom message to the home page of the web server:
```
sudo nano /var/www/html/index.nginx-debian.html
```
- Use the arrow keys to move the cursor to the line just below the h1 header. Add text like this, and replace YOUR_NAME with your name:
```
Hi from YOUR_NAME
```

- Press Ctrl+O and then press Enter to save your edited file, and then press Ctrl+X to exit the nano text editor.

- Confirm that the web server is serving your new page. At the command prompt on my-vm-1, execute this command:

```
curl http://localhost/
```

The response will be the HTML source of the web server's home page, including your line of custom text.

- To exit the command prompt on my-vm-1, execute this command: `exit`

You will return to the command prompt on my-vm-2

- To confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2, execute this command:
```
curl http://my-vm-1/
```
The response will again be the HTML source of the web server's home page, including your line of custom text.

---
# 2. Google Cloud Fundamentals: Getting Started with App Engine
---
### Overview
In this lab, you create and deploy a simple App Engine application using a virtual environment in the Google Cloud Shell.

### Objectives
In this lab, you learn how to perform the following tasks:
- Initialize App Engine.
- Preview an App Engine application running locally in Cloud Shell.
- Deploy an App Engine application, so that others can reach it.
- Disable an App Engine application, when you no longer want it to be visible.

### Steps
* **Initialize App Engine**
1. Sign in to the Google Cloud Platform (GCP) Console with your qwiklabs credentials in an incognito window.

2. Click on the Cloud Shell icon to activate the Google Cloud Shell.
![Select the cloud shell icon](https://storage.googleapis.com/practise-test-cloud-shell-icon/personal%20shell%20stuff.png)
- Click continue.
![continue](https://cdn.qwiklabs.com/lr3PBRjWIrJ%2BMQnE8kCkOnRQQVgJnWSg4UWk16f0s%2FA%3D)
3. Initialize App Engine with your project and choose its region.
```
gcloud app create --project=$DEVSHELL_PROJECT_ID
```
When prompted to select a region, select the region you want your App Engine application located in.
4. Clone the source code repository for a sample application in the hello_world directory:
```
git clone https://github.com/GoogleCloudPlatform/python-docs-samples
```
5. Navigate to the source directory:
```
cd python-docs-samples/appengine/standard_python3/hello_world
```
* **RUNNING THE HELLO WORLD APP.**
1. Execute the following command to download and update the packages list.
```
sudo apt-get update
```
2. Set up a virtual environment in which you will run your application. Python virtual environments are used to isolate package installations from the system.
```
sudo apt-get install virtualenv
```
If prompted [Y/n], press Y and then Enter.
```
virtualenv -p python3 venv
```
3. Activate the virtual environment.
```
source venv/bin/activate
```

4. Navigate to your project directory and install dependencies.
```
pip install  -r requirements.txt
```
5. Run the application:
```
python main.py
```
Please ignore the warning if any.

6. In Cloud Shell, click Web preview (Web Preview) > Preview on port 8080 to preview the application.
To access the Web preview icon, you may need to collapse the Navigation menu.
Result:
![hello world app](https://cdn.qwiklabs.com/vTRhzjVoW3LX%2BaFG6ox7ZExJHDQvTdMK8fAyRGBQCDQ%3D)
To end the test, return to Cloud Shell and press Ctrl+C to abort the deployed service.

* **Deploy and run Hello World on App Engine**
1. Navigate to the source directory:
```
cd ~/python-docs-samples/appengine/standard_python3/hello_world
```
2. Deploy your Hello World application.
```
gcloud app deploy
```
If prompted "Do you want to continue (Y/n)?", press Y and then Enter.
This app deploy command uses the app.yaml file to identify project configuration.

3. Launch your browser to view the app at http://YOUR_PROJECT_ID.appspot.com
```
gcloud app browse
```
4. Copy and paste the URL into a new browser window.
Result:
![app-engine-browser](https://cdn.qwiklabs.com/fnmJeOzuz%2BgxMdMg175OIbQRE84kwir5fKVcB1kXihg%3D)

Congratulations! You created your first application using App Engine.

* **Disable the application**
- App Engine offers no option to Undeploy an application. After an application is deployed, it remains deployed, although you could instead replace the application with a simple page that says something like "not in service."
However, you can disable the application, which causes it to no longer be accessible to users.

1. In the Cloud Console, on the Navigation menu, click App Engine > Settings.

2. Click Disable application.

3. Read the dialog message. Enter the App ID and click DISABLE.

If you refresh the browser window you used to view to the application site, you'll get a 404 error.

![404 error](https://cdn.qwiklabs.com/jVzvehqMDLGdJxcGG6aHjrT1zG6SRd443bZo%2BTO383I%3D)




