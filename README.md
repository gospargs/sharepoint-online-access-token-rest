### SharePoint Application Registration and Access Token Retrieval

1. **Prerequisite**: The user needs to be a SharePoint Administrator.

2. **Download SharePoint Online Manager PowerShell**.

3. **Execute these commands** in PowerShell:
   - Connect to the SharePoint service:
     ```
     Connect-SPOService -Url <YOUR-SHAREPOINT>-admin.sharepoint.com
     ```
   - Set the Tenant settings to allow management of legacy service principal:
     ```
     Set-SPOTenant -SiteOwnerManageLegacyServicePrincipalEnabled $true
     ```

4. **Access SharePoint App Registration** at "https://<YOUR-SHAREPOINT>.sharepoint.com/_layouts/15/appregnew.aspx."

5. **Fill out the form** and select 'Create.'

6. **Copy the information** from the resulting page. 

7. **Access SharePoint App Permissions** at "https://<YOUR-SHAREPOINT>.sharepoint.com/_layouts/15/appinv.aspx."

8. **Fill out the form** with the following details:
   - App Id: PASTE YOUR CLIENT ID and press 'lookup.'
   - Permission Request XML:
     ```
     <AppPermissionRequests AllowAppOnlyPolicy="true">
       <AppPermissionRequest Scope="http://sharepoint/content/sitecollection" Right="FullControl"/>
     </AppPermissionRequests>
     ```
   At the end, press 'create.'

9. You will be asked if you trust this application. Click on "Trust It," and you should be redirected to the Site Settings.

10. Click on Site app permissions. You will get an app identifier that looks like this:
    ```
    i:0i.t|ms.sp.ext|<THIS PART IS YOUR CLIENT ID>@<THIS PART IS YOUR TENANT ID>
    ```

11. Now you should have all the information you need:
    - Client Id
    - Client Secret
    - Title
    - App Domain
    - Redirect URI
    - Tenant Id
    - App Identifier:
      ```
      i:0i.t|ms.sp.ext|<THIS PART IS YOUR CLIENT ID>@<THIS PART IS YOUR TENANT ID>
      ```

12. **Getting an access token in Postman**:

    - **URL**: https://accounts.accesscontrol.windows.net/<YOUR TENANT ID>/tokens/oAuth/2
    - **BODY**:
      ```
      grant_type: client_credentials
      client_id: <THIS PART IS YOUR CLIENT ID>@<THIS PART IS YOUR TENANT ID>
      client_secret: <YOUR CLIENT SECRET>
      resource: 00000003-0000-0ff1-ce00-000000000000/<YOUR SHAREPOINT>.sharepoint.com@<YOUR TENANT ID>
      ```

13. The response should be:
    ```json
    {
        "token_type": "Bearer",
        "expires_in": "86399",
        "not_before": "1699307907",
        "expires_on": "1699394607",
        "resource": "00000003-0000-0ff1-ce00-000000000000/<YOUR SHAREPOINT>.sharepoint.com@<YOUR TENANT ID>",
        "access_token": "<THE TOKEN>"
    }
    ```
