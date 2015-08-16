# Manage the Site Lifecycle

## Delayed Site Collection

A delayed site collection is simply creating a site without selecting a template
up front.

To create a delayed site collection, when selecting a template, choose
"Select template later" from the custom tab.

Once the site collection is created and you attempt to access it, you will be
prompted to select a template.

Note: Subsites cannot be delayed in this way.

## Site Collection Disposition

Site policies allow you to manage the disposition (retention) of sites. With
these policies, you can control:

* Site closure - How long after creation it will take for a site to be
automatically closed.
* Site deletion - How long after creation or closure it will take for a site
to be automatically (and permanently) deleted.
* Postponement - Allows the site owner to manually postpone site deletion for a
period of time.
* Email notification - Before a site is deleted, an email will be sent to site
owners alerting them of the pending site deletion. You can set how far in
advance this email should be sent as well as frequency of follow up
notifications.
* Read-only mode - Set a site to be read-only on closure.

### Closure

* A closed site does not stop users from using the site, but
denotes the site is not longer actively used.
* Sites can be manually closed and re-opened by a site owner.
* Site collections can only be closed or re-opened by a site collection
administrator.

### Deletion

* When a site is deleted all its content and sub-webs are also deleted.

### General

* If a site is about to be deleted or if the site collection is in read-only mode, an information bar will appear at the top of the site for all users.
* The Expiration Policy timer job is responsible for the closure and deletion
of sites.

### Management Using the Content Hub

The Content Type Hub can be used to publish site policies, allowing you to use
the site policies from all sites in your farm.

* Content type syndication must be set up in order to publish site policies.
* Next to a site policy, you can click "Manage publishing for this policy" to
publish, re-publish and un-publish site policies.

### Self-Service Site Policy

You can allow users to apply site policies during site creation with
self-service site creation.

* Self-service site creation must be set up.
* From central administration, you can then choose whether a site policy should
be __hidden__, __optional__, or __requried__.

### Customization

You can use the client-side object model to:

* Determine the status of a site
* Open or close a site
* Apply a site policy
* Change the subject and content of the site deletion alert e-mail by
inserting the tokens for the site url ```"<!--{SiteUrl}-->"```, site expiration
date ```"<!--{SiteDeleteDate}-->"```, and (if you have an associated site
mailbox) the site mailbox ```"<!--{TeamMailboxID}-->"``` into the body of the
custom e-mail.

http://blogs.technet.com/b/tothesharepoint/archive/2013/03/28/site-policy-in-sharepoint.aspx
