# Backup Site Collection

You can backup a site collection using Central Administration, PowerShell or
programmatically.

Backups will be fastest when the backup location is on the SQL server. Try to
avoid network drives.

## Central Administration

Verify that the user account performing this procedure is a member of the
Farm Administrators group. Additionally, verify that the Windows SharePoint
Services Timer V4 service has Full Control permissions on the backup folder.

1. In Central Administration, go to __Backup and Restore__ > __Perform a site
collection backup__
2. Select the site collection to back up
3. Enter the path of the backup file into the __Filename__ textbox
4. If you want to reuse a file, select the __Overwrite existing file__ check box.
5. Click __Start Backup__

## PowerShell

* At the Windows PowerShell command prompt, type the following command:

```Backup-SPSite -Identity <SiteCollectionGUIDorURL> -Path <BackupFile> [-Force] [-NoSiteLock] [-UseSqlSnapshot] [-Verbose]```

* If you want to overwrite a previously used backup file, use the ```Force``` parameter.
* You can use the ```NoSiteLock``` parameter to keep the read-only lock from being set on the site collection while it is being backed up. However, using this parameter can enable users to change the site collection while it is being backed up and could lead to possible data corruption during backup.

## Programmatically

The following example shows a simple way to programmatically back up or restore a site collection.

    // Get a reference to the Web application publishing
    // Web service.
    SPFarm myFarm = SPFarm.Local;
    SPServiceCollection myServices = myFarm.Services;
    Guid serviceID = new Guid("21d91b29-5c5b-4893-9264-4e9c758618b4");
    SPWebService webPubService = (SPWebService)myServices[serviceID];

    // Get a reference to the Web application that hosts the
    // site collection.
    SPWebApplicationCollection myApps = webPubService.WebApplications;
    Guid appID = new Guid("10ea4e6f-ae37-4909-b04f-f516c066bc37");
    SPWebApplication myApp = myApps[appID];

    // As alternative to the preceding three lines, you can use
    // the following when you know the URL of the Web application:
    //     SPWebApplication myApp = SPWebApplication.Lookup(url_of_Web_app)

    // Get a reference to the Web application's collection of
    // site collections.
    SPSiteCollection mySiteCols = myApp.Sites;

    // Back up a specified site collection.
    mySiteCols.Backup(@"http://Server/sites/MySiteCollection", @"\\OtherServer\WSSBackups\SiteCollections\BackupOfMySiteCollection", true);

    // Restoring the site collection is identical to the preceding
    // code except that the "Restore" is used in place of "Backup".
    //
    // mySiteCols.Restore(@"http://Server/sites/MySiteCollection", @"\\OtherServer\WSSBackups\SiteCollections\BackupOfMySiteCollection", true);

https://msdn.microsoft.com/library/office/cc264319(v=office.14).aspx
