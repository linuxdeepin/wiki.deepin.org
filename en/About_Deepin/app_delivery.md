---
title: Instructions for Using the Community Delivery System
description: Delivery APP to deepin APP Store
published: true
date: 2023-11-30T07:38:22.255Z
tags: delivery app, app delivery, delivery applications
editor: markdown
dateCreated: 2023-11-30T07:38:22.255Z
---

The deepin app store (hereinafter referred to as the app store) is the official application distribution platform for the deepin operating system. It provides users with services such as application recommendation, searching, installation, and management. As an open-source operating system community store, we are pleased to have you participate in the development of the deepin app store's application ecosystem. Whether you are an application developer or maintainer, you can submit applications through this delivery system. Before using this delivery system, please read the following instructions carefully to quick understand the usage and related specifications of this system.

## 1. Overview

To ensure that the applications available on the app store are stable, usable, and compliant with regional policies and regulations, each application will go through a review process by the review team. We appreciate your understanding. To help you smoothly pass the application review process, please consider the following common issues that may cause delays or rejections:

- Ensure the completeness and correctness of application information and metadata, and package the application in the .deb format.
- Ensure that the application runs without crashes or errors.
- Ensure that you are the genuine author, maintainer, or contributor of the application. Pirated and infringing applications are not allowed to protect the efforts of other developers.
- Ensure that your email information is valid and accurate, so that application managers can contact you when necessary.
- Ensure that the application is available during the review period. This document will be updated and improved in a timely manner. Each modification is based on the optimization of user experience and fair treatment of all developers or contributors. We hope to find secure, user-friendly, and high-quality applications that meet the needs of users. We will continuously strive for this goal.

## 2. How to Submit an Application

### 2.1 Become an application contributor

