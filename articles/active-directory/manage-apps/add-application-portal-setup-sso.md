---
title: 'Quickstart: Enable single sign-on for an enterprise application'
titleSuffix: Azure AD
description: Enable single sign-on for an enterprise application in Azure Active Directory.
services: active-directory
author: davidmu1
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: quickstart
ms.workload: identity
ms.date: 09/21/2021
ms.author: davidmu
ms.reviewer: ergleenl
# Customer intent: As an administrator of an Azure AD tenant, I want to enable single sign-on for an enterprise application.
---

# Quickstart: Enable single sign-on for an enterprise application in Azure Active Directory

In this quickstart, you use the Azure Active Directory Admin Center to enable single sign-on (SSO) for an enterprise application that you added to your Azure Active Directory (Azure AD) tenant. After you configure SSO, your users can sign in by using their Azure AD credentials. 

Azure AD has a gallery that contains thousands of pre-integrated applications that use SSO. This quickstart uses an enterprise application named **Azure AD SAML Toolkit** as an example, but the concepts apply for most pre-configured [enterprise applications in the gallery](../saas-apps/tutorial-list.md).

It is recommended that you use a non-production environment to test the steps in this quickstart.

## Prerequisites

To configure SSO, you need:

- An Azure account with an active subscription. [Create an account for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- One of the following roles: Global Administrator, Cloud Application Administrator, Application Administrator, or owner of the service principal.
- Completion of the steps in [Quickstart: Create and assign a user account](add-application-portal-assign-users.md).

## Enable single sign-on

To enable SSO for an application:

1. Go to the [Azure Active Directory Admin Center](https://aad.portal.azure.com) and sign in using one of the roles listed in the prerequisites.
1. In the left menu, select **Enterprise applications**. The **All applications** pane opens and displays a list of the applications in your Azure AD tenant. Search for and select the application that you want to use. For example, **Azure AD SAML Toolkit 1**.
1. In the **Manage** section of the left menu, select **Single sign-on** to open the **Single sign-on** pane for editing.
1. Select **SAML** to open the SSO configuration page. After the application is configured, users can sign in to it by using their credentials from the Azure AD tenant.
1. The process of configuring an application to use Azure AD for SAML-based SSO varies depending on the application. For any of the enterprise applications in the gallery, use the link to find information about the steps needed to configure the application. The steps for the **Azure AD SAML Toolkit** are listed in this quickstart.

    :::image type="content" source="media/add-application-portal-setup-sso/saml-configuration.png" alt-text="Configure single sign-on for an enterprise application.":::

1. In the **Set up Azure AD SAML Toolkit 1** section, record the values of the **Login URL**, **Azure AD Identifier**, and **Logout URL** properties to be used later.

## Configure single sign-on in the tenant

You add sign-in and reply URL values, and you download a certificate to begin the configuration of SSO in Azure AD.

To configure SSO in Azure AD:

1. In the Azure portal, select **Edit** in the **Basic SAML Configuration** section on the **Set up single sign-on** pane. 
1. For **Reply URL (Assertion Consumer Service URL)**, enter `https://samltoolkit.azurewebsites.net/SAML/Consume`.
1. For **Sign on URL**, enter `https://samltoolkit.azurewebsites.net/`.
1. Select **Save**.
1. In the **SAML Signing Certificate** section, select **Download** for **Certificate (Raw)** to download the SAML signing certificate and save it to be used later.

## Configure single sign-on in the application

Using single sign-on in the application requires you to register the user account with the application and to add the SAML configuration values that you previously recorded.

### Register the user account

To register a user account with the application:

1. Open a new browser window and browse to the sign-in URL for the application. For the **Azure AD SAML Toolkit** application, the address is `https://samltoolkit.azurewebsites.net`.
1. Select **Register** in the upper right corner of the page.

    :::image type="content" source="media/add-application-portal-setup-sso/toolkit-register.png" alt-text="Register a user account in the Azure AD SAML Toolkit application.":::

1. For **Email**, enter the email address of the user that will access the application. For example, in a previous quickstart, the user account was created that uses the address of `contosouser1@contoso.com`. Be sure to change `contoso.com` to the domain of your tenant.
1. Enter a **Password** and confirm it.
1. Select **Register**.

### Configure SAML settings

To configure SAML setting for the application:

1. Signed in with the credentials of the user account that you created, select **SAML Configuration** at the upper-left corner of the page.
1. Select **Create** in the middle of the page.
1. For **Login URL**, **Azure AD Identifier**, and **Logout URL**, enter the values that you recorded earlier.
1. Select **Choose file** to upload the certificate that you previously downloaded.
1. Select **Create**.
1. Copy the values of the **SP Initiated Login URL** and the **Assertion Consumer Service (ACS) URL** to be used later.

## Update single sign-on values

Use the values that you recorded for **SP Initiated Login URL** and **Assertion Consumer Service (ACS) URL** to update the single sign-on values in your tenant.

To update the single sign-on values:

1. In the Azure portal, select **Edit** in the **Basic SAML Configuration** section on the **Set up single sign-on** pane. 
1. For **Reply URL (Assertion Consumer Service URL)**, enter the **Assertion Consumer Service (ACS) URL** value that you previously recorded.
1. For **Sign on URL**, enter the **SP Initiated Login URL** value that you previously recorded.
1. Select **Save**.

## Test single sign-on

You can test the single sign-on configuration from the **Set up single sign-on** pane.

To test SSO:

1. In the **Test single sign-on with Azure AD SAML Toolkit 1** section, on the **Set up single sign-on** pane, select **Test**.
1. Sign in to the application using the Azure AD credentials of the user account that you assigned to the application.

## Clean up resources

If you are planning to complete the next quickstart, keep the enterprise application that you created. Otherwise, you can consider deleting it to clean up your tenant. For more information, see [Delete an application](delete-application-portal.md).

## Next steps

Learn how to configure the properties of an enterprise application.
> [!div class="nextstepaction"]
> [Configure an application](add-application-portal-configure.md)
