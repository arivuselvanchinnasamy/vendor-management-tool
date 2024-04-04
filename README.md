# vendor-management-tool
Vendor Management Tool is a data entry form that integrates seamlessly with Google Sheets using Streamlit and the streamlit-gsheets-connection library.

# Install
`pip install st-gsheets-connection`

`pip install streamlit`

`pip install pandas`

# Service account / CRUD example
## Initial setup for private spreadsheet and/or CRUD mode
1. Setup .streamlit/secrets.toml inside your Streamlit app root directory, check out Secret management documentation for references.
2. Enable API Access for a Project
-- Head to Google Developers Console and create a new project (or select the one you already have).
-- In the box labeled “Search for APIs and Services”, search for “Google Drive API” and enable it.
-- In the box labeled “Search for APIs and Services”, search for “Google Sheets API” and enable it.
3. Using Service Account
- Enable API Access for a Project if you haven’t done it yet.
- Go to “APIs & Services > Credentials” and choose “Create credentials > Service account key”.
- Fill out the form
- Click “Create” and “Done”.
- Press “Manage service accounts” above Service Accounts.
- Press on ⋮ near recently created service account and select “Manage keys” and then click on “ADD KEY > Create new key”.
- Select JSON key type and press “Create”.

You will automatically download a JSON file with credentials. It may look like this:
```json
{
  "type": "service_account",
   "project_id": "api-project-XXX",
    "private_key_id": "2cd … ba4",
    "private_key": "-----BEGIN PRIVATE KEY-----\nNrDyLw … jINQh/9\n-----END PRIVATE KEY-----\n",
    "client_email": "473000000000-yoursisdifferent@developer.gserviceaccount.com",
    "client_id": "473 … hd.apps.googleusercontent.com",
  "client_email": "api-project-xx@xxx.iam.gserviceaccount.com",
  "client_id": "1234567890",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "<cert_url>",
  "universe_domain": "googleapis.com"
}
```
- Remember the path to the downloaded credentials file. Also, in the next step you’ll need the value of client_email from this file.

- :red[Very important!] Go to your spreadsheet and share it with a client_email from the step above. Just like you do with any other Google account. If you don’t do this, you’ll get a gspread.exceptions.SpreadsheetNotFound exception when trying to access this spreadsheet from your application or a script.

Inside streamlit/secrets.toml place service_account configuration from downloaded JSON file, in the following format (where gsheets is your st.connection name):

    # .streamlit/secrets.toml
    
    [connections.gsheets]
    spreadsheet = ""
    worksheet = "<worksheet-gid-or-folder-id>"  # worksheet GID is used when using Public Spreadsheet URL, when usign service_account it will be picked as folder_id
    type = "service_account"  # leave empty when using Public Spreadsheet URL, when using service_account -> type = "service_account"
    project_id = ""
    private_key_id = ""
    private_key = ""
    client_email = ""
    client_id = ""
    auth_uri = "https://accounts.google.com/o/oauth2/auth"
    token_uri = "https://oauth2.googleapis.com/token"
    auth_provider_x509_cert_url = "https://www.googleapis.com/oauth2/v1/certs"
    client_x509_cert_url = ""

