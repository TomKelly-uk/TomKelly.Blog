# Entra AD Sync

![Entra Sync FLow](http://localhost:4000/assets/img/Entr%20AD%20User%20Sync/Entra%20Sync%20Flow.png)

**Manually retrieve link and store it**
The easiest way to do this is by building your query in Microsoft Graph API, and taking the link from there. 

Using the following URI, 'https://graph.microsoft.com/v1.0/users/delta' we can retrieve the delta link called '@odata.nextLink', as shown in the screenshow below. 

Next we just need to store it in our new table. 

If we navigate to this table in Power Platform, we can conveniently enter the data straight into the table from there. 
