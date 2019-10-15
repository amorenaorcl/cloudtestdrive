

![](../../common/images/customer.logo2.png)
# Microservices on ATP #
## Preparing your Oracle Cloud Tenancy for the lab ##

## Introduction ##

In case you are using a personal instance, either obtained via a free trial or using an existing paying account, you need to ensure the basic services for this lab are up and running, and ensure your user has the right entitlements to execute this lab successfully.

This page will guide you through the following activities :

- Set up Developer Cloud Service
- Create a compartment called CTDOKE which we will use later in the configuring OKE via Terraform lab
- Add a Policy statement for OKE
- Create an API user with a certificate
- Install some software on your local machine:
  - git
  - kubectl
  - terraform
  - Sql Developer

### Step 1: Setting up Developer Cloud Service ###

This step will guide you through the setup of a new Developer Cloud instance :

- Enabling DevCS on your Dashboard
- Creating a Developer Cloud instance
- Configuring the Storage and Build parameters for your DevCS instance

#### Enable DevCS on your PaaS dashboard ####

- Login to your cloud account and navigate to the PaaS dashboard:

![alt text](images/devcs/dashboard.png)

In case your cloud account is linked straight to the OCI dashboard, you need to use the following menu item on the left to reach the PaaS dashboard:

![alt text](images/devcs/gotopaas.png)


- Make sure the "Developer Cloud" service is "visible" on the dashboard as in the above screenshot.  If this is not the case, use the "Customize Dashboard" button to enable it.

![alt text](images/devcs/customize.png)

#### Create an instance ####

- Go into the Developer Cloud Service Overview by clicking on the Service title

![alt text](images/devcs/service.png)

- Open the Service Console.  You should have no existing instances.  If you have, you can skip the following steps and just validate you have a build engine witht the correct libraries included.

![alt text](images/devcs/empty.png)

- Use the "Create Instance" button to create a new Developer Cloud instance

![alt text](images/devcs/create.png)

- Hit the "Next" button and then "Create"

![alt text](images/devcs/confirm.png)

- Now the instance is being created.  This will take a few minutes, you can hit the small arrow to requery the status.

![alt text](images/devcs/creating.png)

#### Access your DevCS Environment ####

To access your Developer Cloud Instance, use the hamburger menu on the right to view the menu item **Access Service Instance**.  Right-click to save the URL, you will need this link later in the labs.



#### Configuring your DevCS Instance ####

Once the instance is available, you need to configure a few things to be able to create projects and run builds:

- Create a dedicated user with specific privileges
- Add a public key to the user profile
- Create a group for DevCS users
- Add a DevCS policy in the **root** compartment
- Then configure the OCI connection using the references to these OCI objects you just created.

You will use the **OCI** type of setup, please ignore the *OCI Classic* setup instructions.

