# Redmine Omniauth AzureAD

This is a fork of https://github.com/Gucin/redmine_omniauth_azure.

## Installation

* Ensure you're running these commands as the `user` that runs `redmine` so the permissions are correct
* `cd /path/to/redmine/plugins`
* `git clone git@github.com:ibboost/redmine_omniauth_azure.git`
* `cd /path/to/redmine`
* `bundle install`
* `bundle exec rake redmine:plugins:migrate RAILS_ENV=production`
* Restart Redmine

## Registration

We will need an Azure Application registration to enable this plugins.

* Login to the Azure Portal > Azure Active Directory > App registrations > New registration
  ```
  Name: redmine-app
  Supported account types: Single tenant
  Redirect URI: https://${your_redmine_url}/oauth2callback_azure
  ```
* `Register`
* In the newly created `redmine-app` application go to `Certificates & secrets`
* On the `Client secrets` tab click `New client secret`
  ```
  Description: redmine-app-secret
  Expires: 730 days(24 months)
  ```
* Record the `Value` of the generated secret as this is the only time it will show. This is the `Client Secret`
* Go to `API permissions`
* `Add a permission`
  ```
  Microsoft Graph - Application permissions - Group.Read.All
  ```
* Click on `Grant admin consent for Default Directory` and confirm
* Go to `Authentication` and confirm the `Web - Redirect URIs` has an entry 
  ```
  https://${your_redmine_url}/oauth2callback_azure
  ```
* Retrieve these three bits of information to be used to authentication the plugin from Redmine
  * Tenant ID: `Directory (tenant) ID` from the `Overview` tab on the application
  * Client ID: `Application (client) ID` from the `Overview` tab on the application
  * Client Secret: `Client Secret` value generated above

## Configuration

Login as a Redmine Adminstrator

* Select the `Administation` tab from the top menu.
* `Plugins` tab
* `Configure` on `Redmine Omniauth Azure plugin`
* Enter the details from above
  * Client ID
  * Client Secret
  * Tenant ID
* Optionally add in allowed domains, one per line
* Check `Enable OAuth Authentication` and `Apply` to enable AzureAD logins
* Optionally allow `Autologin` with  `Administration > Settings > Authentication > Autologin`

## Login Process
When a user goes to the sign in page and click the `Login with Azure` the plugin will redirect them to the Azure login page where they will need to sign in. After a successful sign in they will be redirected back to Redmine.

An Administrator will need to create a user account for new users if auto registration isn't used.