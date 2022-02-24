# Bulk enrollment of Apple devices | XenMobile Server Current Release
You can enroll large numbers of iOS, iPadOS, and macOS devices in XenMobile in two ways.

-   Use the Apple Deployment Program to enroll the iOS, iPadOS, and macOS devices that you buy directly from Apple, a participating Apple Authorized Reseller, or a carrier. That support includes Shared iPads. XenMobile supports the Apple Deployment Program for Apple Business Manager (ABM) and Apple School Manager (ASM) for Education. This article describes how to integrate multiple devices with your ABM account. For information on enrolling in ABM and connecting your ABM account with XenMobile, see [Deploy devices through Apple Deployment Program](https://docs.citrix.com/en-us/xenmobile/server/provision-devices/apple-deployment-program.html). For information about Apple School Manager accounts, see [Integrate with Apple Education features](https://docs.citrix.com/en-us/xenmobile/server/provision-devices/integrate-with-apple-education.html).

    For enrollment of macOS devices, XenMobile requires that the devices run macOS 10.10 or later.
-   You can also use Apple Configurator 2 to enroll iOS devices whether you purchased them directly from Apple or not.

With ABM:

-   You do not have to touch or prepare the devices. Instead, you submit device serial numbers or purchase order numbers through ABM to configure and enroll the devices.
-   After XenMobile enrolls the devices, you can give them to users who can start using them right away. When you set up devices with ABM, you can eliminate some of the Setup Assistant steps that users would have to complete when they first start their devices.
-   For more information on setting up ABM, see the documentation available from [Apple Business Manager](https://business.apple.com/).

With Apple Configurator 2:

-   You attach iOS devices to an Apple computer running macOS 10.7.2 or later and the Apple Configurator 2 app. You prepare the iOS devices and configure policies through Apple Configurator 2.
-   After you provision the devices with the required policies, the first time the devices connect to XenMobile, the devices receive policies from XenMobile. You can then start managing the devices.
-   For more information about using Apple Configurator 2, see the [Apple Configurator Help](https://help.apple.com/configurator/mac/1.7.2/#/cadf1802aed).

## Prerequisites

Open required ports for connectivity between XenMobile and Apple. For more information, see [Port requirements](https://docs.citrix.com/en-us/xenmobile/server/system-requirements/ports.html).

## Integrate your Apple Business Manager account with XenMobile

If you do not have an ABM account set up with XenMobile, complete the following steps in [Deploy devices through Apple Deployment Program](https://docs.citrix.com/en-us/xenmobile/server/provision-devices/apple-deployment-program.html).

-   Enroll in Apple Business Manager.
-   Connect your Apple Business Manager account with XenMobile.
-   Order Deployment Program enabled devices.
-   Manage Deployment Program enabled devices.

## Set a default server for bulk enrollment

To assign large orders of iOS, iPadOS, and macOS devices to an MDM server, you can set XenMobile as the default server.

1.  Sign in to [Apple Business Manager](https://business.apple.com/) using an administrator or device enrollment manager account.
2.  In the sidebar, click **Settings > Device Management Settings**.
3.  Choose an existing MDM server. Under **Default Device Assignment**, click **Change**. Select the default XenMobile server for each device type. Click **Done**.

## Configure deployment rules of device policies and apps for ABM accounts

You can associate ABM accounts with different device policies and apps by using the **Deployment Rules** section under **Configure > Device Policies** and **Configure > Apps**. You can specify that a policy or app either:

-   Deploys only for a particular ABM account.
-   Deploys for all ABM accounts except the one selected.

The list of ABM accounts includes only those accounts with a status of enabled or disabled. If the ABM account is disabled, the ABM device doesn’t belong to this account. Therefore, XenMobile doesn’t deploy the app or policy to the device.

In the following example, a device policy deploys only for devices with the ABM account name “ABM Account NR”.

![](https://docs.citrix.com/en-us/xenmobile/server/media/apple-dep-deployment-rule-policy-example.png)

## User experience when enrolling an Apple Deployment Program enabled device

When users enroll an Apple Deployment Program enabled device, their experience is as follows.

1.  Users start their Apple Deployment Program enabled device.
2.  XenMobile delivers the Apple Deployment Program configuration that you configured in the XenMobile console to the Apple Deployment Program enabled device.
3.  Users configure the initial settings on their device.
4.  The device automatically starts the XenMobile device enrollment process.
5.  Users continue to configure the other initial settings on their device.
6.  In the home screen, users might be prompted to sign in to Apple App Store so that they can download Citrix Secure Hub.

    > **Note:**
    >
    > This step is optional if you configure XenMobile to deploy the Secure Hub app using the device-based volume purchase app assignment. In this case, you don’t need to create an Apple App Store account or use an existing account.

    ![](https://docs.citrix.com/en-us/xenmobile/server/media/ios-dep-54.png)
7.  Users open Secure Hub and type their credentials. If required by the policy, users might be prompted to create and verify a Citrix PIN.

    XenMobile deploys any remaining required apps to the device.

## To configure Apple Configurator 2 settings

You can configure and deploy iPhone and iPad devices in bulk using Apple Configurator 2 instead of Apple Business Manager.

### Step 1: Configure settings in XenMobile

1.  In the XenMobile console, go to **Settings > Apple Configurator Device Enrollment**.

    ![](https://docs.citrix.com/en-us/xenmobile/server/media/settings-apple-dep-enrollment-8.png)
2.  Set **Enable Apple Configurator device enrollment** to **Yes**.
3.  The **Enrollment URL to enter in Apple Configurator** is a read-only field. This setting provides the URL for the XenMobile server that communicates with Apple. Copy and paste this URL when you configure settings in Apple Configurator 2. The enrollment URL is the XenMobile server fully qualified domain name (FQDN), such as `mdm.server.url.com`, or the IP address.
4.  To prevent unknown devices from enrolling, set **Require device registration before enrollment** to **Yes**. Note: If this setting is **Yes**, you must add the configured devices to **Manage > Devices** in XenMobile manually or through a CSV file before enrollment.
5.  To require users of iOS devices to enter their credentials when enrolling, set **Require credentials for device enrollment** to **Yes**. The default is not to require credentials for enrollment.
6.  Note: If the XenMobile server is using a trusted SSL certificate, skip this step. Click **Export anchor certs** and save the certchain.pem file to the macOS keychain (login or System).

    ![](https://docs.citrix.com/en-us/xenmobile/server/media/settings-apple-dep-enrollment-9.png)

### Step 2: Configure settings in Apple Configurator 2

1.  Install Apple Configurator 2 from the App Store.
2.  Use a Dock Connector-to-USB cable to connect devices to the Mac running Apple Configurator 2. You can configure up to 30 connected devices simultaneously. If you do not have a Dock Connector, use one or more powered USB 2.0 high-speed hubs to connect the devices.
3.  Start Apple Configurator 2. The configurator shows any devices that you can prepare for supervision.
4.  To prepare a device for supervision:

    -   Select **Supervise devices** if you intend to maintain control of the device by reapplying a configuration regularly. Click **Next**.

        > **Important:**
        >
        > Placing a device into Supervised mode installs the selected version of iOS on the device, completely wiping the device of any previously stored user data or apps.
    -   In iOS, click **Latest** for the latest version of iOS that you want to install.
5.  In **Enroll in MDM Server**, choose an MDM server. To add a new server, click **Next**
6.  In **Define an MDM server**, provide a name for the server and paste the MDM server URL from the XenMobile console.
7.  In **Assign to organization**, choose an organization to supervise the device.

    For more information on preparing devices with Apple Configurator 2, see the Apple Configurator help page, [Prepare devices](https://help.apple.com/configurator/mac/1.0/#cad96c7acd6).
8.  As each device is prepared, turn it on to start the iOS Setup Assistant, which prepares the device for first-time use.

## To assign devices from Apple Configurator 2 to Apple Business Manager

You can associate iPhone and iPad devices from Apple Configurator 2 with your Apple Business Manager account. When you add devices, they appear in the **Devices** section. These devices no longer include enrollment settings assigned through Apple Configurator 2. For more information, see [Assign devices added from Apple Configurator 2 to Apple Business Manager](https://support.apple.com/guide/apple-business-manager/assign-devices-added-from-apple-configurator-apd200a54d59/web).

## Renew or update certificates when using the Apple Deployment Program

When the XenMobile Secure Sockets Layer (SSL) certificate is renewed, you upload a new certificate in the XenMobile console in **Settings > Certificates**. In the **Import** dialog box, in **Use as**, click **SSL Listener** so that the certificate is used for SSL. After you restart the server, XenMobile uses the new SSL certificate. For more information about certificates in XenMobile, see [Uploading Certificates in XenMobile](https://docs.citrix.com/en-us/xenmobile/server/authentication.html).

It is not necessary to reestablish the trust relationship between Apple Deployment Program and XenMobile when you renew or update the SSL certificate. You can, however, reconfigure your **Apple Deployment Program** settings at any time by following the preceding steps in this article.

For more information about the Apple Deployment Program, see the [Apple documentation](https://support.apple.com/).

## Renew your connection between the Apple Deployment Program and XenMobile

XenMobile displays a License Expiration Warning when your Automated Device Enrollment server token expires.

![](https://docs.citrix.com/en-us/xenmobile/server/media/license-expiration-warning-dep.png)

Replace the token from Apple School Manager/Apple Business Manager.

### Step 1: Download a public key from your XenMobile server

1.  In the XenMobile console, go to **Settings > Apple Deployment Program** to download a new public key.

### Step 2: Create and download a server token file from your Apple account

1.  Sign in to Apple Business Manager to download the token.
2.  Open **Settings** and select the server from which you need a token. Click **Edit**.
3.  Under **MDM Server Settings**, upload the new public key you downloaded from XenMobile and save the changes.
4.  Click **Download Token** to download the new token.

### Step 3: Upload a server token file in XenMobile

1.  In Citrix XenMobile, go to **Settings > Apple Deployment Program**.
2.  Select the Deployment Program account, click **Edit**, and upload your server token file.
3.  Click **Next** and save the changes.
    This content has been machine translated dynamically.

Dieser Inhalt ist eine maschinelle Übersetzung, die dynamisch erstellt wurde. [(Haftungsausschluss)](#mt-disclaimer)

Cet article a été traduit automatiquement de manière dynamique. [(Clause de non responsabilité)](#mt-disclaimer)

Este artículo lo ha traducido una máquina de forma dinámica. [(Aviso legal)](#mt-disclaimer)

此内容已经过机器动态翻译。 [放弃](#mt-disclaimer)

このコンテンツは動的に機械翻訳されています。[免責事項](#mt-disclaimer)

이 콘텐츠는 동적으로 기계 번역되었습니다. [책임 부인](#mt-disclaimer)

Este texto foi traduzido automaticamente. [(Aviso legal)](#mt-disclaimer)

Questo contenuto è stato tradotto dinamicamente con traduzione automatica.[(Esclusione di responsabilità))](#mt-disclaimer)

This article has been machine translated.

Dieser Artikel wurde maschinell übersetzt. [(Haftungsausschluss)](#mt-disclaimer)

Ce article a été traduit automatiquement. [(Clause de non responsabilité)](#mt-disclaimer)

Este artículo ha sido traducido automáticamente. [(Aviso legal)](#mt-disclaimer)

この記事は機械翻訳されています.[免責事項](#mt-disclaimer)

이 기사는 기계 번역되었습니다.[책임 부인](#mt-disclaimer)

Este artigo foi traduzido automaticamente.[(Aviso legal)](#mt-disclaimer)

这篇文章已经过机器翻译.[放弃](#mt-disclaimer)

Questo articolo è stato tradotto automaticamente.[(Esclusione di responsabilità))](#mt-disclaimer)

[![](https://docs.citrix.com/assets/images/Switch-white.png)
  Switch to english Auf Englisch anzeigen Lire en anglais Leer en inglés 英語に切り替え 영어로 전환 Mudar para ingles 切换到英文 Passa all'inglese](javascript:) 

Translation failed!

# Bulk enrollment of Apple devices

April 21, 2021

Contributed by: 

C

You can enroll large numbers of iOS, iPadOS, and macOS devices in XenMobile in two ways.

-   Use the Apple Deployment Program to enroll the iOS, iPadOS, and macOS devices that you buy directly from Apple, a participating Apple Authorized Reseller, or a carrier. That support includes Shared iPads. XenMobile supports the Apple Deployment Program for Apple Business Manager (ABM) and Apple School Manager (ASM) for Education. This article describes how to integrate multiple devices with your ABM account. For information on enrolling in ABM and connecting your ABM account with XenMobile, see [Deploy devices through Apple Deployment Program](/en-us/xenmobile/server/provision-devices/apple-deployment-program.html). For information about Apple School Manager accounts, see [Integrate with Apple Education features](/en-us/xenmobile/server/provision-devices/integrate-with-apple-education.html).

    For enrollment of macOS devices, XenMobile requires that the devices run macOS 10.10 or later.
-   You can also use Apple Configurator 2 to enroll iOS devices whether you purchased them directly from Apple or not.

With ABM:

-   You do not have to touch or prepare the devices. Instead, you submit device serial numbers or purchase order numbers through ABM to configure and enroll the devices.
-   After XenMobile enrolls the devices, you can give them to users who can start using them right away. When you set up devices with ABM, you can eliminate some of the Setup Assistant steps that users would have to complete when they first start their devices.
-   For more information on setting up ABM, see the documentation available from [Apple Business Manager](https://business.apple.com/).

With Apple Configurator 2:

-   You attach iOS devices to an Apple computer running macOS 10.7.2 or later and the Apple Configurator 2 app. You prepare the iOS devices and configure policies through Apple Configurator 2.
-   After you provision the devices with the required policies, the first time the devices connect to XenMobile, the devices receive policies from XenMobile. You can then start managing the devices.
-   For more information about using Apple Configurator 2, see the [Apple Configurator Help](https://help.apple.com/configurator/mac/1.7.2/#/cadf1802aed).

## Prerequisites[](javascript:void(0))

Open required ports for connectivity between XenMobile and Apple. For more information, see [Port requirements](/en-us/xenmobile/server/system-requirements/ports.html).

## Integrate your Apple Business Manager account with XenMobile[](javascript:void(0))

If you do not have an ABM account set up with XenMobile, complete the following steps in [Deploy devices through Apple Deployment Program](/en-us/xenmobile/server/provision-devices/apple-deployment-program.html).

-   Enroll in Apple Business Manager.
-   Connect your Apple Business Manager account with XenMobile.
-   Order Deployment Program enabled devices.
-   Manage Deployment Program enabled devices.

## Set a default server for bulk enrollment[](javascript:void(0))

To assign large orders of iOS, iPadOS, and macOS devices to an MDM server, you can set XenMobile as the default server.

1.  Sign in to [Apple Business Manager](https://business.apple.com/) using an administrator or device enrollment manager account.
2.  In the sidebar, click **Settings > Device Management Settings**.
3.  Choose an existing MDM server. Under **Default Device Assignment**, click **Change**. Select the default XenMobile server for each device type. Click **Done**.

## Configure deployment rules of device policies and apps for ABM accounts[](javascript:void(0))

You can associate ABM accounts with different device policies and apps by using the **Deployment Rules** section under **Configure > Device Policies** and **Configure > Apps**. You can specify that a policy or app either:

-   Deploys only for a particular ABM account.
-   Deploys for all ABM accounts except the one selected.

The list of ABM accounts includes only those accounts with a status of enabled or disabled. If the ABM account is disabled, the ABM device doesn’t belong to this account. Therefore, XenMobile doesn’t deploy the app or policy to the device.

In the following example, a device policy deploys only for devices with the ABM account name “ABM Account NR”.

![](https://docs.citrix.com/en-us/xenmobile/server/media/apple-dep-deployment-rule-policy-example.png)

## User experience when enrolling an Apple Deployment Program enabled device[](javascript:void(0))

When users enroll an Apple Deployment Program enabled device, their experience is as follows.

1.  Users start their Apple Deployment Program enabled device.
2.  XenMobile delivers the Apple Deployment Program configuration that you configured in the XenMobile console to the Apple Deployment Program enabled device.
3.  Users configure the initial settings on their device.
4.  The device automatically starts the XenMobile device enrollment process.
5.  Users continue to configure the other initial settings on their device.
6.  In the home screen, users might be prompted to sign in to Apple App Store so that they can download Citrix Secure Hub.

    > **Note:**
    >
    > This step is optional if you configure XenMobile to deploy the Secure Hub app using the device-based volume purchase app assignment. In this case, you don’t need to create an Apple App Store account or use an existing account.

    ![](https://docs.citrix.com/en-us/xenmobile/server/media/ios-dep-54.png)
7.  Users open Secure Hub and type their credentials. If required by the policy, users might be prompted to create and verify a Citrix PIN.

    XenMobile deploys any remaining required apps to the device.

## To configure Apple Configurator 2 settings[](javascript:void(0))

You can configure and deploy iPhone and iPad devices in bulk using Apple Configurator 2 instead of Apple Business Manager.

### Step 1: Configure settings in XenMobile

1.  In the XenMobile console, go to **Settings > Apple Configurator Device Enrollment**.

    ![](https://docs.citrix.com/en-us/xenmobile/server/media/settings-apple-dep-enrollment-8.png)
2.  Set **Enable Apple Configurator device enrollment** to **Yes**.
3.  The **Enrollment URL to enter in Apple Configurator** is a read-only field. This setting provides the URL for the XenMobile server that communicates with Apple. Copy and paste this URL when you configure settings in Apple Configurator 2. The enrollment URL is the XenMobile server fully qualified domain name (FQDN), such as `mdm.server.url.com`, or the IP address.
4.  To prevent unknown devices from enrolling, set **Require device registration before enrollment** to **Yes**. Note: If this setting is **Yes**, you must add the configured devices to **Manage > Devices** in XenMobile manually or through a CSV file before enrollment.
5.  To require users of iOS devices to enter their credentials when enrolling, set **Require credentials for device enrollment** to **Yes**. The default is not to require credentials for enrollment.
6.  Note: If the XenMobile server is using a trusted SSL certificate, skip this step. Click **Export anchor certs** and save the certchain.pem file to the macOS keychain (login or System).

    ![](https://docs.citrix.com/en-us/xenmobile/server/media/settings-apple-dep-enrollment-9.png)

### Step 2: Configure settings in Apple Configurator 2

1.  Install Apple Configurator 2 from the App Store.
2.  Use a Dock Connector-to-USB cable to connect devices to the Mac running Apple Configurator 2. You can configure up to 30 connected devices simultaneously. If you do not have a Dock Connector, use one or more powered USB 2.0 high-speed hubs to connect the devices.
3.  Start Apple Configurator 2. The configurator shows any devices that you can prepare for supervision.
4.  To prepare a device for supervision:

    -   Select **Supervise devices** if you intend to maintain control of the device by reapplying a configuration regularly. Click **Next**.

        > **Important:**
        >
        > Placing a device into Supervised mode installs the selected version of iOS on the device, completely wiping the device of any previously stored user data or apps.
    -   In iOS, click **Latest** for the latest version of iOS that you want to install.
5.  In **Enroll in MDM Server**, choose an MDM server. To add a new server, click **Next**
6.  In **Define an MDM server**, provide a name for the server and paste the MDM server URL from the XenMobile console.
7.  In **Assign to organization**, choose an organization to supervise the device.

    For more information on preparing devices with Apple Configurator 2, see the Apple Configurator help page, [Prepare devices](https://help.apple.com/configurator/mac/1.0/#cad96c7acd6).
8.  As each device is prepared, turn it on to start the iOS Setup Assistant, which prepares the device for first-time use.

## To assign devices from Apple Configurator 2 to Apple Business Manager[](javascript:void(0))

You can associate iPhone and iPad devices from Apple Configurator 2 with your Apple Business Manager account. When you add devices, they appear in the **Devices** section. These devices no longer include enrollment settings assigned through Apple Configurator 2. For more information, see [Assign devices added from Apple Configurator 2 to Apple Business Manager](https://support.apple.com/guide/apple-business-manager/assign-devices-added-from-apple-configurator-apd200a54d59/web).

## Renew or update certificates when using the Apple Deployment Program[](javascript:void(0))

When the XenMobile Secure Sockets Layer (SSL) certificate is renewed, you upload a new certificate in the XenMobile console in **Settings > Certificates**. In the **Import** dialog box, in **Use as**, click **SSL Listener** so that the certificate is used for SSL. After you restart the server, XenMobile uses the new SSL certificate. For more information about certificates in XenMobile, see [Uploading Certificates in XenMobile](/en-us/xenmobile/server/authentication.html).

It is not necessary to reestablish the trust relationship between Apple Deployment Program and XenMobile when you renew or update the SSL certificate. You can, however, reconfigure your **Apple Deployment Program** settings at any time by following the preceding steps in this article.

For more information about the Apple Deployment Program, see the [Apple documentation](https://support.apple.com/).

## Renew your connection between the Apple Deployment Program and XenMobile[](javascript:void(0))

XenMobile displays a License Expiration Warning when your Automated Device Enrollment server token expires.

![](https://docs.citrix.com/en-us/xenmobile/server/media/license-expiration-warning-dep.png)

Replace the token from Apple School Manager/Apple Business Manager.

### Step 1: Download a public key from your XenMobile server

1.  In the XenMobile console, go to **Settings > Apple Deployment Program** to download a new public key.

### Step 2: Create and download a server token file from your Apple account

1.  Sign in to Apple Business Manager to download the token.
2.  Open **Settings** and select the server from which you need a token. Click **Edit**.
3.  Under **MDM Server Settings**, upload the new public key you downloaded from XenMobile and save the changes.
4.  Click **Download Token** to download the new token.

### Step 3: Upload a server token file in XenMobile

1.  In Citrix XenMobile, go to **Settings > Apple Deployment Program**.
2.  Select the Deployment Program account, click **Edit**, and upload your server token file.
3.  Click **Next** and save the changes.

The official version of this content is in English. Some of the Citrix documentation content is machine translated for your convenience only. Citrix has no control over machine-translated content, which may contain errors, inaccuracies or unsuitable language. No warranty of any kind, either expressed or implied, is made as to the accuracy, reliability, suitability, or correctness of any translations made from the English original into any other language, or that your Citrix product or service conforms to any machine translated content, and any warranty provided under the applicable end user license agreement or terms of service, or any other agreement with Citrix, that the product or service conforms with any documentation shall not apply to the extent that such documentation has been machine translated. Citrix will not be held responsible for any damage or issues that may arise from using machine-translated content.

DIESER DIENST KANN ÜBERSETZUNGEN ENTHALTEN, DIE VON GOOGLE BEREITGESTELLT WERDEN. GOOGLE LEHNT JEDE AUSDRÜCKLICHE ODER STILLSCHWEIGENDE GEWÄHRLEISTUNG IN BEZUG AUF DIE ÜBERSETZUNGEN AB, EINSCHLIESSLICH JEGLICHER GEWÄHRLEISTUNG DER GENAUIGKEIT, ZUVERLÄSSIGKEIT UND JEGLICHER STILLSCHWEIGENDEN GEWÄHRLEISTUNG DER MARKTGÄNGIGKEIT, DER EIGNUNG FÜR EINEN BESTIMMTEN ZWECK UND DER NICHTVERLETZUNG VON RECHTEN DRITTER.

CE SERVICE PEUT CONTENIR DES TRADUCTIONS FOURNIES PAR GOOGLE. GOOGLE EXCLUT TOUTE GARANTIE RELATIVE AUX TRADUCTIONS, EXPRESSE OU IMPLICITE, Y COMPRIS TOUTE GARANTIE D'EXACTITUDE, DE FIABILITÉ ET TOUTE GARANTIE IMPLICITE DE QUALITÉ MARCHANDE, D'ADÉQUATION À UN USAGE PARTICULIER ET D'ABSENCE DE CONTREFAÇON.

ESTE SERVICIO PUEDE CONTENER TRADUCCIONES CON TECNOLOGÍA DE GOOGLE. GOOGLE RENUNCIA A TODAS LAS GARANTÍAS RELACIONADAS CON LAS TRADUCCIONES, TANTO IMPLÍCITAS COMO EXPLÍCITAS, INCLUIDAS LAS GARANTÍAS DE EXACTITUD, FIABILIDAD Y OTRAS GARANTÍAS IMPLÍCITAS DE COMERCIABILIDAD, IDONEIDAD PARA UN FIN EN PARTICULAR Y AUSENCIA DE INFRACCIÓN DE DERECHOS.

本服务可能包含由 Google 提供技术支持的翻译。Google 对这些翻译内容不做任何明示或暗示的保证，包括对准确性、可靠性的任何保证以及对适销性、特定用途的适用性和非侵权性的任何暗示保证。

このサービスには、Google が提供する翻訳が含まれている可能性があります。Google は翻訳について、明示的か黙示的かを問わず、精度と信頼性に関するあらゆる保証、および商品性、特定目的への適合性、第三者の権利を侵害しないことに関するあらゆる黙示的保証を含め、一切保証しません。

ESTE SERVIÇO PODE CONTER TRADUÇÕES FORNECIDAS PELO GOOGLE. O GOOGLE SE EXIME DE TODAS AS GARANTIAS RELACIONADAS COM AS TRADUÇÕES, EXPRESSAS OU IMPLÍCITAS, INCLUINDO QUALQUER GARANTIA DE PRECISÃO, CONFIABILIDADE E QUALQUER GARANTIA IMPLÍCITA DE COMERCIALIZAÇÃO, ADEQUAÇÃO A UM PROPÓSITO ESPECÍFICO E NÃO INFRAÇÃO. 
 [https://docs.citrix.com/en-us/xenmobile/server/provision-devices/ios-bulk-enrollment.html](https://docs.citrix.com/en-us/xenmobile/server/provision-devices/ios-bulk-enrollment.html)
