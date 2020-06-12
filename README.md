# Python Google API Examples

### Steve Godfrey

## Summary

In this repository are several examples in whihc Python code is calling Google APIs to access primarily Google Sheets and in a limited way Google Drive.  To demonstrate the functionality, the code is written in Jupyter notebooks and data from pandas dataframes are both read from and written to Google Sheets.

At this time, the notebook examples only use the pygsheets API [wrapper](https://pygsheets.readthedocs.io/en/stable/), but future repo updates with contain examples using just the raw API.


### Notebooks

* A demonstration using the service-file approach [pygsheets_service_file](Code/pygsheets_service_file.ipynb)
* A demonstration using the OAuth approach [pygsheets_OAuth](Code/pygsheets_OAuth.ipynb)

### References

* [Google Cloud Authentication](https://cloud.google.com/docs/authentication?_ga=2.163995511.-977261100.1589725507)
* [pygsheets](https://pygsheets.readthedocs.io/en/stable/)
* [Blog: Data Intevew Qs](https://www.interviewqs.com/blog/python_sheets)
* [Blog: Play with Google Spreadsheets](https://medium.com/@dcaichara/play-with-google-spreadsheets-with-python-301dd4ee36eb)


## Authentication

For a coded new to API authentication or using Google Cloud, establishing an invoking Google API credentials can be somewhat intimidating and a bit of work.  However, once the basics of the security model are understood and the credentials are in place, it will have been worth effort since, at that point, the G Drive communication will be mostly seamless.

The following detailed steps cover two authentication mechanisms:  (1) Service Account and (2) OAuth 2.0 Cient IDs and API Keys.  Overall, the Service Account approach is easier and therefore faster to implement, but it does face some limitations.  One limitation is that each Sheet file must be individually shared with the service account.  In my case, this is a manual process (although there may be some automated way to handle the task).  If the project will only write to or read from a small number of Sheets, the service-method mechanism may be fine.  A second limitation is that it may not work under certain G Suite security settings.  At my last company, the G Suite applications were restricted from sharing with users outside the organization's specific domains and this blocked Service Account authentication.

OAuth 2.0 can provide broader Drive and G-Suite application access, but users are prompted and can control application access to their Drives.  In these examples, this approach will drop a credential file into the notebook's directory and one should be careful not to share (or post to GitHub) that file.


### Google Cloud

Both of these authentication methods require a Google Cloud ID created at the Google Cloud portal.  That should be the first step in establish autehentication credentials.

<img src="Images/GoogleCloud.png" alt="alt text" width="800"/>
&nbsp;


### Google Cloud Project

Both methods create credentials for a specific project, and therefore require the creation of a project as the first step after the Cloud Accout has been established.  In addition, this project needs to have APIs assigned to it.  In our case, we are going to add the Google Drive and Google Sheets APIs as noted in step 3 below.

1. Create a new or use an existing project at the Google Cloud [Console](https://console.developers.google.com)
  1. Select a project, then hit New Project in the pop-up window
  2. Give it a name and an organization (although no org is fine)
<img src="Images/Create_project_2.png" alt="alt text" width="800"/>
&nbsp;

2. Select this or another project, if it's not already selected
  1. If requested, select external app
  2. Name the app
<img src="Images/Create_project.png" alt="alt text" width="800"/>
&nbsp;

3. Enable APIs and Services by select the '+' or searching the Library
  1. Google Drive
  2. Google Sheets API
<img src="Images/API_library.png" alt="alt text" width="800"/>
&nbsp;

For this project, I created both Service and OAuth credentials, and they can be seen in the image below (OAuth in orange and Service Account in green)
<img src="Images/CredentialsThisProject.png" alt="alt text" width="800"/>
&nbsp;


### Service Accounts

The following are the steps to create a service-file credential for the project created above.

1. Create credentials selecting a service account
<img src="Images/CreatingServiceAccount_1.png" alt="alt text" width="800"/>
&nbsp
2. Enter details about the service file
<img src="Images/CreatingServiceAccount_2.png" alt="alt text" width="800"/>
&nbsp
3. Add users to this service account (optional) and complete
<img src="Images/CreatingServiceAccount_3.png" alt="alt text" width="800"/>
&nbsp
4. At this point, we need to create a key for this Service Account
  1. From a Console screen with access to Service Accounts (e.g. Service Accounts menu item), create a private key
<img src="Images/CreateServiceAccount_PrivateKey_1.png" alt="alt text" width="800"/>
&nbsp; 
  2. Save it to your computer
<img src="Images/CreateServiceAccount_PrivateKey_2.png" alt="alt text" width="800"/>
&nbsp;
5. Open the Google Sheet and share it with the XXX@YYY.iam.gserviceaccount.com
<img src="Images/ShareSheetWithServiceAccount_1.png" alt="alt text" width="800"/>
&nbsp;
<img src="Images/ShareSheetWithServiceAccount_2.png" alt="alt text" width="800"/>
&nbsp;

<strong> At this point, you should be ready to use these credentials in Python code; See the [pygsheets_service_file](Code/pygsheets_service_file.ipynb) notebook for example code </strong>



### OAuth 2.0 Client IDs and API Keys

The following are the steps to create a service-file credential for the project created above.

1. Create OAuth credentials 
  1. If needed, Configure Consent screen
     1. Select External at the User Type screen
<img src="Images/Configure_consent_screen_first_step.png" alt="alt text" width="800"/>
&nbsp;
     2. Complete the OAuth consent screen by naming app and adding required URLs
<img src="Images/OAuth_consent_screen_details.png" alt="alt text" width="800"/>
&nbsp;     
2. Select Application type - Desktop app
<img src="Images/Create_credentials_app_type.png" alt="alt text" width="800"/>
&nbsp;     
3. Download credentials file with file name like client_secret_XXX-YYY.apps.googleusercontent.com.json;  It's common to store in a dedicated credentials directory (that is not uploaded to GitHub)
<img src="Images/Create_credentials_download_secret_file.png" alt="alt text" width="800"/>
&nbsp;  
4. Create API Key; Save the API credentials with a common approach to save the API key in a JSON file in the credential directory (rather than saving as a Python variable in a notebook)
<img src="Images/Create_api_key.png" alt="alt text" width="800"/>
&nbsp;  

  <strong> At this point, you should be ready to use these credentials in Python code; See the [pygsheets_OAuth](Code/pygsheets_OAuth.ipynb) notebook for example code </strong>



#### The first time using these credentials

1. The first time this notebook is run, you will be requested to provide this application (i.e. this Python script) access to your Google Drive as noted below
<img src="Images/Authorize_app_to_access_google.png" alt="alt text" width="800"/>
&nbsp; 
2. Follow the prompts noting that you will likely get unverified message which is expected since Google has verified this in-development app
<img src="Images/App_isnt_verified_advanced_clicked.png" alt="alt text" width="800"/>
&nbsp;  
3. Follow the prompts granting the application access to your Google Drive (most likely granting all permissions); You will receive a code which is copied back into the notebook cell requesting a code
<img src="Images/App_authorize_code.png" alt="alt text" width="800"/>
&nbsp;
4. This will create a JSON file stored in the notebook's directory, probably named
  <em>sheets.googleapis.com-python.json</em>; you should leave it there but not copy to GitHub
  
