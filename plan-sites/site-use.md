# Identify Inactive Site Collections

There are two ways to identify inactive site collections, actively or passively.

## Last Modified Dates

To actively check if a site is inactive, you can check the following properties
of a site using PowerShell or CSOM:

* ```LastContentModifiedDate```
* ```LastSecurityModifiedDate```

## Site Use Confirmation and Deletion

You can also configure email notifications to be sent to site owners of inactive
sites. The following properties control when these are sent.

* Notification start date - After how many days should a site start to be
checked for inactivity.
* Check frequency - How often after the notification start date should emails
be sent.
* Automatic deletion - Allows you to specify if a site should be deleted if
confirmation is not received after a certain amount of notices.

### Confirmation

Site collection owners will receive the inactive site collection email
notifications.

If the site collection owner confirms that the site collection is active by clicking the confirmation link in the email notification, the certification date of the site collection is renewed.

https://technet.microsoft.com/en-us/library/cc262420.aspx
