# Content Type Hub

A content type hub allows you to create content types centrally and publishing
them to the subscribed sites.

## Create the Content Type Hub

1. Create a site collection for the content type hub. Template does not matter.
2. In __Site Collection Features__, activate the __Content Type Syndication Hub__ feature.

## Associating The Content Type Hub

The content type hub needs to be associated with a __Managed Metadata Service__.

### Using Central Admin

1. Navigate to __Service Applications__.
2. Select the __Managed Metadata Service__ and click __Properties__ in the ribbon.
3. Enter the __Content Type Hub__ URL near the bottom of the dialog and click __OK__.
4. Select the __Managed Metadata Service Connection__, and click __Properties__ in the ribbon.
5. Check the following items
  * Consumes content types from the Content Type Gallery at ```site collection url```.
  * Push-down Content Type Publishing updates from the Content Type Gallery to sub-sites and lists using the content type.
6. Click __OK__.

### Using PowerShell

    Add-PSSnapin "Microsoft.SharePoint.PowerShell"

    $mms = Get-SPServiceApplication -Name "Managed Metadata Service"  
    Set-SPMetadataServiceApplication -Identity $mms -HubUri "http://sathishserver:3000/sites/SathishContentTypeHubSite/"

    $mmsp = Get-SPServiceApplicationProxy | ? {_.DisplayName -eq "Managed Metadata Service"}
    Set-SPMetadataServiceApplicationProxy -Identity $mmsp -ContentTypeSyndicationEnabled -ContentTypePushdownEnabled -Confirm:$false

## Create and Publish Content Types

1. Create a custom content type
2. In settings of the content type, click __Manage publishing for this content type__.
3. Choose the __Publish__ radio button option and click __OK__.

## Using Published Content Types

If you don't see the published content types on your site, it may be because
they haven't been pushed down to the site yet.

There are two timer jobs that handle this:
1. Content Type Hub
2. Content Type Subscriber (site-specific)

Run these two jobs, in order, about a minute apart.

Once that the jobs have successfully completed, the newly published content types
should appear in your site collection for use.