After registering as a deepin user through the deepin community, you can access the community application delivery system via the deepin official website.Or just visit the website [https://appdelivery.deepin.org.cn](https://appdelivery.deepin.org.cn)

### 2.2 Submit an Application

After entering the submission page, click "Upload Application" to submit your application. Use the language option in the upper right corner to control the display language of the uploaded application.
![2023-11-30_37116.png](/2023-11-30_37116.png)

## 3. Application Information

Complete application information is a necessary prerequisite for passing the review process quickly. Please carefully read and strictly comply with the following requirements:

- The application should be in the correct language (currently, primarily Chinese or English, controlled through the language option in the page header. More languages will be supported in the future).
- The application name should not exceed 60 characters and should not include hidden characters such as line breaks.
- The application category should match its actual functionality and purpose.
- The one-sentence introduction of the application should not exceed 100 characters and should not include hidden characters such as line breaks.
- The application should select the appropriate distribution region, ensuring compliance with relevant local laws and regulations.
- The application should select the correct system compatibility (currently only supporting X86 architecture deepin v20 and v23 systems, with future updates and adjustments based on deepin's mainline support).
- The application description should not exceed 1000 characters and should align with the application's functionality.
- If it is an update version, please provide the correct version update log within 1000 characters.
- The application icon should be a complete square (image format: JPG/PNG, resolution: 96x96px-512x512px, image size unlimited).
- Upload 3-6 screenshots of the application with different interface (image format: JPG/PNG, image size: not exceeding 2MB).
- If you are the author or maintainer of the application, please provide a valid name and contact email, along with the application's official website (or source code repository website) for review.
- The app store currently only support deb format application packages. Please ensure that the application package you upload is in the .deb format.
![2023-11-30_58013.png](/2023-11-30_58013.png)

## 4. Application Security

Your application must not contain unpleasant or offensive content, and must not damage users' devices or cause personal harm during usage. Please carefully understand and strictly follow the following requirements:

- The application must not contain viruses, trojans, or any other harmful features that infringe on user functionality (including suspicious behaviors like code execution).
- The application must not engage in malicious charging practices, including but not limited to: unauthorized charges without user confirmation (where users are required to confirm both the purchase and payment separately), hidden charges, or misleading payment prompts (such as embedding the payment agreement within the application's guide page) to deceive or manipulate users into making payments.
- The application must not acquire unnecessary permissions that could compromise user information security.
- The application must not access user devices or personal information beyond what is necessary without informing the user.
- The application must not forcefully initiate system services without user authorization.
- The application should handle collected user information properly and must not use, disclose, or allow third-party access to this information without proper authorization.

## 5. Application Functionality

### 5.1 Application Completeness

- 5.1.1 Ensure that the application submitted for review is the final version, with complete and accurate application information and metadata. Remove all temporary or test content before submission.
- 5.1.2 Before submitting for review, ensure that the application has passed functional and compatibility tests.
- 5.1.3 Please do not consider the application review process as a testing service. We will reject incomplete applications or applications with obvious technical problems or crashes.

### 5.2 Application Testability

- 5.2.1 The application must be testable. If, for any reason, the product cannot be tested, it may not meet the requirements for publication.
- 5.2.2 If your application requires an account login that cannot be publicly registered, please provide us with a valid demo account (indicate in the application description).
- 5.2.3 If your product requires access to a server, the server must be accessible to ensure the application functions properly.

### 5.3 Application Compliance

The application must not have any functional issues. Please adhere to the following requirements:

- The application must be installable without any issues or parsing failures. It must be uninstallable without any errors or uninstallation failures. The application must not crash during startup or runtime.
- The application content must be displayed correctly and accessible. Buttons within the application must respond when clicked and not display errors. If the application requires login, the registration/login account functionality must be available.
- The application icon and name should be relevant to the application's content. The application must not generate additional icons or loading methods unrelated to the main functionality, such as "Access XXX Website." If the application needs to add functionality to the right-click menu, only one entry or submenu is allowed.
- The application is not allowed to perform automatic updates (updates must prompt users with update information, and users can choose whether to update). Uninstallation of the application can only be done through the system's right-click uninstall function or the software center's uninstall function. Adding application icons and functions like "Uninstall XXX Software" in the menu after installation is not permitted.
- The application must not download or install standalone applications, additional code, or resources to add functionality or make significant changes that were not visible during the review process.
- The application must not forcefully or deceptively modify system default settings.
- The requested permissions of the application must align with its actual functionality. If there is no reasonable application scenario or necessity, personal information collection beyond the scope or frequency, such as contact lists, location, ID cards, or facial data, is not allowed. The application must not frequently request unrelated permissions that the user has explicitly denied. If the user refuses to grant permissions unrelated to the current service scenario during installation or runtime, the application must not exit or close. The application must not force start on boot or forcibly restart the device or modify system settings unrelated to the application's functionality.
- Applications must be packaged and submitted using the technologies provided by the application store. The use of third-party installers is not allowed. Applications must be updated using the application store's distribution mechanism and not through other update mechanisms.
- Applications are only allowed to associate with necessary file types that are already supported by the application. Malicious registration of minetype types is not permitted.
- If the application's functionality is restricted for specific users, such as limited geographical regions or internal use within an organization, please specify the specific limitations in the application description.

## 6. Application Content

Providing users with safe and secure content enhances their experience. Please thoroughly understand and strictly adhere to the following requirements:

### 6.1 General Content Requirements

- 6.1.1 The application must not be a simple package of a website page, template, content aggregator, or list of links.
  
- 6.1.2 The application must not primarily serve as a distribution platform for applications.
  

### 6.2 Harmful Risks

- 6.2.1 The application must not contain realistic depictions of humans or animals being killed, tortured, or subjected to cruelty, or content that encourages violence.
- 6.2.2 Children's applications must not contain external links, purchase opportunities, or any other content that may cause interference for children, unless it is kept within a designated area monitored by parents.
- 6.2.3 The application must not contain content that promotes illegal drug use, excessive alcohol consumption, drunk driving, or any other content that may harm others' health or violate the law.

### 6.3 Illegal Activities

- 6.3.1 The application must not involve illegal monetary transactions, gambling, anti-government or anti-social activities, or any other content prohibited by national or regional laws.
- 6.3.2 The application must not distribute explicit, pornographic, offensive, disturbing, disgusting, or vulgar content that disregards others' feelings.
- 6.3.3 The application must not contain content that discriminates against races or undermines ethnic unity.
- 6.3.4 The application must not contain defamatory or malicious content targeting religion, race, sexual orientation, gender, or other targeted groups.
- 6.3.5 The application must not contain content that encourages the illegal use of weapons, dangerous items, or promotes the purchasing of military weapons.

### 6.4 User-Generated Content

User-generated content refers to the content contributed by users for the application or product, which can be accessed or viewed by some or all users. If your product includes user-generated content, you must effectively manage it, including but not limited to:

Implementing filtering mechanisms to remove inappropriate content, establishing a reporting mechanism, and promptly responding to reports. Providing terms of service and/or content guidelines to users. Offering a method for users to report inappropriate content within the product.

If there are violations or illegal activities in the content within the application, we will reject,remove,or suspend the delivery account after confirmation.

### 6.5 Copyright and Maintenance Requirements

- 6.5.1 For **commercial applications**, cracked or pirated versions with clear copyright ownership are not allowed.
- 6.5.2 For **open-source applications**, they should not be identical or similar to other developers' applications in terms of appearance, name, theme, etc. Only one author or maintainer is allowed to upload the same version of the same application at the current time. If the current author or maintainer of an open-source application has not updated or maintained it for more than one month, and you want to update and maintain it, please send an email to [appstore@deepin.org](mailto:appstore@deepin.org) for explanation. After the operation team's review and confirmation, you can proceed with the upload of the new version. As an open-source community application store, we encourage you to contribute to and maintain open-source applications. If you have stopped maintaining the current application for more than one month due to personal reasons or other issues, please actively apply for delisting and explain the reasons, so that more open-source enthusiasts can join.

## 7. In-App Advertising

In-app advertising should only be displayed within the application. You need to understand and strictly adhere to the following requirements:

- The application must not push content unrelated to its own product. Advertisements should be suitable for the target audience and must not contain fraudulent content, promotions of other application stores, or malicious URLs.
- In-app advertising must not interfere with the user's normal operation experience. The main function of the application must not be solely advertising promotion. Advertisements must not be sent anonymously.
- In-app advertising must not force the installation of software or engage in bundled downloads. Users must not be forced, misled, or induced to click on advertisements. Advertisements must have a close function, and when the application is closed, in-app advertising should also be closed simultaneously.
- In-app advertising must not interfere with the functionality of third-party applications or devices.

## 8. User Privacy

You need to handle user personal data with caution and ensure compliance with the laws and regulations of the country or region where the app is published. You need to understand and strictly adhere to the following requirements:

- The application must not collect, transmit, or use user privacy data without proper user authorization or in violation of current legal provisions.
- If the application includes antivirus or security protection functionality, it should provide a privacy policy and explain the relevant user data collected or transmitted, as well as the purpose, recipients, and other related information of such user data in accordance with current laws.
- If the application transfers user personal information, it should inform users how the transferred information will be used and where it will be used.
- The collection, storage, and transfer of user identity, authentication, and other personal information by the application should be protected with encryption measures to ensure data confidentiality and integrity.
- The application must not use any sensitive privacy permissions or data unrelated to its functionality.
- The application must not tamper with user personal information within the app and must not access or tamper with user personal information in third-party applications.

## 9. Application Review Instructions

After successfully submitting your application through this system, it will undergo a review process. Please keep the following points in mind:

**Review Time:** Our review team will check your application as soon as possible. However, if your application is more complex or if there are new issues, it may require a more in-depth review and consideration.

**Review Progress:** The progress of the application review will be reflected in the application status on the delivery platform, so please pay attention to it.

**Review Rejection:** Our goal is to follow these guidelines fairly and consistently, but nobody is perfect. If your application is rejected, we will inform you of the reasons for rejection. If you still have questions or wish to provide additional information, you can supplement the information and resubmit or email us for clarification.

## 10. Application Maintenance

### 10.1 Application Query

The application list displays detailed information and operational controls for the applications uploaded under the current account, including application name, package name, version number, etc.
![2023-11-30_60174.png](/2023-11-30_60174.png)
You can use the search bar in the upper right corner of the list to search for and operate applications based on their names.

### 10.2 Review Status

The "Review Status" indicates the current status of the application in the store process. You can perform corresponding operations based on the application status. The specific statuses are shown in the table below.

|     |     |
| --- | --- |
| **Review Status** | **Description** |
| Draft | The application information has been completed but has not been submitted for review. |
| Under review | The application has been submitted and in the review process, you can check the progress through \[Review Status\]. |
| Rejected | The application has been submitted for review, but the submitted version did not pass, so the current version has not been published. You need to modify it based on the specific reasons. You can check the reason after "?Review Failed". |
| Synchronizing | The application has passed the review and is currently being published. It will be available after the process is completed. |
| Available | The application has been published on the selected compatible system' app store. |
| Removed | The application has been removed from the app store. |
| Older version removed | This is the previous version of the application before a new version is published. |
| Remove request under review | The application is currently undergoing review for removal. |
| Synchronizing failed | The application has passed all review processes but has not been successfully pushed to the app store. If you encounter this scenario, please contact the app store staff for assistance. |

### 10.3 Operations

The "Operations" column is shown in the figure below, and different controls will be displayed based on the application's status. You can perform corresponding operations based on the application's status.

![2023-11-30_36941.png](/2023-11-30_36941.png)

The specific operations are shown in the table below.

|     |     |
| --- | --- |
| **Operation** | **Description** |
| Older version | You can view all the submission history records of the current application and view detailed information. |
| Modify | Modify the current application information, and submit for review or save as a draft. |
| Update | Update the current application and submit for review or save as a draft. |
| Remove | Initiate a request to remove the current application. After approval by the operation staff, the application will be removed from the app store. |
| Undo | For applications in review status (under review, under removal review), you can revoke the application and cancel the review request. |
| Delete | For drafts, removed, and failed review applications, delete the most recent submission record. Deleting only removes the most recent submission, and does not delete all records of the application. |

## 11. TAG

If you have any suggestions or opinions regarding the deepin community app store, you can consult us via email [appstore@deepin.org](mailto:appstore@deepin.org) or post in the deepin forum. Our team will follow up on your questions as soon as possible.

If you represent your company and need to upload corporate applications to deepin and UOS app stores, please register as a corporate developer on the [UOS Open Platform](https://developer.chinauos.com/#/). Through the UOS Open Platform, you will receive more support for platform capabilities.

Once again, thank you for joining us as an app contributor. We look forward to seeing more excellent works developed or maintained by you!