# Getting Started with the STMicroelectronics STM32L4 Discovery Kit IoT Node<a name="getting_started_st"></a>

Before you begin, see [Prerequisites](freertos-prereqs.md)\.

If you do not already have the STMicroelectronics STM32L4 Discovery Kit IoT Node, visit the AWS Partner Device Catalog to purchase one from our [partner](https://devices.amazonaws.com/detail/a3G0L00000AANsWUAX/STM32L4-Discovery-Kit-IoT-Node)\.

Make sure you have installed the latest Wi\-Fi firmware\. To download the latest Wi\-Fi firmware, see [STM32L4 Discovery kit IoT node, low\-power wireless, BLE, NFC, SubGHz, Wi\-Fi](https://www.st.com/resource/en/utilities/inventek_fw_updater.zip)\. Under **Binary Resources**, choose **Inventek ISM 43362 Wi\-Fi module firmware update \(read the readme file for instructions\) **\.

## Setting Up Your Environment<a name="st-setup-env"></a>

### Install System Workbench for STM32<a name="install-system-workbench"></a>

1. Browse to [OpenSTM32\.org](http://www.openstm32.org/HomePage)\.

1. Register on the OpenSTM32 webpage\. You need to sign in to download System Workbench\.

1. Browse to the [System Workbench for STM32 installer](http://www.openstm32.org/System%2BWorkbench%2Bfor%2BSTM32) to download and install System Workbench\.

If you experience issues during installation, see the FAQs on the [System Workbench website](http://www.openstm32.org/HomePage)\.

## Download and Configure Amazon FreeRTOS<a name="st-download-and-configure"></a>

After your environment is set up, you can download Amazon FreeRTOS\.

### Download Amazon FreeRTOS<a name="st-download"></a><a name="st-download-free-rtos"></a>

1. In the AWS IoT console, browse to the [Amazon FreeRTOS page](https://console.aws.amazon.com/freertos)\.

1. In the navigation pane, choose **Software**\.

1. Under **Amazon FreeRTOS Device Software**, choose **Configure download**\.

1. Choose **Download FreeRTOS Software**\.

1. Under **Software Configurations**, find **Connect to AWS IoT\- ST**, and then choose **Download**\.

1. Unzip the downloaded file to the AmazonFreeRTOS folder, and make a note of the folder's path\.

**Note**  
The maximum length of a file path on Microsoft Windows is 260 characters\. The longest path in the Amazon FreeRTOS download is 122 characters\. To accommodate the files in the Amazon FreeRTOS projects, make sure that the path to the `AmazonFreeRTOS` directory is fewer than 98 characters long\. For example, `C:\Users\Username\Dev\AmazonFreeRTOS` works, but `C:\Users\Username\Documents\Development\Projects\AmazonFreeRTOS` causes build failures\.  
In this tutorial, the path to the `AmazonFreeRTOS` directory is referred to as `BASE_FOLDER`\.

### Configure Your Project<a name="st-freertos-config-project"></a>

To run the demo, you must configure your project to work with AWS IoT, which requires that you register your board as an AWS IoT thing\. [Registering Your MCU Board with AWS IoT](freertos-prereqs.md#get-started-freertos-thing) is a step in the [Prerequisites](freertos-prereqs.md)\.

**To configure your AWS IoT endpoint**

1. Browse to the [AWS IoT console](https://console.aws.amazon.com/iotv2/)\.

1. In the navigation pane, choose **Settings**\.

   Your AWS IoT endpoint is displayed in **Endpoint**\. It should look like `<1234567890123>-ats.iot.<us-east-1>.amazonaws.com`\. Make a note of this endpoint\.

1. In the navigation pane, choose **Manage**, and then choose **Things**\.

   Your device should have an AWS IoT thing name\. Make a note of this name\.

1. Open `<BASE_FOLDER>\demos\common\include\aws_clientcredential.h` and specify values for the following `#define` constants:
   + `clientcredentialMQTT_BROKER_ENDPOINT` *Your AWS IoT endpoint*
   + `clientcredentialIOT_THING_NAME` *The AWS IoT thing name of your board*

**To configure your Wi\-Fi settings**

1. Open the `aws_clientcredential.h` file\.

1. Specify values for the following `#define` constants:
   + `clientcredentialWIFI_SSID` *The SSID for your Wi\-Fi network*
   + `clientcredentialWIFI_PASSWORD` *The password for your Wi\-Fi network*
   + `clientcredentialWIFI_SECURITY` *The security type of your Wi\-Fi network*

     Valid security types are:
     + `eWiFiSecurityOpen` \(Open, no security\)
     + `eWiFiSecurityWEP` \(WEP security\)
     + `eWiFiSecurityWPA` \(WPA security\)
     + `eWiFiSecurityWPA2` \(WPA2 security\)

**To configure your AWS IoT credentials**
**Note**  
To configure your AWS IoT credentials, you need the private key and certificate that you downloaded from the AWS IoT console when you registered your device\. After you have registered your device as an AWS IoT thing, you can retrieve device certificates from the AWS IoT console, but you cannot retrieve private keys\.

Amazon FreeRTOS is a C language project, and the certificate and private key must be specially formatted to be added to the project\. You must format the certificate and private key for your device\.

1. In a browser window, open `<BASE_FOLDER>\tools\certificate_configuration\CertificateConfigurator.html`\.

1. Under **Certificate PEM file**, choose the `<ID>-certificate.cert.pem` that you downloaded from the AWS IoT console\.

1. Under **Private Key PEM file**, choose the `<ID>-private.pem.key` that you downloaded from the AWS IoT console\.

1. Choose **Generate and save aws\_clientcredential\_keys\.h**, and then save the file in `<BASE_FOLDER>\demos\common\include`\. This overwrites the existing file in the directory\.
**Note**  
The certificate and private key are hard\-coded for demonstration purposes only\. Production\-level applications should store these files in a secure location\.

## Build and Run the Amazon FreeRTOS Demo Project<a name="st-build-and-run-example"></a>

### Import the Amazon FreeRTOS Demo into the STM32 System Workbench<a name="st-freertos-import-project"></a><a name="st-import-project"></a>

1. Open the STM32 System Workbench and enter a name for a new workspace\.

1. From the **File** menu, choose **Import**\. Expand **General**, choose **Existing Projects into Workspace**, and then choose **Next**\.

1. In **Select Root Directory**, enter `<BASE_FOLDER>\demos\st\stm32l475_discovery\ac6`\.

1. The project `aws_demos` should be selected by default\.

1. Choose **Finish** to import the project into STM32 System Workbench\.

1. From the **Project** menu, choose **Build All**\. Confirm the project compiles without any errors or warnings\.

### Run the Amazon FreeRTOS Demo Project<a name="st-run-example"></a>

1. Use a USB cable to connect your STMicroelectronics STM32L4 Discovery Kit IoT Node to your computer\. 

1. Rebuild your project\.

1. Sign in to the [AWS IoT console](https://console.aws.amazon.com/iotv2/)\.

1. In the navigation pane, choose **Test** to open the MQTT client\.

1. In **Subscription topic**, enter `freertos/demos/echo`, and then choose **Subscribe to topic**\.

1. From **Project Explorer**, right\-click `aws_demos`, choose **Debug As**, and then choose **Ac6 STM32 C/C\+\+ Application**\.

   If a debug error occurs the first time a debug session is launched, follow these steps:

   1. In STM32 System Workbench, from the **Run** menu, choose **Debug Configurations**\.

   1. Choose **aws\_demos Debug**\. \(You might need to expand **Ac6 STM32 Debugging**\.\)

   1. Choose the **Debugger** tab\.

   1. In **Configuration Script**, choose **Show Generator Options**\.

   1. In **Mode Setup**, set **Reset Mode** to **Software System Reset**\. Choose **Apply**, and then choose **Debug**\. 

1. When the debugger stops at the breakpoint in `main()`, from the **Run** menu, choose **Resume**\.

You should see MQTT messages sent by your device in the MQTT client in the AWS IoT console\.

### Run the Bluetooth Low\-Energy Demo<a name="st-run-ble"></a>


|  | 
| --- |
| Amazon FreeRTOS support for Bluetooth Low Energy is in public beta release\. BLE demos are subject to change\. | 

**Note**  
To run the BLE demo, you need the SPBTLE\-1S BLE module for the STM32L475 Discovery Kit\.

Amazon FreeRTOS supports [Bluetooth Low Energy \(BLE\)](https://docs.aws.amazon.com/freertos/latest/userguide/freertos-ble-library.html) connectivity\. You can download Amazon FreeRTOS with BLE from [GitHub](https://github.com/aws/amazon-freertos/tree/feature/ble-beta)\. The Amazon FreeRTOS BLE library is still in public beta, so you need to switch branches to access the code for your board\. Check out the branch named `feature/ble-beta`\.

To run the Amazon FreeRTOS demo project across BLE, you need to run the Amazon FreeRTOS BLE Mobile SDK Demo Application on an iOS or Android mobile device\.

**To set up the the Amazon FreeRTOS BLE Mobile SDK Demo Application**

1. Follow the instructions in [Mobile SDKs for Amazon FreeRTOS Bluetooth Devices](https://docs.aws.amazon.com/freertos/latest/userguide/freertos-ble-mobile.html) to download and install the SDK for your mobile platform on your host computer\.

1. Follow the instructions in [Amazon FreeRTOS BLE Mobile SDK Demo Application](https://docs.aws.amazon.com/freertos/latest/userguide/ble-demo.html#ble-sdk-app) to set up the demo mobile application on your mobile device\.

 For instructions about how to run the MQTT over BLE demo on your board, see the [MQTT over BLE demo application](https://docs.aws.amazon.com/freertos/latest/userguide/ble-demo.html#ble-demo-mqtt)\.

## Troubleshooting<a name="st-troubleshooting"></a>

If you see the following in the UART output from the demo application, you need to update the Wi\-Fi module’s firmware:

```
[Tmr Svc] WiFi firmware version is: xxxxxxxxxxxxx
[Tmr Svc] [WARN] WiFi firmware needs to be updated.
```

To download the latest Wi\-Fi firmware, see [STM32L4 Discovery kit IoT node, low\-power wireless, BLE, NFC, SubGHz, Wi\-Fi](https://www.st.com/resource/en/utilities/inventek_fw_updater.zip)\. In **Binary Resources**, choose the download link for **Inventek ISM 43362 Wi\-Fi module firmware update**\.