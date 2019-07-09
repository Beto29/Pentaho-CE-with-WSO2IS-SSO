# Pentaho-CE-with-WSO2IS-SSO
Integrating Pentaho Community Edition 8.2 version with WSO2 Identity Server 5.8.0

Copy the below list of files to Pentaho Community Version folder:

Copy the file logout.jsp to “\pentaho-server\tomcat\webapps\pentaho” folder

Copy the file pentaho-saml-sample-8.2.0.0-SNAPSHOT.kar to “\pentaho-server\pentaho-solutions\system\karaf\deploy” folder

Copy the file pentaho.saml.cfg to “\pentaho-server\pentaho-solutions\system\karaf\etc” folder

Copy the applicationContext-spring-security-saml.xml file to “\pentaho-server\pentaho-solutions\system” folder

Now edit the pentaho-spring-beans.xml file available in \pentaho-server\pentaho-solutions\system folder; with a text editor and write the following line

<import resource="applicationContext-spring-security-saml.xml" />

Edit the “security.properties” file available in the \pentaho-server\pentaho-solutions\system folder using a text editor and change the value of provider value to saml from jackrabbit and save it.   

Checks before starting Pentaho Community Edition BA Server:

Files that we have placed in Pentaho Community Edition BA Server and the files that have been edited are as follows:

applicationContext-spring-security-saml.xml
pentaho.saml.cfg
pentaho-saml-sample-8.2.0.0-SNAPSHOT.kar
logout.jsp


Edited files:
pentaho-spring-beans.xml 
Security.properties

Now go to ~/Downloads/pentaho-server/ folder, open a terminal window and run the command ./start-pentaho.sh

Once the server starts successfully and you type http://localhost:8080 on a Browser of your choice , the URL should lead you Login Page of SSO Circle. 

We will not be able to login to Pentaho Home page with the default pentaho user credentials.

If this is achieved , the SAML2 plugin for Pentaho Community Edition is working.

Stop the Pentaho BA Server.

SAML plugin has been activated


After Configuring your WSO2 Idnetity Server 5.8.0 Version

Open a browser of your choice and enter the following URL “https://localhost:9443/carbon/admin/login.jsp”

Enter the default user credentials mentioned below

Username: admin, Password: admin

After logging into WSO2 Carbon Server; On the left side under Service Providers,Select the “Add”button. It will take you to the following page. R



Select “Manual configuration” radio button, then under Service Provider Name enter a name of your choice and description if required and select the “Register” option.

Once the new Service Provider is registered, now select the “List” option and then select the edit option, it will lead you to the following page. 



Now Expand the Inbound Authentication Configuration and select the Configure option by expanding SAML2 Web SSO Configuration. 

After you select the Configure option, it should take you to the following page

Select the Metadata File Configuration radio button, it will ask you to Choose File, 


In Choose File, go to the path Pentaho_SAML_Configurables which has been created in earlier steps and select the pentaho-sp.xml file and then select the Upload button.


Note: Before uploading the file, the pentaho-sp.xml file will be having “http://localhost:8080”,always use the port number and hostname or ip number which has been defined in server.xml file located in “pentaho-server\tomcat\conf” and do the respective changes to the pentaho-sp.xml file using a text editor.

And also comment the entire “<md:KeyDescriptor use="encryption">” section in the pentaho-sp.xml file.


If the pentaho-sp.xml file has been successfully uploaded,refer the screenshot message


Select “OK”, On Observing the Application Certificate should be available.

Now select the edit option in SAML2 Web SSO Configuration by expanding the Inbound Authentication Configuration. 

After selecting the edit option, all the details will be filled in the page. Refer the screenshot below

Change the NameID format to “urn:osasis:names:tc:SAML:1.1:nameidformat-unspecified”

Change the SLO Response URL to “http://localhost:8080/pentaho/logout.jsp”
Change the SLO Request URL to “http://localhost:8080/pentaho/Logout”

Do the following checks based on the below screenshots

After doing all the Checks, select the “Update”option available at the bottom of the page.

Now select the “Resident”option under Identity Providers which is available on the left side of the page. Refer the screenshot

The page will now look as follows 


Now edit the SAML2 WEB SSO Configuration by expanding the Inbound Authentication Configuration and change the Identity Provider Entity ID: value to “wso2”.


And then Select the Update option available at bottom.

After updating, go to “List” option in Service Providers and Edit the Pentaho SSO (which we have created earlier)and edit the SAML2 Web SSO Configuration by expanding the Inbound Authentication Configuration. Select the Download IDP Metadata option and save the metadata.xml file in Pentaho_SAML_Configurables folder. Refer the below screenshot

Now edit the pentaho.saml.cfg file using a text editor, locate the “saml.sp.metadata.entityId” and give its value as “pentaho”

In the same file enable “saml.sp.metadata.filesystem”by removing # and give its value as /Pentaho_SAML_Configurables/pentaho-sp.xml and disable the line below by adding # . 