A detailed explanation of these steps is provided in [this section of the Developer Cloud Documentation](https://docs.oracle.com/en/cloud/paas/developer-cloud/csdcs/service-setup.html#GUID-0FCE0C4F-75F4-43BC-8699-EBE039DA5E7A).  Navigate to that page, then use the **Back** button of your browser to returrn to this location.

###############################
Set Up the OCI Connection
Before you set up the connections, set up your OCI account to host DevCS resources. They allow DevCS to manage necessary resources, such as VMs for your builds and storage buckets for your project data.

To set up the OCI account, open the OCI console and create a compartment, a group and a user to access the compartment, and a policy that defines access to the compartment.
You can use the root compartment and the tenancy user that was created when the OCI account was created, but it's recommended to create a dedicated compartment to host DevCS resources. This allows you to organize DevCS resources better as they aren't mixed with the other resources of your tenancy. You can also restrict users and control read-write access to the compartment without affecting other resources. To learn more about compartments, see Understanding Compartments.

After setting up your OCI account, share the compartment's and the created user's details with the DevCS Organization Administrator to set up the OCI connection in DevCS.

- Set Up the OCI Account
On the OCI Console, click the Menu icon in the top-left corner.
Under Governance and Administration, select Identity, and then select Compartments.
On the Compartments page, create a compartment to host DevCS resources.
To create the compartment in the tenancy (root compartment), click Create Compartment.
In the Create Compartment dialog box, fill in the fields, and click Create Compartment.
Here's an example:
OCI Create Compartment dialog box
To learn more about compartments, see Working with Compartments.
Create a user to access the DevCS compartment.
In the left navigation bar, under Governance and Administration, go to Identity and click Users.
Click Create User.
In the Create User dialog box, fill in the fields, and click Create.
Here's an example:

Create User dialog box
To learn more about OCI users, see Working with Users.
On your computer, generate a private-public key pair in the PEM format.
To find out how to generate a private-public key pair in the PEM format, see How to Generate an API Signing Key.
Here's an example of private-public key files on a Windows computer:

Private and Public key files
Upload the public key to the user's details page.
Open the public key file in a text editor and copy its contents.
In the left navigation bar of the OCI dashboard, click under Governance and Administration, go to Identity and click Users.
Click the user's name created in Step 3.
In the User Details page, click Add Public Key.
Here's an example:

User Details page
In the Add Public Key dialog box, paste the contents of the public key file, then click Add.
To learn more about uploading keys, see How to Upload the Public Key.
On the Groups page, create a group for the user who can access the DevCS compartment and add the user to the group.
In the left navigation bar, under Governance and Administration, go to Identity and click Groups.
Click Create Group.
In the Create Group dialog, fill in the fields and click Submit.
Here's an example:

Create Group dialog box
On the Groups page, click the group's name.
On the Group Details page, click Add User to Group.
In the Add User to Group dialog box, select the user created in Step 3, and click Add.
Here's an example:

Add user to a group
To learn more about groups, see Working with Groups.
In the root compartment, not the DevCS compartment, create a policy to allow the group created in step 6 to access the DevCS compartment.
In the left navigation bar, under Governance and Administration, go to Identity and click Policies.
On the left side of the Policies page, from the Compartment list, select the root compartment.
Click Create Policy.
In Name and Description, enter a unique name and a description.
In Policy Statements, add these statements.
allow group <group-name> to manage all-resources in compartment <compartment-name>
This grants all permissions to the DevCS group users to manage all resources within the DevCS compartment.

allow group <group-name> to read all-resources in tenancy
This grants read permissions to the DevCS group so that its users can read—but not use, create or modify—all resources inside and outside the DevCS compartment. The group users can't use, create, or modify the resources. This statement is optional.

Here's an example:

Create Policy dialog box
Click Create.
To learn more about policies, see Working with Policies.
Get the Required OCI Input Values
Every Oracle Cloud Infrastructure resource has an Oracle-assigned unique ID called an Oracle Cloud Identifier (OCID). To connect to OCI, you need the account's tenancy OCID, home region, the compartment's OCID that hosts DevCS resources, and the OCID and fingerprint of the user who can access the DevCS compartment. To connect to OCI Object Storage, you need the Storage namespace. You can get these values from the OCI Console pages.

This table describes how to get the OCI input values required for the connection.

To get these values ...	Do this:
Tenancy OCID, Home Region, and Storage Namespace	On the OCI console, from the left navigation bar, select Administration > Tenancy Details.
The Tenancy Information tab displays the tenancy OCID in OCID, home region in Home Region, and the storage namespace in Object Storage Namespace.

Here's an example:

Description of oci_tenancyinformation.png follows
Description of the illustration oci_tenancyinformation.png
User OCID and Fingerprint	On the OCI console, from the left navigation bar, under Governance and Administration, select Identity > Users.
The User Information tab displays the user OCID in OCID. Click the Copy link to copy it to the clipboard.

Here's an example of devcs.user:

Description of oci_userinformation.png follows
Description of the illustration oci_userinformation.png
To get the fingerprint of the public key associated with your OCI account, scroll down to the API Keys section and copy the fingerprint value.

Description of oci_apikeys.png follows
Description of the illustration oci_apikeys.png
Compartment OCID	On the OCI console, from the left navigation bar, select Identity > Compartments.
The Compartments list displays the compartments with the compartment OCID in the OCID field. Click the Copy link to copy it to the clipboard.

Here's an example:

Description of oci_compartments.png follows
Description of the illustration oci_compartments.png
Set Up the OCI Connection in DevCS
To connect to OCI, get the DevCS compartment's details, user details, and the required OCID values. Then, create an OCI connection from DevCS. If you're not the OCI administrator, get the details from the OCI administrator.

In the navigation bar, click Organization Organization.
Click the OCI Account tab.
Click Connect.
OCI Account tab
In Account Type, select OCI.
In Tenancy OCID, enter the tenancy's OCID copied from the Tenancy Details page.
In User OCID, enter the OCID of the user who can access the DevCS compartment.
In Home Region, select the home region of the OCI account.
In Private Key, enter the private key of the user who can access the DevCS compartment.
The private key file was generated and saved on your computer when you created the private-public key pair in the PEM format. See Step 4 in Set Up the OCI Account.
In Passphrase, enter the passphrase used to encrypt the private key. If no passphrase was used, leave the field empty.
In Fingerprint, enter the fingerprint value of the private-public key pair.
In Compartment OCID, enter the compartment's OCID copied from the Compartments page.
In Storage Namespace, enter the storage namespace copied from the Tenancy Details page.
To agree to terms and conditions, select the terms and conditions check box.
To validate the connection details, click Validate.
After validating the connection details, click Save.
Here's an example of an OCI Account tab filed with required OCI details.
OCI Account tab filled with OCI credentials and other details
###############################

#### Create a Virtual Machine per build jobs

- On the left-side menu, select the top level **Organization** menu, then click on **Virtual Machines Templates** in the top menu.  Next you can hit the **Create Template** button.

![alt text](images/devcs/NewTemplate2.png)

- In the dialog box, specify a name, for example **DockerOCIOKE**  and use the default **Oracle Linux 7** image.  Then hit the **Create** button.

  ![alt text](images/devcs/im04.png)


- Now select the template you just created (DockerOCIOKE), and add the required software packages by clicking on the **Configure Software** button.

![alt text](images/devcs/im05-2.png)

- Select the following packages:
  - Docker 17,2
  - Kubectl
  - OCIcli ==> this will prompt you to also install Python3
  - SQLcl 18

![alt text](images/devcs/im06-2.png)

- Finally, navigate to the **Build Virtual Machines** menu on the top menu, and hit the **+ Create VM** button.

  ![alt text](images/devcs/im07-2.png)

  In the dialog that pops up, enter following values:
  
  - Choose **Quantity = 1**
  
  - Select the **VM Template** you just created: **DockerOCIOKE**
  
  - Set the **Region** to **eu-Frankfurt-1**
  
  - Select the compute **Shape** : **VM.Standard2.2**
  
    ![alt text](images/devcs/im08.png)

You finished all the steps to finalize the Developer Cloud setup.  

### **STEP 2: Create an OCI Compartment (which we will use later in the configuring OKE via Terraform lab)**

- In the Cloud Infrastructure Console, click on the hamburger menu on the top left of the screen. From the pull-out menu, under Identity, click Compartments.

![](images/100/Compartments.jpeg)

- You will see the list of compartments currently available in your instance, which will include at least the root compartment of your tenancy (with has the tenancy name). 
  - ![](images/100/ListCompartmentsCTDOKE.png)
- Click on **Create Compartment** button to start the compartment creation process

![](images/100/CreateCompartment4.png)

Enter the following in create Compartment window

- **Name**: Enter **CTDOKE**
- **Description**: Enter a description for the compartment
- **Parent Compartment**:  select the root compartment.
- Click on the **Create Compartment** link 

- You can verify the compartment created on Compartments page

### **STEP 3**: Add a Policy Statement for OKE

- Before the Oracle managed Kubernetes service can create compute instances in your OCI tenancy, we must explicitly give it permission to do so using a policy statement. From the OCI Console navigation menu, choose **Identity->Policies**.

  ![img](images/devcs/LabGuide200-13c980fa.png)

- In the Compartment drop down menu on the left side, choose the **root compartment**. It will have the same name as your OCI tenancy (Cloud Account Name).

  ![img](images/devcs/LabGuide200-a321171a.png)

- Click **PSM-root-policy**

  ![img](images/devcs/LabGuide200-e67b7705.png)

- Click the **Add Policy Statement** button

  ![img](images/devcs/LabGuide200-3d4a7471.png)

- In the Statement box, enter: `allow service OKE to manage all-resources in tenancy` and click **Add Statement**

  ![img](images/devcs/LabGuide200-bd5bcbd1.png)


### STEP 4: Create an OCI API user with a certificate

- Add an API (non-SSO) user with an API key:

  - Navigate to the "Identity" , "Users" screen and add a user called "api.user"

  - Add an API key: you need a private/public key pair, and you need to paste the public one into the key field. 

    - On a Mac : open a console window and execute following commands

    - ```
      mkdir ./mykey
      openssl genrsa -out ./mykey/api_key.pem 2048
      openssl rsa -pubout -in ./mykey/api_key.pem -out ./mykey/api_key_public.pem
      ```

    - On a Windows PC, you can use [puttygen](https://www.ssh.com/ssh/putty/download).exe to create a key.

  - Copy the fingerprint of your API key in a temporary file

  - Copy the OCID of this new user in a tempporary file

  ![](images/660/OkeUser.png)


- Create an Auth Token for the user api.user

  - Take care to copy the Token string in a file on your PC : you will nbeed it later, and you cannot retrieve it back from the console.

    ![](images/660/Auth_token.png)

  
### STEP 4: Install the required software on your laptop

#### Installing git

In order to easily update and upload files into your Developer repository, we will clone the newly created DevCS repository onto your machine.  For this you need to install **git** on your laptop.   

- Download the software for your OS on [this location](https://git-scm.com/downloads) 
- In this tutorial we will use the command line to execute the required git operations, but if you have a git GUI installed (like GitHub Desktop or GitKraken) you can execute the equivalent operations through these tools.

#### Installing kubectl

This page covers how to install the Kubernetes command line interface and connect to your Kubernetes cluster

Choose the section that corresponds to your machine:

##### MacOS

Download the latest release with the following `curl` command:

```
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 49.9M  100 49.9M    0     0  4289k      0  0:00:11  0:00:11 --:--:-- 4150k
```

Make the kubectl binary executable.

```
$ chmod +x ./kubectl
```

Move the binary in to your PATH.

```
$ mv ./kubectl /usr/local/bin/kubectl
```

Verify the installation using the version command.

```
$ kubectl version
Client Version: version.Info{Major:"1", Minor:"8", GitVersion:"v1.8.4", GitCommit:"9befc2b8928a9426501d3bf62f72849d5cbcd5a3", GitTreeState:"clean", BuildDate:"2017-11-20T05:28:34Z", GoVersion:"go1.8.3", Compiler:"gc", Platform:"linux/amd64"}
The connection to the server localhost:8080 was refused - did you specify the right host or port?
```

At this step the server connection failure is normal. For easier usage it is recommended to setup the autocomplete for bash.

```
$ source <(kubectl completion bash)
```

##### Linux machines

For Linux, use the same sequence as described above for Linux, only replace the CURL command with the following:

```
$ curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 49.9M  100 49.9M    0     0  4289k      0  0:00:11  0:00:11 --:--:-- 4150k
```

​	Note the difference: you are using /bin/linux/... instead of /bin/darwin/...
​

##### Windows

To find out the latest stable version take a look at [https://storage.googleapis.com/kubernetes-release/release/stable.txt](https://storage.googleapis.com/kubernetes-release/release/stable.txt)

For example if latest stable version is: **v1.8.4** then construct the download link in the following way: *https://storage.googleapis.com/kubernetes-release/release/VERSION_NUMBER/bin/windows/amd64/kubectl.exe*. Thus in case of **v1.8.4** the link looks like this:

[https://storage.googleapis.com/kubernetes-release/release/v1.8.4/bin/windows/amd64/kubectl.exe](https://storage.googleapis.com/kubernetes-release/release/v1.8.4/bin/windows/amd64/kubectl.exe)

Once you have the executable binary add to your PATH variable.

```
set PATH=%PATH%;c:\download_folder\kubectl.exe
```

Verify the installation using the version command.

```
C:\Users\pnagy>kubectl version
Client Version: version.Info{Major:"1", Minor:"7", GitVersion:"v1.7.0", GitCommit:"d3ada0119e776222f11ec7945e6d860061339aad", GitTreeState:"clean", BuildDate:"2
017-06-29T23:15:59Z", GoVersion:"go1.8.3", Compiler:"gc", Platform:"windows/amd64"}
Unable to connect to the server: dial tcp 192.168.99.100:8443: connectex: A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond.
```


---

#### Installing terraform

Terraform is an open-source infrastructure as code software tool created by HashiCorp. It enables users to define and provision a datacenter infrastructure using a high-level configuration language known as Hashicorp Configuration Language

- Go to the [Hashicorp Terraform website](https://www.terraform.io/downloads.html) to download the software for your OS
- unzip the executable file in the directory of your choice
- Add the terraform command to your path
  - On Mac: export PATH=$PATH:`pwd`
  - On Windows: go to System Steetings, Advanced, Environment Variables, and add the path to your Terraform directory 


##### Installing Oracle SQL Developer

You should have Oracle SQL Developer installed on your machine. If not, you can download the latest version from [here](https://www.oracle.com/technetwork/developer-tools/sql-developer/downloads/index.html).



---

