[Go to ATP Overview Page](../../ATP/readme.md)

![](../../common/images/customer.logo2.png)
# Microservices on ATP

## Part 2: Build a Container image with the aone application runing on ATP

#### **Introduction**

In this lab, you’ll learn how to build a Docker image for a Node.js REST service on Oracle Developer Cloud Service (DevCS), using an ATP database as it's data source.

Let’s get started! 

## Steps

### Step 1: Configure the connection to your OCIR Docker Repository

Open your project in Develoepr Cloud, and follow the steps below:

- Click **Docker** in the left navigation bar, then click **Link External Repository**.

  ![](images/650/im08-2.png)

- In the Dialog Box, enter following parameters

  - Registry Name: **MyOCIR**
  - Registry URL: fill in the URL of your OCI Repository.  Example for an instance based in Frankfurt:
     https://fra.ocir.io  , but replace the "fra" by the correct code : **phx** for Phenix,  **lhr** for London, ...
  - The **Username** is composed of {cloud-tenancy-name}/{username}, for example **gse00014089/api.user** 
  - Type your password **token** in the **Password** field - **attention, this is not the password** ! Typically a string looking like : i!co>5426CWaLZ&_Zh!r

  ![](images/650/im01.png)

- Now click **Create**.  Your credentials will be checked, and if they are correct you should see the list of objects that are already in the repository (this will look different on your instance)

  - ![](images/650/im02.png)

### Step 2: Configure the Docker build job for building your container - create the job and get required libraries

- Click **Build** in the left nav bar, then click **New Job**. 

  ![](images/650/image034.png)

- In the New Job dialog: 
  - Type **NodeJSDockerOCIR** in the **Job Name** field 

  - Type Build and push Docker image to OCIR in the **Description** field 

  - Select the build template you set up, for example **OKE2** from the **Software Template** drop-down  

  - Click **Create Job**.

    ![](images/650/image035.png)

- From the **Add Source Control** dropdown, select Git.

  ![](images/650/image036.png)

- Select your **Repository.git** from the **Repository** drop-down.

- Select master from the **Branch** drop-down.

- Leave the **Automatically perform build on SCM commit** check box **unchecked**.

  ![](images/650/image037.png)

  

- Navigate to **Steps**, and use the **Add Step** button to select **Unix Shell** and enter the below commands in the **Script** field

```
    chmod 400 atpkey
    scp -o 'StrictHostKeyChecking no' -i atpkey opc@130.61.120.69:/home/opc/jle/instantclient-basic-linux.x64-12.1.0.2.0.zip instantclient-basic-linux.x64-12.1.0.2.0.zip
    ls -al 
```

​	![](images/650/image_unix.png)

- Explanation of these operations:
  We need the library to access the ATP database, called **instantclient** in the environment where we will execute the Docker Build operation, so we can include it in the container.  Since this is a "Licensed" library that can be downloaded from the Oracle website by accepting the T&C's, we automate this operation by supplying a copy on a running Compute instance with the IP address 130.61.120.69.

  
  

### Step 3: Add more steps to the build: Execute the Docker commands

- Use the **Add Step** button and add a step of type **Docker Builder->Docker login**. 

  - Use the dropdown of the field **Registry Host** to select the Repository configuration you just ceated (named **MyOCIR**.  The username and password field are automatically filled in now.

    ![](images/650/image038.png)





- Using the **Add Step** drop-down, select **Docker Builder->Docker build**. 

  - Select the **MyOCIR** registry from the dropdown field of the  **Registry Host** field (should be pre-filled in)
  - The **Image Name** is composed as follows: my_instance_name/your_repo_name/image_name
    - Example : oractdemeabdmnative/jle_repo/atp01
    - Use your initials in the repo name to distinguish from other users
  - In the **Source** radio buttons, click **Context root in Workspace**.

- From the **Add Builder** drop-down, select **Docker Builder->Docker push**. 
  - Your **Registry Host** and **Image Name** should be pre-filled with the previously specified values.

  ![](images/650/im46.png)

- Make sure to check the **Full Image Name** displaying on the Docker Build and Docker Deploy steps, they should be the same !

- Click **Save**.

  ![](images/650/image040.png)


### Step 4: Configure some scripts to point to your environment

Before we can run the Build Job we just created, we need to parametrize some scripts to be pointing to your specific environment.

- Navigate to the **Git** page of your Developer project

- Open the file **Dockerfile**, and hit the Edit button (pencil icon upper right) to change the file

- On line 17, change the **COPY** comman to reflect the name of your wallet directory:

  - For example, if your wallet folder is named **APT1**, replace the original line with the blow text:

    ```dockerfile
    COPY ./wallet_ATP1 ./wallet_NODEAPPDB2
    ```

- Hit the **Commit** button (upper right) to save your changes.

- Now navigate into your wallet folder by clicking on it, then select the file **sqlnet.ora** and hit the "Edit" pencil icon.

  - In this file, replace the default line below:

  - ```
    WALLET_LOCATION = (SOURCE = (METHOD = file) (METHOD_DATA = (DIRECTORY="?/network/admin")))
    ```

    

  - by this line:

  - ```
    WALLET_LOCATION = (SOURCE = (METHOD = file) (METHOD_DATA = (DIRECTORY=$TNS_ADMIN)))
    ```

  - This will allow us to simply set the environment variable $TNS_ADMIN to the correct path.

- Finally, you need to ensure the docker image has the right connection information for connecting to the database.  Navigate to the folder **aone/scripts**, and locate the file called **dbconfig.js**

  - In this file, enter the username, password and connect string of your ATP database.  This is just a crude way of simply setting up connectivity, this should be parametrized in a real-world deployment!
  - The connect string is of the form **atp2_high**, where atp2 would be your datbase name.
  - Hit the **Commit** button to save the modifications.

- You are now ready to try out your Build Job in the next step!



### Step 5: Run the Build Job

Before we move on, we want to ensure the job we just created works correctly. 

- Hit the **Build Now** button to kick off the build. 

![](images/650/im10.png)

If this is the very first time you invoke the executor, this might take some time (up to 10 minutes), as the Executioner needs to be instantiated and launched.  Consecutive builds will start almost immediately.

![](images/650/im11.png)

Once the job finished successfully, you should see something like this :

![	](images/650/im13.png)

- Navigate to your OCI console to check the container has indeed been uploaded
  - On the menu, select **Developer Services**, and then **Registry**

![](images/650/im12.png)



- You should see your container in the list, deployed just a few minutes ago
  ![](images/650/im45.png)



You have finished this part of the lab, navigate to the next step to continue!


---
