# APEXUI-in-Oracle-Fusion-Apps
Let's say you have developed an APEX apps that needs to be integrated in the Oracle Fusion Apps. This technical paper provides the various options to achieve it. Integrating an Oracle APEX-developed application as a tile in Oracle Fusion Applications (e.g., Fusion HCM, ERP, SCM, etc.) is a common requirement when extending Oracle Cloud Applications with custom functionality. This integration is typically done using Oracle Visual Builder Extensibility (VBX) or App Composer, depending on the Fusion App and type of extension. A good example would be you have developed a custom approval dashboard in APEX for procurement users. You can add a tile in Fusion Procurement’s homepage labeled "Custom Approvals", linking directly to your APEX app. With SSO, users won’t need to log in again.

Here’s a step-by-step guide for integrating an APEX app as a tile (also called a Springboard icon) in Oracle Fusion Apps:
1. Host Your APEX App Securely

Your APEX app should be:

Hosted on Oracle Cloud (ATP/ORDS) or another secure, accessible HTTPS endpoint.

Configured with Single Sign-On (SSO) using Oracle Identity Cloud Service (IDCS) if you want seamless auth with Fusion (optional but recommended).

2. Create a Web Link Tile in Fusion Apps
Oracle Fusion allows you to add custom web link tiles to the Springboard (home page) using Structure tools.

A. Use the “Structure” Tool:
Log in to Fusion Apps as an admin or user with Page Customization privileges.
Click your username → Tools → Page Composer (or use “Structure” or “Appearance” in older versions).
Navigate to the Springboard (Home page).
Enter Edit mode (Customize or Personalize depending on scope).
Click Add → Add Tile (Web Link).

B. Configure the Tile:
Title: Name of your APEX app.
URL: Public HTTPS link to your APEX app (e.g., https://yourdomain/r/apex/f?p=100:1).
Icon: Choose an icon or upload a custom one.
Target: Choose whether to open in a new tab or embedded (usually new tab is safer).
Optionally, configure visibility rules (based on roles, conditions, etc.).

3. (Optional) Setup SSO Between Fusion and APEX
To provide a seamless user experience, configure SSO using Oracle Identity Cloud Service (IDCS):

A. Register APEX as a Client in IDCS:
Go to IDCS Admin Console.
Create a new Confidential App.
Configure Redirect URIs for APEX app.
Use OAuth2 or SAML depending on your ORDS/APEX setup.

B. Update ORDS to Accept IDCS Tokens:
Configure ORDS to validate OAuth tokens from IDCS.
If you do not use SSO, users will be prompted to log in separately to APEX.

4. (Optional) Embed APEX Inside Fusion via IFrame or VBX Component

You can also embed the APEX app within a Fusion page using VBX (Visual Builder Extensibility):

Steps:

Create a VBX Extension Project for Fusion Apps.
Use the Web View / IFrame component to embed the APEX app.
Deploy the extension back to Fusion and expose it via a tile or menu.
This provides a more native Fusion experience, especially if your app interacts with Fusion data.

5. Test the Integration
Log in as a user with access to the tile.
Click the tile → Ensure the APEX app loads.
Confirm authentication flow (SSO or login prompt).
Validate responsive layout and UI fit.

6. Important Notes
For Security always use HTTPS and SSO where possible.
For Good Performance, keep APEX pages lightweight if embedded.
For Access Control, it is a good practice to control tile visibility using EL expressions or roles.
For Maintenance, 	Keep ORDS and APEX versions updated for compatibility.

7. Furthermore, to enable Single Sign-On (SSO) between Oracle Fusion and your APEX app (hosted via ORDS), you’ll use Oracle Identity Cloud Service (IDCS) as the identity provider.

Let’s walk through the steps:
A. Register the APEX App in IDCS

Log in to the Oracle Identity Cloud Service Console.

Go to Applications → Add Application
Select Confidential Application (for OAuth2 with token-based access).

Save and note: Client ID and Client Secret

B. Configure ORDS to Use IDCS for OAuth2

In your ORDS configuration (ords/standalone or via Tomcat):

Add your IDCS endpoints in the ords configuration: Refer securityjson1 (checked in file)

Restart ORDS

C. Update APEX Authentication Scheme

In your APEX app:

Go to Shared Components → Authentication Schemes.

Create a new Social Sign-In authentication.

Use:

Authorization URL → From IDCS

Token URL → From IDCS

Client ID / Secret → From IDCS

Scope → openid

Save and test the login.

Final URL in Fusion Tile

If using OAuth redirect: https://yourapexserver.com/ords/f?p=100:1:0::::::
If user is already authenticated via SSO, they'll be redirected seamlessly.


To Add an iFrame to Your Extension Page

Inside your VBX extension page:
Drag and drop a "Web View" or HTML component.
Add the checkedin iframehtml code to embed your APEX app
Ensure that your APEX server allows embedding via iframe by setting the proper X-Frame-Options or Content-Security-Policy.


Handle Authentication (SSO or Login)

If your APEX app is SSO-enabled via IDCS, it will auto-authenticate when the user accesses the embedded iframe. If not, the iframe will prompt for login (not ideal).
Best practice is to set up SSO between APEX and Fusion via IDCS (see previous message), so the embedded app does not show login prompts.

Deploy the Extension

Click Deploy inside VBX.
Publish the extension to your Fusion sandbox.
Test it in the sandbox environment:
Navigate to the extended Fusion page. Ensure the embedded APEX app loads correctly.
Publish the Sandbox once verified.

Now your APEX application is seamlessly embedded inside Oracle Fusion, such as:
A new tab inside an employee's record.A new section in a procurement workspace. 
A completely new page accessible from the navigator menu. Users will see and use the APEX app within the Fusion UI, not as a separate browser window.

If you need further assistance , please comment and I will be more than happy to help.