In the same file locate “saml.idp.url” and give its value as “wso2”

In the same file enable saml.idp.metadata.filesystem by removing # and give its value as /Pentaho_SAML_Configurables/metadata.xml and disable the below line by adding #. 


Save the file.

Now Start the Pentaho BA server, and enter “http://localhost:8080/” on a browser of your choice, if everything is configured with reference to WSO2, the Pentaho URL should redirect the page to the following URL and refer the screenshot


Give the Username and Password as “admin”,its the default credentials when you are using WSO2 Carbon Server. Once the credentials are authenticated and authorized , the page will be redirecting to “http://localhost:8080/pentaho/Home”.


As I gave administrator privileges and did role mapping to wso2 Admin user,all the Administrative privileges are available with “admin” user


Note: As WSO2 Identity Server is now the authentication provider, the user logging in by default will only get “Read” Privileges. The User Creation, Roles Creation,Role mapping ,Permissions or Privileges will be explained clearly in later part of documentation.

When the user wants to Logout of Pentaho, after Logging out the URL will be redirected to “http://localhost:8080/pentaho/logout.jsp” page. 

User Creation, Role Mapping and Permissions in WSO2

Permissions: By default Pentaho will be having the 7 following privileges or Permissions to end users

Administer Security
Read Content
Manage Data Sources
Create Content
Publish Content
Schedule Content
Execute

The above mentioned Permissions or Privileges should be defined. In WSO2 select “List”  option in Service Provider and edit the Entity Id “PentahoSSO”

Expand the Roles/Permission Configuration and Select the +Add Permission option and add the above mentioned 7 permissions and then select the Update option.
 Once it updates, the service provider will be having the mentioned permissions. 

The permissions we have created for Service Provider Entity ID PentahoSSO can be viewed in User and Roles section available in top left corner of the Carbon Server page. Select the “List” option in Users and Roles, select Roles, it will provide the list of roles available , there select “Permissions” option for Application/PentahoSSO and scroll down to bottom. 

Role Mapping:

 By default Pentaho has 4 Roles, their default permissions in Pentaho are defined as follows:

Super User or Administrator  role have all 7 Permissions defined in Permissions section of the document.

Business Analyst role has only Publish Content Permission.

Power User  role has Read Content,Create Content,Publish Content,Execute, Schedule Content permissions.

Report Author role has Schedule Content and Manage Data Sources permissions.


Now create or add 4 roles with the above mentioned names in WSO2 Identity Server Users and Roles section.

Select the +Add option, then Select Add New Role option, the select the Application option from Domain drop down, enter the name “BusinessAnalyst” and select next button. 
Note: WSO2 doesn’t allow Blank Spaces while entering role names.



After selecting next button, WSO2 will take you to Permissions Page, scroll down to the Application Section at the bottom and select the permission Publish Content and select the finish option.

Follow the procedure for the remaining 3 roles Administrator or Super User,Report Author and Power User and give respective permissions mentioned above in the document. After all roles are created, the roles section when you select List option will look like this

Highlighted in yellow are the roles that have been created.
Roles Admin,Internal/everyone,Internal/system are default Carbon Server roles


Create some test users by selecting +Add under Users and Roles section, and the select Add New User option and create users and map them to the roles that have been created. Refer the screenshots below


Select Finish option, similarly create another TestUser2 and map it to Application/Administrator role.
Refer the users screenshot below

Users and Roles have been created and users have been mapped to roles.


Now select “List” from Service Providers and Edit the PentaoSSO Service Provider Entity ID, Expand Roles and Permissions Configuration. Select the + Add Role Mapping option and map the created roles in WSO2 with that of Pentaho. Refer the screenshot below


Select Update Option. 
After Updating Expand the Claim Configuration. In that select the option “Define Custom Claim Dialect”
Select +Add Claim URI
Under Service Provider Claim
Enter Pentaho Roles
Under Local Claim, Select the URL http://wso2.org/claims/role
Check the Requested Claim and Mandatory Claim boxes

The select Update option available at the bottom.

Now with a text editor edit the pentaho-saml.cfg file, locate and change the value of saml.role.related.user.attribute.name = Pentaho Roles from Pentaho Role and save the file.

Role Mapping process is now complete.


Now start the Pentaho BA server,Once the server starts. Try logging with TestUser1 and TestUser2.

TestUser2 has Admin privileges and TestUser1 has Business Analyst privileges.

While logging in as TestUser1 for the first time, Username and Role check boxes will come, select both the check boxes and select Approve option. 


Note:Any created user in WSO2 Identity server when logging in for first time should select the Username and Role check boxes.

Note: Read Content permission is provided to all Authenticated users


As defined earlier in the document, Business Analyst role has only Publish Content permission, so when TestUser1 tries to schedule a Report, permission has to be denied. Refer the screenshot


Now logout as TestUser1 and try Logging in as TestUser2 who has admin privileges. Even for TestUser2 when he is logging in for first time he has to select the Username and Role check boxes


TestUser2 has complete admin privileges.

