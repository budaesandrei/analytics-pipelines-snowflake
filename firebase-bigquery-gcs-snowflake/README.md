# Analytics Pipeline DevOps - Firebase to Snowflake

## Setup the Firebase Poject

1. To setup a firebase project navigate to this [link](https://console.firebase.google.com/) and click on **Create a project** and follow the setup on the screen. Make sure to **Enable Google Analytics** for the project
2. Navigate to the **Project Settings** > **Integration** > **Big Query** > **Manage** and make sure that **Google Analytics** from the **Exported Integrations** is **On** and in the **Export Settings** tick at least **Daily** (*Note: You will need to switch to the Blaze Plan to enable Streaming*)
3. After a few days of app runtime, the event data should have collected in Big Query. Navigate to [Google Cloud Console](https://console.cloud.google.com/) and enable **Billing**

## Authentication

### Google Cloud Console

There are various ways to authenticate but in this template we will create and use a **Service Account**:
1. Navigate to [IAM & Roles](https://console.cloud.google.com/iam-admin/serviceaccounts) and select your project
2. Click **+ CREATE SERVICE ACCOUNT** and add a service account name (e.g. bigquery-python), add a **description** (optional) and click **CREATE AND CONTINUE**
3. Add **Roles** from the dropdown and **+ ADD ANOTHER ROLE** as many times as needed then **CONTINUE**:
    * Add **Basic** > **Owner**
4. Click **Done**
5. Once the **Service Account** has been created, click on the three dots under **Action** and click **Manage Keys**
6. From the next page, click on **ADD KEY** > **Create New Key** > **JSON** > **Create**
7. This will download a one time file on your computer. **DO NOT LOSE THIS FILE** as it cannot be downloaded again. If you do lose it, you can delete the current key and create a new one (from Step 5)
8. Rename the file to **"gccreds.json"** and drop it in the same folder as this notebook (if you want to keep it in more secure location I suggest you save the path in an environment variable)

### Snowflake

1. Create an account in Snowflake (e.g. data_loader) with a role that has **ACCOUNTADMIN** rights (e.g. data_loader and assign the accountadmin role to this role)
2. (For Windows) In the Search Bar search for **Environment Variables** and open **Edit the system environment variables** 
3. Click on **Environment Variables** and in the **User variables** section click **New**
4. In the **Variable name** type in **ETL_SN_CREDS**
5. In the **Variable value** insert a **JSON** in the format
```JSON
{"username":"data_loader", "password":"<PASSWORD>","account":"<ACCOUNT>","warehouse":"<WAREHOUSE>","role":"data_loader"}
```

## Enable Google Cloud APIs

To access services programatically, Google requires you to manually enable some APIs:
1. Enable the [Access Management (IAM) API](https://console.cloud.google.com/flows/enableapi?apiid=iam.googleapis.com)

## Install Dependencies

Open a python terminal (I use **Anaconda Prompt**) and run
```python
pip install -r requirements.txt
```