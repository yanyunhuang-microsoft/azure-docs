---
title: Manage consent to applications and evaluating consent requests
description: Learn how to manage consent requests when user consent is disabled or restricted, and how to evaluate a request for tenant-wide admin consent to an application in Azure Active Directory.
titleSuffix: Azure AD
services: active-directory
author: davidmu1
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: how-to
ms.date: 08/25/2021
ms.author: davidmu
ms.reviewer: phsignor
---

# Manage consent to applications and evaluate consent requests in Azure Active Directory

Microsoft recommends [restricting user consent](../../active-directory/manage-apps/configure-user-consent.md) to allow users to consent only for app from verified publishers, and only for permissions you select. For apps which do not meet this policy, the decision-making process will be centralized with your organization's security and identity administrator team.

After end-user consent is disabled or restricted, there are several important considerations to ensure your organization stays secure while still allowing business critical applications to be used. These steps are crucial to minimize impact on your organization's support team and IT administrators, while preventing the use of unmanaged accounts in third-party applications.

## Process changes and education

 1. Consider enabling the [admin consent workflow](configure-admin-consent-workflow.md) to allow users to request administrator approval directly from the consent screen.

 2. Ensure all administrators understand the [permissions and consent framework](../develop/consent-framework.md), how the [consent prompt](../develop/application-consent-experience.md) works, and how to [evaluate a request for tenant-wide admin consent](#evaluating-a-request-for-tenant-wide-admin-consent).
 3. Review your organization's existing processes for users to request administrator approval for an application, and make updates if necessary. If processes are changed:
    * Update the relevant documentation, monitoring, automation, and so on.
    * Communicate process changes to all affected users, developers, support teams, and IT administrators.

## Auditing and monitoring

1. [Audit apps and granted permissions](../../security/fundamentals/steps-secure-identity.md#audit-apps-and-consented-permissions) in your organization to ensure no unwarranted or suspicious applications have previously been granted access to data.

2. Review [Detect and Remediate Illicit Consent Grants in Office 365](/microsoft-365/security/office-365-security/detect-and-remediate-illicit-consent-grants) for additional best practices and safeguards against suspicious applications requesting OAuth consent.

3. If your organization has the appropriate license:

    * Use additional [OAuth application auditing features in Microsoft Defender for Cloud Apps](/cloud-app-security/investigate-risky-oauth).
    * Use [Azure Monitor Workbooks to monitor permissions and consent](../reports-monitoring/howto-use-azure-monitor-workbooks.md) related activity. The *Consent Insights* workbook provides a view of apps by number of failed consent requests. This can be helpful to prioritize applications for administrators to review and decide whether to grant them admin consent.

### Additional considerations for reducing friction

To minimize impact on trusted, business-critical applications which are already in use, consider proactively granting administrator consent to applications that have a high number of user consent grants:

1. Take an inventory of the apps already added to your organization with high usage, based on sign-in logs or consent grant activity. A PowerShell [script](https://gist.github.com/psignoret/41793f8c6211d2df5051d77ca3728c09) can be used to quickly and easily discover applications with a large number of user consent grants.

2. Evaluate the top applications that have not yet been granted admin consent.

   > [!IMPORTANT]
   > Carefully evaluate an application before granting tenant-wide admin consent, even if many users in the organization have already consented for themselves.

3. For each application that is approved, grant tenant-wide admin consent using one of the methods documented below.

4. For each approved application, consider [restricting user access](configure-user-consent.md).

## Evaluating a request for tenant-wide admin consent

Granting tenant-wide admin consent is a sensitive operation.  Permissions will be granted on behalf of the entire organization, and can include permissions to attempt highly privileged operations. For example, role management,  full access to all mailboxes or all sites, and full user impersonation.

Before granting tenant-wide admin consent, you must ensure you trust the application and the application publisher, for the level of access you're granting. If you aren't confident you understand who controls the application and why the application is requesting the permissions, *do not grant consent*.

The following list provides some recommendations to consider when evaluating a request to grant admin consent.

* **Understand the [permissions and consent framework](../develop/consent-framework.md) in the Microsoft identity platform.**

* **Understand the difference between [delegated permissions and application permissions](../develop/v2-permissions-and-consent.md#permission-types).**

   Application permissions allow the application to access the data for the entire organization, without any user interaction. Delegated permissions allow the application to act on behalf of a user who at some point was signed into the application.

* **Understand the permissions being requested.**

   The permissions requested by the application are listed in the [consent prompt](../develop/application-consent-experience.md). Expanding the permission title will display the permission’s description. The description for application permissions will generally end in "without a signed-in user". The description for delegated permissions will generally end with "on behalf of the signed-in user." Permissions for the Microsoft Graph API are described in [Microsoft Graph Permissions Reference](/graph/permissions-reference) - refer to the documentation for other APIs to understand the permissions they expose.

   If you do not understand a permission being requested, *do not grant consent*.

* **Understand which application is requesting permissions and who published the application.**

   Be wary of malicious applications trying to look like other applications.

   If you doubt the legitimacy of an application or its publisher, *do not grant consent*. Instead, seek additional confirmation (for example, directly from the application publisher).

* **Ensure the permissions requested are aligned with the features you expect from the application.**

   For example, an application offering SharePoint site management may require delegated access to read all site collections, but wouldn't necessarily need full access to all mailboxes, or full impersonation privileges in the directory.

   If you suspect the application is requesting more permissions than it needs, *do not grant consent*. Contact the application publisher to obtain more details.

## Granting consent as an administrator

### Granting tenant-wide admin consent

See [Grant tenant-wide admin consent to an application](grant-admin-consent.md) for step-by-step instructions for granting tenant-wide admin consent from the Azure portal, using Azure AD PowerShell, or from the consent prompt itself.

### Granting consent on behalf of a specific user

Instead of granting consent for the entire organization, an administrator can also use the [Microsoft Graph API](/graph/use-the-api) to grant consent to delegated permissions on behalf of a single user. For a detailed example using Microsoft Graph PowerShell, see [Grant consent on behalf of a single user using PowerShell](#grant-consent-on-behalf-of-a-single-user-using-powershell).

## Limiting user access to applications

Users' access to applications can still be limited even when tenant-wide admin consent has been granted. For more information on how to require user assignment to an application, see [methods for assigning users and groups](./assign-user-or-group-access-portal.md).

For more a broader overview including how to handle additional complex scenarios, see [using Azure AD for application access management](what-is-access-management.md).

## Disable all future user consent operations to any application

Disabling user consent for your entire directory prevents end users from consenting to any application. Administrators can still consent on user’s behalf. To learn more about application consent, and why you may or may not want to consent, read [Understanding user and admin consent](../develop/howto-convert-app-to-be-multi-tenant.md).

To disable all future user consent operations in your entire directory, follow these steps:

1. Open the [**Azure portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**
2. Open the **Azure Active Directory Extension** by clicking **All services** at the top of the main left-hand navigation menu.
3. Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.
4. Select **Enterprise applications** and select **User settings** in the **Manage** section.
:::image type="content" source="media/manage-consent-requests/disable-user-consent-operations.png" alt-text="disabling user consent operations for all apps.":::
5. Disable all future user consent operations by setting the **Users can consent to apps accessing company data on their behalf** toggle to **No** and click the **Save** button.

## Grant consent on behalf of a single user using PowerShell

When a user grants consent on behalf of themselves, the following happens:

1. A service principal for the client application is created, if does not already exist. A service principal is the instance of an application or a service, in your Azure AD tenant. Access granted to the app or service is associated with this service principal object.
1. For each API to which the application requires access, a delegated permission grant is created for the permissions needed by the application to that API, for access on behalf of the user. A delegated permission grant authorizes an application to access an API on behalf of a user, when that user has signed in.
1. The user is assigned the client application. Assigning the application to the user ensures the application is listed in the [My Apps](my-apps-deployment-plan.md) portal for that user, allowing them to review and revoke the access granted on their behalf.

To manually perform the steps which are equivalent to granting consent to an application on behalf of one user, you will need the following details:

* The app ID for app for which you are granting consent (we'll call this the "client application").
* The API permissions required by the client application. You will need to know the app ID of the API and the permission IDs or claim values.
* The username or object ID for the user on behalf of who access will be granted.

In the following example, we will use [Microsoft Graph PowerShell](/graph/powershell/get-started) to perform the three steps listed above to grant consent on behalf of a single user. For this example, the client application will be [Microsoft Graph Explorer](https://aka.ms/ge), and we will be granting access to the Microsoft Graph API.

```powershell
# The app for which consent is being granted. In this example, we're granting access
# to Microsoft Graph Explorer, an application published by Microsoft.
$clientAppId = "de8bc8b5-d9f9-48b1-a8ad-b748da725064" # Microsoft Graph Explorer

# The API to which access will be granted. Microsoft Graph Explorer makes API 
# requests to the Microsoft Graph API, so we'll use that here.
$resourceAppId = "00000003-0000-0000-c000-000000000000" # Microsoft Graph API

# The permissions to grant. Here we're including "openid", "profile", "User.Read"
# and "offline_access" (for basic sign-in), as well as "User.ReadBasic.All" (for 
# reading other users' basic profile).
$permissions = @("openid", "profile", "offline_access", "User.Read", "User.ReadBasic.All")

# The user on behalf of who access will be granted. The app will be able to access 
# the API on behalf of this user.
$userUpnOrId = "user@example.com"

# Step 0. Connect to Microsoft Graph PowerShell. We need User.ReadBasic.All to get
#    users' IDs, Application.ReadWrite.All to list and create service principals, 
#    DelegatedPermissionGrant.ReadWrite.All to create delegated permission grants, 
#    and AppRoleAssignment.ReadWrite.All to assign an app role.
#    WARNING: These are high-privilege permissions!
Connect-MgGraph -Scopes ("User.ReadBasic.All Application.ReadWrite.All " `
                        + "DelegatedPermissionGrant.ReadWrite.All " `
                        + "AppRoleAssignment.ReadWrite.All")

# Step 1. Check if a service principal exists for the client application. 
#     If one does not exist, create it.
$clientSp = Get-MgServicePrincipal -Filter "appId eq '$($clientAppId)'"
if (-not $clientSp) {
   $clientSp = New-MgServicePrincipal -AppId $clientAppId
}

# Step 2. Create a delegated permission grant granting the client app access to the
#     API, on behalf of the user. (This example assumes that an existing delegated 
#     permission grant does not already exist, in which case it would be necessary 
#     to update the existing grant, rather than create a new one.)
$user = Get-MgUser -UserId $userUpnOrId
$resourceSp = Get-MgServicePrincipal -Filter "appId eq '$($resourceAppId)'"
$scopeToGrant = $permissions -join " "
$grant = New-MgOauth2PermissionGrant -ResourceId $resourceSp.Id `
                                     -Scope $scopeToGrant `
                                     -ClientId $clientSp.Id `
                                     -ConsentType "Principal" `
                                     -PrincipalId $user.Id

# Step 3. Assign the app to the user. This ensure the user can sign in if assignment
#     is required, and ensures the app shows up under the user's My Apps.
if ($clientSp.AppRoles | ? { $_.AllowedMemberTypes -contains "User" }) {
    Write-Warning ("A default app role assignment cannot be created because the " `
                 + "client application exposes user-assignable app roles. You must " `
                 + "assign the user a specific app role for the app to be listed " `
                 + "in the user's My Apps access panel.")
} else {
    # The app role ID 00000000-0000-0000-0000-000000000000 is the default app role
    # indicating that the app is assigned to the user, but not for any specific 
    # app role.
    $assignment = New-MgServicePrincipalAppRoleAssignedTo `
          -ServicePrincipalId $clientSp.Id `
          -ResourceId $clientSp.Id `
          -PrincipalId $user.Id `
          -AppRoleId "00000000-0000-0000-0000-000000000000"
}
```

## Next steps

* [Five steps to securing your identity infrastructure](../../security/fundamentals/steps-secure-identity.md#before-you-begin-protect-privileged-accounts-with-mfa)
* [Configure the admin consent workflow](configure-admin-consent-workflow.md)
* [Configure how end-users consent to applications](configure-user-consent.md)
* [Permissions and consent in the Microsoft identity platform](../develop/v2-permissions-and-consent.md)
