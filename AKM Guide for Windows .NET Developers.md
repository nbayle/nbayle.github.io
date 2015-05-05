![image alt text](image_0.jpg)

![image alt text](image_1.jpg)

**Alliance Key Manager**

**Guide for Windows .NET Developers**

Software version: 3.0.3

Documentation version: 3.0.3.001

**Alliance Key Manager** 

**Guide for Windows .NET Developers** 

Copyright 2011, 2014 by Townsend Security, Inc.

All rights reserved.

Both this book and the software described by this book are protected by copyright. You may not copy or reproduce this book in any form without prior written permission from Townsend Security, Inc. The software associated with this product is governed by a license agreement. This software is yours to use only as long as you adhere to the terms of the license agreement.

US GOVERNMENT RESTRICTED RIGHTS. The SOFTWARE PRODUCT and documentation are provided with RESTRICTED RIGHTS. Use, duplication, or disclosure by the Government is subject to restrictions as set forth in subparagraph (c)(1)(ii) of the Rights in Technical Data and Computer Software clause at DFARS 252.227-7013 or subparagraphs (c)(1) and (2) of the Commercial Computer Software-Restricted Rights at 48 CFR 52.227-19, as applicable. Manufacturer is Townsend Security, Inc., 724 Columbia St. NW, Suite 400, Olympia, WA 98501 USA.

Alliance Key Manager is a registered trademark of Townsend Security, Inc.

IBM, IBM i, i5/OS, iSeries, eServer, System i and OS/400 are registered trademarks of IBM Corporation. Microsoft Windows, Microsoft Excel, Microsoft Word, the .NET Framework, and Microsoft SQL Server are registered trademarks of Microsoft Corporation. Java is a registered trademark of Oracle Corporation.

Townsend Security, Inc.
724 Columbia St. NW, Suite 400
Olympia, WA 98501 USA
Phone:	360.359.4400
Toll Free:	800.357.1019
Fax:	360.357.9047
Website: www.townsendsecurity.com
E-Mail: info@townsendsecurity.com

**Table of Contents** 

[TOC]

# Chapter 1: About This Manual

## Alliance Key Manager

Townsend Security’s Alliance Key Manager (AKM) provides a complete key management solution, including server setup and configuration, key lifecycle administration, secure key storage, key import/export, key access control, mirroring, and backup/restore. AKM supports compliance audit logging of all server, key access and configuration functions. AKM can be deployed as a privately managed Hardware Security Module (HSM), a dedicated Cloud HSM, a ready to use VMware virtual machine, or a ready to use AKM server in the cloud. Server management is accessed via a secure web browser interface and you can create and manage encryption keys using the AKM Administrative Console. The AKM solution supports the generation of certificates and private keys needed for authentication between client and server. A number of client-side applications, pre-compiled libraries, and code samples are available to help developers and key clients on a variety of platforms retrieve data encryption keys or perform remote encryption and decryption on the AKM server. All technical documentation and materials needed for deployment can be found on the AKM Supplemental. 

 

## AKM Client Library for Windows

The AKM Client Library for Windows includes a .NET assembly which provides an API for .NET applications to access the encryption keys on the AKM server for key retrieval or remote encryption. Key caching is included as a feature of key retrieval. .NET sample code for key retrieval and remote encryption is included to demonstrate both local encryption in your application and remote encryption on the AKM server. 

## SQL Server UDF

An additional SQL Server UDF assembly provides the same functionality for all editions of SQL Server. If you would like to use this assembly, see the AKM SQL Server UDF Guide in addition to this guide. The AKM SQL Server UDF Guide provides information on UDFs for key retrieval, key caching, and remote encryption and also describes using views and triggers to implement automatic encryption of a table.

> **_NOTE:_** If you have SQL Server Enterprise edition or higher, you can use AKM Key Connection for SQL Server, a ready to use client application which utilizes EKM and TDE (Transparent Data Encryption) to provide automatic key retrieval without any changes to your application. See the AKM Key Connection for SQL Server User Guide for more information. 

## Who is this for?

This guide is designed to help Windows .NET application developers and project managers implement key retrieval or remote encryption in their business applications using the AKM Client Library for Windows. It is also a starting point for users of SQL Server who wish to implement the SQL Server UDF or sample code, who will follow the steps for installation of the Client Library for Windows and installation of certificates and private keys to the Windows Certificate store, and then use the AKM SQL Server UDF Guide for information on the SQL Server UDF.

## Limitations

The AKM Client Library for Windows has the following limitations:

* The AKM Client Library cannot run on Windows XP because XP does not support secure ciphers. 

* To run the AKM Client Library on Windows Server 2003 you will need to apply several Microsoft hotfixes. For more information, please see the AKM Windows 2003 HotFix Readme.

* You must have the Microsoft .NET Framework 3.5 or later with version 1.1.14246.0 of the AKM Client Library. 

## Other resources

The following documents provide additional information on the installation and use of Alliance Key Manager:

* AKM User Guide

* AKM Server Management Guide

* AKM Administrative Console Guide

These and other resources are available on the AKM Supplemental.

## Notices

This product and documentation is covered by U.S. and International copyright law. This product may incorporate software licensed under one or more open source license agreements. Government users please note that this product is provided under restricted government use license controls. Please refer to the Alliance Key Manager End User License Agreement for more information.

## Change log

The following table provides information on the changes to this documentation:


**Version** | **Date** | **Description**
--- | --- | ---
 **0.01** | 1/6/2009 | Initial draft
**0.02** | 1/21/2009 | Add additional notes on Windows Certificates
**1.00** | 9/12/2011  | Add new notes about limitations and Hot Fix requirements.
 **1.01** | 11/5/2011 | Add step-by-step instructions and examples of using the Tester.exe sample key retrieval application. 
**3.0.0.001** | 3/19/2014 | Update for AKM 3.0.0. Name change from AKM QuickStart for Windows.
**3.0.0.002** | 4/23/2014 | Minor updates.
**3.0.0.003** | 10/10/2014 | Update for AKM Client Library version 1.1. Name change  from "AKM Guide for Windows Developers" to “AKM Guide for Windows .NET Developers”.
**3.0.3.001** | 12/16/2014 | Update for AKM 3.0.3 and the ready to use version of AKM for VMware. 



# Chapter 2: Introduction

This chapter introduces concepts related to using the AKM Client Library, including the use of certificates and private keys, configuration of the client application, encryption key names vs. key instances, key retrieval and caching, and remote encryption. It also summarizes AKM Client Library components and capabilities. 

## Certificates and private keys

The client and AKM server use certificates and private keys to establish a secure TLS connection and perform authentication. Certificates must be installed in the Windows certificate store. See subsequent chapters for more information on locating and installing the required certificates. 

## Client configuration

You will need to configure your client application to authenticate with the AKM server. Configuration includes certificate and private key information, server information, and other properties. You can set the configuration directly in the code of your application or in an application configuration file. See Chapter 6: Configure your Client Application for more information.

## AKM Key Service

The AKM Client Library for Windows includes functionality for key retrieval for local encryption in your application. Key caching is also included as a feature of the AKM Key Service. Key caching stores the encryption keys securely in memory for a configurable timespan. An RSA key in the Windows key store called the "key-caching key" encrypts the keys. See Chapter 8: Key Caching for more information. Sample code included with the AKM Client Library demonstrates key retrieval and key caching.

## AKM Encryption Service

The AKM Client library for Windows includes functionality for remote encryption and decryption on the AKM server with AES CBC or ECB encryption so the encryption keys never leave the server. Sample code included with the AKM Client Library demonstrates CBC encryption with a random IV (initialization vector). ECB mode is also supported, but is only recommended for backwards compatibility with applications previously using ECB mode encryption, or for applications needing deterministic encryption.

## Key names and key instances

Every encryption key created by AKM is given a key name and a unique instance (version) name. When the key is rolled, a new key is created with the same key name and a new instance name. This maintains the relationship of each instance to the original key, and means that you can roll the key without having to re-encrypt data that was protected with an older instance of the key. A rolled key is technically a new key, but since the key name does not change, it is considered a new instance of the same key. Every time the key is rolled and a new instance is created, this becomes the "current" instance of the key (sometimes known as the “default” instance). All older instances of the key are retained until you explicitly delete them.

When you send a key retrieval or encryption request, you must specify a key name and you can specify a key instance to request a particular instance of a key. If you do not specify a key instance, AKM will use the current instance of the key. You can store the instance name so that the correct instance of the key will be used on decryption.

## AKM Client Library for Windows

The AKM Client Library includes the following components:

* `TownsendSecurity.KeyClient.dll`: A .NET assembly for key retrieval and remote encryption.

* `TownsendSecurity.EncryptDecryptUdf.dll`: A SQL Server CLR assembly for key retrieval and remote encryption. 

* `Tester`: A tester application to test connectivity to the AKM server and key retrieval.

* `EncryptFile`: A sample application demonstrating local and remote encryption and decryption of a file.  

* Sample code:

    * `EncryptFile`: Sample code for the `EncryptFile` application demonstrating local and remote encryption of a file.

    * `EncryptString`: Sample code demonstrating local and remote encryption and decryption of a string. 

    * `SQL Server UDF`: Sample code for the SQL Server CLR assembly demonstrating local and remote encryption and decryption.

### `TownsendSecurity.KeyClient.dll`

The Key Client assembly is a .NET assembly which provides an API  for your .NET applications to access the keys stored on the AKM server. The `KeyService` object provides functionality for key retrieval with the option to securely cache the encryption keys. The `EncryptionService` object allows you to send data for remote encryption and decryption on the AKM server. The samples `EncryptFile` and `EncryptString` perform local and remote encryption and decryption of a file and a string, respectively.

### `TownsendSecurity.EncryptDecryptUdf.dll`

The SQL Server UDF is a companion assembly of the Key Client assembly, providing UDFs (user-defined functions) for key retrieval or remote encryption with SQL Server. Remote encryption uses CBC encryption with a random IV (initialization vector). The `Local.Encrypt` and `Local.Decrypt` methods provide key retrieval and key caching, and `Remote.Encrypt` and `Remote.Decrypt` methods provide remote encryption on the AKM server. These functions are interchangeable: you can encrypt locally, and decrypt the result remotely, for example. The SQL Server UDF samples demonstrate local and remote encryption and decryption. See the AKM SQL Server UDF Guide for more information on using this assembly.

### `Tester`

This is a GUI application that provides an easy way to test that your certificates are valid and correctly installed, your client configuration is working properly, you have network connectivity to the AKM server, that AKM is responding, you can retrieve keys and perform key caching. You can also use it to become familiar with the available .NET tracing.
		
### `EncryptFile`
 
The `EncryptFile` application is a console application performing local or remote encryption and decryption of a file using AES CBC encryption with random IV and with HMAC. Local encryption retrieves the encryption key and uses Microsoft AES crypto libraries to locally encrypt/decrypt the file. Remote encryption uses AES CBC encryption on the AKM server. Both local and remote encryption use the Microsoft CryptoStream class and show how to run the encryption through a stream. The HMAC is SHA-256 and uses a second symmetric key. For local encryption, the encryption key and the HMAC key are retrieved. For remote encryption, the HMAC key is retrieved and the encryption key remains on the AKM server. Source code for the `EncryptFile` application is available in the `Sample Code` directory.

## Sample code

Sample code is included with the installation in the `Sample Code` directory, and you may modify it to suit your needs. See the file `Disclaimer.txt` in the `Sample Code` directory for information on use of the sample code. 

### `EncryptFile`

Source code for the `EncryptFile` application.

### `EncryptString` 

`EncryptString` performs local and remote encryption and decryption of a string. Local encryption retrieves the encryption key and uses Microsoft’s ICryptoTransform interface to locally encrypt and decrypt the string, and demonstration of the use of key caching is included. Remote encryption uses AES CBC encryption on the AKM server. The string sample does not add a MAC. 

### SQL Server UDF 

Source code for the **TownsendSecurity.EncryptDecryptUdf.dll** assembly demonstrates key retrieval and local AES CBC encryption using Microsoft crypto libraries and remote encryption using AES CBC encryption on the AKM server. The output returned by the encrypt functions packs the key name, instance, IV, and ciphertext into a varbinary output string which is unpacked for decryption. See the AKM SQL Server UDF Guide for more information on the assembly and sample code.

# Chapter 3: Before You Begin

You will need to complete the following steps before continuing:

* Obtain a license

* Locate the required AKM certificates and private keys

* Install and set up the AKM server 

* Create encryption keys

* Install the Microsoft .NET Framework 3.5 or later

> **_NOTE:_** If you are deploying AKM for VMware, AKM for Windows Azure, or AKM for Amazon Web Services, licensing, server setup, and certificate/private key generation is completed automatically during initialization. You will also have the option to create an initial set of encryption keys for use in your client application.

See below for more information.

## Licensing 

You will need a temporary or permanent license to use or evaluate AKM. A temporary license will supply you with a fully functional AKM server that you can run in your environment for evaluation or testing. If you are deploying AKM for Windows Azure or AKM for VMware, a 30-day license is generated automatically during the initialization process. With AKM for Amazon Web Services, if you choose a BYOL model, a 30-day license in generated automatically during initialization; if you choose a fee-based model, AKM will generate a permanent license. If your temporary license expires, you will need to purchase a permanent license from Townsend Security or your software vendor.

## Certificates

The client and AKM server use certificate and private keys to establish a secure TLS connection and perform authentication. You will need to install the following certificates and private keys on the client in the Windows certificate store in order to authenticate your client application with the AKM server:

* AKM’s certificate authority (CA) certificate in `.pem` format. This is the root CA certificate.

* Client certificate/private key. These must be installed as one file in PKCS #12 format (`.p12` or `.pfx`).

* Intermediate CA certificate in `.pem` format (optional)

> **_NOTE:_** If you use AKM-generated certificates and private keys, you will not need to use an intermediate CA certificate.

<!-- -->

> **_NOTE:_** The standard approach to client/server authentication as described in this document is to install AKM’s CA certificate and a client certificate/private key on the client. See Appendix B: Certificate Pinning for a discussion of using AKM server certificates on the client for additional security.

<!-- -->

> **_SECURITY ALERT:_** Private key files must be protected during creation, distribution, and storage to prevent loss. The loss of these files will compromise the security of the AKM server. Depending on the file format, the private key files may be bundled with a certificate or they may be separate files. Transfer the private key files by sharing them over a secure network, placing them in a password-protected zip file, sending them using SFTP, or another secure method. Use the same level of care you would employ to protect encryption keys, including encryption. In the event the private keys are compromised or lost, you should immediately replace the certificate authority on the AKM server and all client certificates in that chain of trust. See the AKM Certificate Manager Guide for more information.

See below for information on locating these certificates.

### AKM for VMware, Windows Azure, and AWS

If you are deploying AKM for VMware, AKM for Windows Azure, or AKM for Amazon Web Services, all required AKM certificates and private keys are generated on startup and stored on the AKM server. See the specific guide for your deployment for information on downloading these certificates from the AKM server for use in your client application.

### HSM and Cloud HSM

If you are deploying AKM as a hardware security module (HSM) or Cloud HSM, you can use the AKM Certificate Manager application to create all of the required certificates and private keys. For information on installing the AKM Certificate Manager application and creating certificates, see the AKM Certificate Manager Guide on the AKM Supplemental. 

After creating certificates with AKM Certificate Manager, you will need the following files from the `AKMCerts\AKM_Certs\` folder in your Windows home directory:

* `certificate_authority\CASelfSignedCert.pem` or `CASelfSignedCert.der`  (AKM’s CA certificate)

* `client_<UserName>\p12\client_<UserName>_certBundle.p12` (the client certificate/private key)

If these files were created with associated passwords, you will need those passwords as well.

## Server setup

If you are deploying AKM for VMware, AKM for Windows Azure, or AKM for Amazon Web Services, server setup is completed for you. See the specific guide for your deployment for more information. If you are deploying the AKM HSM or Cloud HSM, you will need to install and set up the AKM server and any additional secondary (mirror, failover or high availability) servers before continuing. See the AKM Server Management Guide for more information.

You will need the following server information before continuing:

* The IP address or DNS name of the primary AKM server and any secondary AKM servers

* The port number that AKM has been configured to use for key retrieval (the default is 6000) and/or encryption (the default is 6003)

## Encryption keys 

If you are deploying AKM for VMware, AKM for Windows Azure, or AKM for Amazon Web Services, you will have the option to generate a set of encryption keys during initialization. See the specific guide for your deployment for more information on encryption keys available for use. You can set key access control for these keys using the AKM Administrative Console. For HSM and Cloud HSM deployments, you will need to create new encryption keys using the AKM Administrative Console. See the AKM Administrative Console Guide for more information.

## Microsoft .NET Framework

The AKM Client Library for Windows requires Microsoft .NET Framework 3.5 or later. To install the Microsoft .NET Framework, visit [http://www.microsoft.com](http://www.microsoft.com) and download an installer from Microsoft.

## Checklist

Before continuing, you will need the following items:

* AKM’s CA certificate in `.pem` format

* A client certificate/private key in in PKCS #12 format (`.p12` or `.pfx`) 

* The IP address or DNS name of the AKM server and the port number it will use for key retrieval (for both the primary and secondary AKM servers if using a failover server) or remote encryption

* The name of one or more encryption keys on the AKM server

* Microsoft .NET Framework 3.5 or later

## Next steps

Subsequent chapters will guide you through installation of the AKM Client Library for Windows, installation of certificates and private keys to the Windows certificate store, client configuration, samples for key retrieval and key caching, samples for remote encryption, and error reporting.

# Chapter 4: Install the AKM Client Library

The install file for the AKM Client Library is located in the following directory on the AKM Supplemental:

`AKM_Supplemental\Client_Platforms\Windows\Install`

Double-click `AKM_Client_Library_Installer_[version].msi` to start the installation process.

## AKM Client Library components

After the installation is complete, you will find a set of files at this path:


```
C:\Program Files\Townsend Security\AKM Client Library

```

The files in the `bin` directory are:

* `TownsendSecurity.KeyClient.dll` (Key Client .NET assembly) 

* `TownsendSecurity.EncryptDecryptUdf.dll` (SQL Server UDF assembly)

* `EncryptFile` (Console application for local or remote encryption of a file)

* `EncryptFile.exe.config` (An XML configuration file for `EncryptFile`)

* `Tester` (Tester application to test connectivity to the AKM server and key retrieval)

* `Tester.exe.config` (An XML configuration file for `Tester`) 

The files in the `Sample Code` directory are:

* `EncryptFile\EncryptFile` (Sample code for the  `EncryptFile` application)

* `EncryptString\EncryptString` (Sample code performing local and remote encryption/decryption of a string)

* `SQL Server UDF\`

    * `Local` (Sample code for the SQL Server UDF performing local encryption/decryption)

    * `Packing` (Sample code which packs the key name, instance, IV, and ciphertext into the resulting binary string, and that unpack them for decryption)

    * `Remote` (Sample code performing remote encryption/decryption on the AKM server)

# Chapter 5: Install Certificates

This chapter describes installing certificates into the Windows certificate store.  

## Overview

Certificates must be installed in the Windows certificate store for Current User or Local Computer. Install the certificates to the store for Current User if the client application will be run under the Current User. If your client application will run as another user, install the certificates in the Local Computer certificate store where they will be available to all users. You will then grant the user running the client application read permissions to the client private key.

> **_SECURITY ALERT:_** It is common for SQL Server or other applications to be run under a service account (e.g. Network Service) that may not be exclusively used by SQL Server. Your private key must be accessible to the user account that SQL Server uses, but if the account is shared with other applications, this exposes your private key to those applications. Malicious or accidental exposure may result, yielding access to encryption keys on your AKM server.

> Therefore, we strongly recommend your application run under a dedicated service account. This is generally a good security practice, and specifically protects access to your encryption keys. If you cannot change the user that your application runs under, then you must grant access to the private key to that user. Be aware, however, of the risk that this creates.

Install locations for certificates are listed below:

* Client certificate and private key - Personal store 

* AKM’s root CA certificate - Trusted Root Certification Authorities store 

* Intermediate CA certificate (optional) - Intermediate Certification Authorities store 

The configuration for the AKM Client Library uses the thumbprint attributes of the certificates. See below for a detailed guide to installing certificates. 

## Start Windows Certificate Manager

### Client application running under Current User

If installation, configuration, and the client application will all be run under the Current User, you can  start the Microsoft Management Console (MMC) with the Certificate Manager snap-in for the Current User:

Click **_Start_**, **_Run_**, and enter "certmgr.msc". The following panel is displayed:

![image alt text](image_2.png)

You can now install certificates to the "Current User" certificate stores. See the section Install the client certificate/private key below. 

### Client application running under another user

If your client application will run as another user, you will need to install the certificates in the "Local computer" store and grant the client application user private key access.

Click **_Start_**, **_Run_**, and enter "mmc". The following panel is displayed:

![image alt text](image_3.png)

Select **_File_**, **_Add/Remove Snap-in_**. The following panel is displayed:

![image alt text](image_4.png)

Select the **_Certificates_** snap-in and click **_Add_**. 

The following panel is displayed:

![image alt text](image_5.png)

> **_NOTE:_** SQL Server might be installed to run under a Windows Service account. However, if you are using SQL Server it is not necessary to select the Service account option shown, as you can follow the same steps to grant user permissions to the client private key as described elsewhere in this document.

Select **_Computer account_** and click **_Next_**. The following panel is displayed:

![image alt text](image_6.png)

Select **_Local computer_** store and click **_Finish_**. In the Add or Remove Snap-ins dialog, click **_OK_**.

You can now import certificates to the "Local computer" certificate stores. Certificates installed in the “Local Computer” store will be available to all users. However, whoever installs the private keys will need to grant explicit user access to private keys. See below for more information.

You can add the Certificates snap-in twice (once for "Current User" and once for “Local computer”) if you so choose.

## Install the client certificate/private key

Open Windows Certificate Manager. Right-click on the **_Personal_** store. Hover over **_All tasks_**. Select **_Import_**. The Certificate Import Wizard will open. Click **_Next_**. The following panel is displayed:

![image alt text](image_7.png)

Click **_Browse_** to select the `.p12` or `.pfx` client certificate/private key.  You will need to select the "Personal Information Exchange" file format in the file chooser. Click **_Next_**. The following panel is displayed:

![image alt text](image_8.png)

If there is a password associated with the file, enter it now. The password used when creating the PKCS#12 file is used only in this step; Unless you choose otherwise, once imported, the certificate and private  key are unlocked when you log into Windows. If you enter the wrong password, the wizard will not let you proceed.

> **_NOTE:_** If you want to require the user to enter the password whenever accessing the client private key, check the box to **_Enable strong private key protection_**. You will be prompted to enter a password every time the key is used by an application. Do not select this option if you are using the Key Client assembly from a non-interactive program such as a server.

Click **_Next_**. The following panel is displayed:

![image alt text](image_9.png)

Select **_Place all certificates in the following store_** and select "Personal". 

Click **_Next_**, then click **_Finish_**. 

## Grant user access to the client private key

If your application is run under a different user and you have installed certificates and private keys to Local Computer, you will need to grant read access to the client private key to the user running the client application. To share the client private key with another user, right-click the certificate and hover over **_All Tasks_**. Select **_Manage Private Keys_**:

![image alt text](image_10.png)

In the permissions dialog, select the users or groups to which you would like to grant read access to the client private key.

## Install the CA certificate 

The process for installing the CA certificate is the same as installing the client certificate/private key with some differences:

* The CA certificate is installed in the Trusted Root Certification Authorities store. If using an intermediate CA certificate, this will be installed in the Intermediate Certification Authorities store.

* There will not be a password, as certificates are not password protected. 

The first time you import the certificate, you will be warned about trusting the certificate. If the certificate is trusted, click **_Yes_** to advance through the warning.

## Verify certificate installation

Go to the Action menu and click **_Refresh_**. That will refresh the certificate lists, and you should find the newly imported certificates. The client certificate should be in the Personal store, the root CA certificate should be in the Trusted Root Certification Authorities store, and any Intermediate CA certificates should be in the Intermediate Certification Authorities store. 

## Verify connection to the AKM server

You can use the `Tester` application to verify the connection to the AKM server and test key retrieval and key caching. This will ensure that the network and the certificates just installed are configured properly. See Appendix A: Verify the Connection with Tester for more information.

## Find certificate thumbprints

The configuration options for the AKM Client Library identify certificates by thumbprint. You will need to look up the thumbprints of your certificates in order to configure these certificates in your application. To find a certificate’s thumbprint, double-click on the certificate in the Windows Certificate Manager. Click the **_Details_** tab and scroll to the bottom of the pane:

![image alt text](image_11.png)

Select the **_Thumbprint_** field. The thumbprint will populate in the field below and you can copy it. 

# Chapter 6: Configure Your Client Application

## Overview

After you have installed the required certificates and private keys to the Windows certificate store, you can configure your application to connect with the Key Client assembly for key retrieval or remote encryption.

## `KeyService` and `EncryptionService`

There are two primary object classes in the Key Client assembly: `KeyService` and `EncryptionService`. A `KeyService` object  is used to retrieve encryption keys from the AKM server, and an `EncryptionService` object is used to encrypt or decrypt data remotely on the AKM server. Before using a `KeyService` or `EncryptionService` object, they need to be configured to connect to the server. 

## Configuration

The client and AKM server use certificates and private keys to perform mutual authentication over TLS. In addition, the AKM Client Library for Windows requires a pinned certificate in the client configuration. Any one of the certificates in the server certificate chain must be identified as the "server certificate". This could be the root CA certificate, an intermediate CA certificate, or the AKM server certificate(s). The pinned certificate must be installed in the appropriate Windows certificate store as described elsewhere in this document. A standard approach is to use the root CA certificate and this will be used in the examples below. See Appendix B: Certificate Pinning for a discussion of using AKM server certificates in your configuration as pinned certificates for additional security.

You will need to provide the following information in the configuration:

* IP addresses of the primary AKM server and any additional secondary servers

* Ports on which those AKM servers are configured to perform key retrieval (default: 6000) or remote encryption (default: 6003)

* Thumbprint of the client certificate

* Thumbprint of AKM’s root CA certificate

## Configuration objects

The configuration consists of a `ClientConfiguration` object which identifies a client certificate and one or more server configurations as `ServerConfiguration` objects. The `ClientConfiguration` has some fields that are set to default values, and other fields which must be set. 

These objects classes are defined as:


```
public class ClientConfiguration
{
	public string Name { get; set; }
	public string ClientCertificateThumbprint { get; set; }

	public ServerConfiguration[] ServerConfigurations { get; set; }
}
```

The `Name` field is only informative. If you load the client configuration from the application configuration file using the second format (see below) the name will be set from the configuration. Otherwise, this field is initially set to null and you can use it however you like. 

The `ClientCertificateThumbprint` identifies a client certificate, used to authenticate this client to the AKM server over TLS.  

> **_NOTE:_**  This is the thumbprint of the client certificate you imported to the Windows certificate store in an earlier chapter. The client certificate must be in the Personal certificate store of either Current User or Local Computer and the Windows user ID (i.e. security principal) in effect at the time of the connection needs read permission to the private key. For information on how to grant user access to private keys, see the section Grant user access to the client private key in the previous chapter. 

`ClientConfiguration` contains an array of `ServerConfiguration`:


```
public class ServerConfiguration
{
	public bool AllowCertificateNameMismatch { get; set; }
	public bool CheckCertificateRevocation { get; set; }
	public int CryptoOfficerPort { get; set; }
	public int EncryptionPort { get; set; }
	public string Hostname { get; set; }
	public int KeyRetrievalPort { get; set; }
	public int KmipPort { get; set; }
	public string ServerCertificateThumbprint { get; set; }
}

```

One `ServerConfiguration` object is required, and you will set additional `ServerConfiguration` objects for any secondary (failover) AKM servers. The first in the array is the primary, and any others are secondary.

> **_NOTE:_** If there is a failure to connect to the AKM server, an attempt is made to connect to the next AKM server in the array, and so on to the end of the array. (However, only a connection failure invokes this behavior. A request failure, such as a key-not-found error, for example, will not cause the next server configuration to be tried.)

The `ServerCertificateThumbprint` identifies one of the certificates of the server certificate chain, such as the root CA certificate. This is the "pinned" certificate.

> **_NOTE:_**  The certificate you are identifying by thumbprint must be in the appropriate certificate store of either Current User or Local Computer.

`AllowCertificateNameMismatch` defaults to `false`. If you are using AKM-generated certificates, be sure to set this to `true` as these certificates do not contain the server DNS name. 

You can either configure the client directly in code or in an application configuration file.  The following sections describe these methods for .NET applications. The code snippets here are C# unless otherwise noted. To configure your client application for use with the SQL Server UDF, see the SQL Server UDF Guide.

### Configure the client in code 

You can set the client configuration in code and then pass it to the `KeyService` or `EncryptionService` constructor: 


```
ServerConfiguration serverConfiguration = new ServerConfiguration()
	{
    	AllowCertificateNameMismatch = true,
      Hostname = "192.168.1.80",
    	ServerCertificateThumbprint =
        	"ee fd 06 98 cc ee 2c 0e af d3 42 f3 6d 8a c5 46 46 f0 03 f8"
	};

ServerConfiguration failoverConfiguration = new ServerConfiguration()
	{
    	AllowCertificateNameMismatch = true,
      Hostname = "192.168.1.81",
    	ServerCertificateThumbprint =
        	"69 92 5b 93 62 d8 12 e6 0f e7 9c 98 5d f3 ee 5d ee 79 11 12"
	};

ClientConfiguration clientConfiguration = new ClientConfiguration()
	{
    	ClientCertificateThumbprint =
        	"e8 8b 7e 7a 33 7f d9 8e c2 cc 60 26 39 f9 79 59 83 bd 64 fd",
    	ServerConfigurations =
        	new ServerConfiguration[]
            	{ serverConfiguration, failoverConfiguration }
	};

var encryptionService = new EncryptionService(clientConfiguration);
```

> **_NOTE:_** You can also create a KeyService or EncryptionService object and then fill in the fields. See the EncryptString sample for an example of using this method.


The fields in red are example values. In each `ServerConfiguration` object, supply the hostname/IP address for the AKM server for the `Hostname` value. For the `ServerCertificateThumbprint` value, supply the thumbprint of your root CA certificate.

For the `ClientCertificateThumbprint` value, supply the thumbprint of the client certificate.

### Configure the client in a .NET application configuration file

You can configure the object using the application configuration file (This will be the configuration file ending in `.config`, for example `yourapp.exe.config`). There are two styles of configuration supported. The second format is newer and has more configuration options than the first format.

**First format**

This format has the following limitations:

* You cannot configure multiple key clients. 

* The encryption port cannot be specified in the `app.config` file, and is the default value of 6003 unless you change it programmatically after instantiating the `EncryptionService` object.

Here is an example of configuring the client using the first format:


```
<?xml version="1.0"?>
<configuration>
  <configSections>
	<section name="MyConfig" type="System.Configuration.NameValueSectionHandler" requirePermission="false"/>
  </configSections>
  <MyConfig>
	<add key="Server" value="192.168.1.80:6000, 192.168.1.81:6000"/>
	<add key="ClientCertificateThumbprint" value="e8 8b 7e 7a 33 7f d9 8e c2 cc 60 26 39 f9 79 59 83 bd 64 fd"/>
	<add key="ServerCertificateThumbprint" value="ee fd 06 98 cc ee 2c 0e af d3 42 f3 6d 8a c5 46 46 f0 03 f8"/>
	<add key="AllowCertificateNameMismatch" value="true"/>
  </MyConfig>
</configuration>

```

For the `section name` value, supply a name for the configuration (Example: `MyConfig`)

For the `Server` value, supply the hostname/IP addresses of your AKM servers in a single comma-separated value string. Servers share the same `ServerCertificateThumbprint` value.

For the `ServerCertificateThumbprint` value, supply the thumbprint of your root CA certificate.

For the `ClientCertificateThumbprint` value, supply the thumbprint of your client certificate.

You can then pass the configuration section name ("`MyConfig`") to the `EncryptionService` constructor as a string parameter:


```
var encryptionService = new EncryptionService(“MyConfig”);

```

**Second format**

The second format uses the custom configuration section `TownsendSecurity.KeyClient.ConfigurationSection`. This format supports the configuration of multiple key clients.

Here is an example of the second format:


```
<?xml version="1.0"?>
<configuration>
  <configSections>
	<section name="MyConfig" type="TownsendSecurity.KeyClient.ConfigurationSection, TownsendSecurity.KeyClient" requirePermission="false"/>
  </configSections>
  <MyConfig>
	<keyClients>
  	<add name="PERLEBERG" clientCertificateThumbprint="e8 8b 7e 7a 33 7f d9 8e c2 cc 60 26 39 f9 79 59 83 bd 64 fd" keyServer="192.168.1.80" failoverKeyServer="10.0.1.254"/>
	<add name="DRESDEN" clientCertificateThumbprint="e8 8b 7e 7a 33 7f d9 8e c2 cc 60 26 39 f9 79 59 83 bd 64 fd" keyServer="192.168.1.82"/>
	</keyClients>
	<keyServers>
  	<add hostname="192.168.1.80" keyRetrievalPort="6000" encryptionPort="6003" serverCertificateThumbprint="ee fd 06 98 cc ee 2c 0e af d3 42 f3 6d 8a c5 46 46 f0 03 f8" allowCertificateNameMismatch="true" checkCertificateRevocation="false"/>
  	<add hostname="192.168.1.81" keyRetrievalPort="6000" encryptionPort="6003" serverCertificateThumbprint="ee fd 06 98 cc ee 2c 0e af d3 42 f3 6d 8a c5 46 46 f0 03 f8" allowCertificateNameMismatch="true"/>
  	<add hostname="192.168.1.82" keyRetrievalPort="6000" encryptionPort="6003" serverCertificateThumbprint="ee fd 06 98 cc ee 2c 0e af d3 42 f3 6d 8a c5 46 46 f0 03 f8" allowCertificateNameMismatch="true"/>
	</keyServers>
  </MyConfig>
</configuration>

```

For the `section name` value, supply a name for the configuration (Example: `MyConfig`).

In the `<keyClients>` section, supply a `name` for the client (optional), the `ClientCertificateThumbprint` of the client certificate, and the IP address of the AKM server for the `keyServer` value. You can configure as many clients as you wish.

In the `<keyServers>` section, supply the thumbprint of your root CA certificate. You can add additional secondary servers as in the example above. 

In the example above, clients **PERLEBERG** and `DRESDEN` have been configured, and two secondary AKM servers have been configured. 

The following example passes the configuration section ("`MyConfig`") to the `EncryptionService` constructor as a string parameter. An optional second parameter to the constructor selects the client by name (“`PERLEBERG`”), or defaults to the first client listed if the second parameter is not given:


```
var encryptionService = new EncryptionService(“MyConfig”, “PERLEBERG”);

```

## Next steps

Your client application has now been configured to communicate with the Key Client assembly. See subsequent chapters and the `EncryptFile` and `EncryptString` sample code for information on implementing key retrieval or remote encryption.

# Chapter 7: Retrieve Encryption Keys

## Overview

This chapter outlines how to retrieve encryption keys using the `KeyService` class to encrypt and decrypt a string. Code snippets below are in C# and are excerpted from a full sample which you can find later in the chapter. The sample code `EncryptString` included with the AKM Client Library also shows remote encryption of the string.

First you will create a `KeyService` object and configure the object, then you will use the `GetSymmetricKey` method to retrieve keys. The `KeyService` object will also perform in-memory key caching with a configurable timespan, so that `GetSymmetricKey` will first check the memory cache before going to AKM Server. See Chapter 8: Key Caching for more information on caching retrieved encryption keys.

## Create the `KeyService` object

This example uses a `using` statement to create the `KeyService` object. Note that class `KeyService` implements the `IDisposable` interface, and the `using` statement will call `Dispose` on the object when it goes out of scope.


```
    static void EncryptLocal(string keyName, string data)
    {
        using (var keyService = new KeyService())
        {
        }
    }

```

## Configure the `KeyService` object

The `KeyService` object has a `ClientConfiguration` field. The `ClientConfiguration` has some fields that are set to default values, and other fields which must be set. Here, the `ClientConfiguration` is passed to a pair of static methods to set required fields.

`AllowCertificateNameMismatch` defaults to `false` 

If you are using AKM-generated certificates, be sure to set this field to `true` as these certificates do not contain the server DNS name.


```
    private static void EncryptLocal(string keyName, string data)
    {
        using (var keyService = new KeyService())
        {
            SetClientConfiguration(keyService.ClientConfiguration);
        }
    }

    private static void SetClientConfiguration(ClientConfiguration clientConfiguration)
    {
        clientConfiguration.ClientCertificateThumbprint =
            "e8 8b 7e 7a 33 7f d9 8e c2 cc 60 26 39 f9 79 59 83 bd 64 fd";

        clientConfiguration.ServerConfigurations = new ServerConfiguration[] { new ServerConfiguration() };


        SetServerConfiguration(clientConfiguration.ServerConfigurations[0]);
    }

    private static void SetServerConfiguration(ServerConfiguration serverConfiguration)
    {
        serverConfiguration.ServerCertificateThumbprint =
            "ee fd 06 98 cc ee 2c 0e af d3 42 f3 6d 8a c5 46 46 f0 03 f8";

        serverConfiguration.AllowCertificateNameMismatch = true;

        serverConfiguration.Hostname = "192.168.1.80";
    }

```

## Retrieve the encryption key

Once the `KeyService` object is configured, the `GetSymmetricKey` method is called to retrieve an encryption key from the AKM server. You can retrieve a key by key name, key instance, or both. 

Specifying the key name retrieves the current instance of the key. You can either supply the key name alone or supply the key name and set the instance name to blanks (null). For example:


```
var symmetricKey1 = keyService.GetSymmetricKey(“AES128”);

var symmetricKey2 = keyService.GetSymmetricKey(“AES128”, null);

```

Specifying the instance name alone or the key name and instance name retrieves a particular instance of a key:


```
var symmetricKey3 = keyService.GetSymmetricKey(null, “xerYqb76hVaI7OCEvCZx0w==”);

var symmetricKey4 = keyService.GetSymmetricKey(“AES128”, “xerYqb76hVaI7OCEvCZx0w==”);

```

This example retrieves the encryption key by name to get the current instance:

    `private static void EncryptLocal(string keyName, string data)`

    `{`

        `using (var keyService = new KeyService())`

        `{`

            `SetClientConfiguration(keyService.ClientConfiguration);`

            `var key = keyService.GetSymmetricKey(keyName);`

            `instance = key.Instance;`

        `}`

    `}`

The key instance will be stored with the ciphertext so that later, for decryption, the key can be retrieved by instance.

## Enable key caching

The following sample code retrieves the key twice. You enable key caching in the `KeyService` object by setting a non-zero timespan on the  `KeyCacheTimeSpan` property. The first retrieval will retrieve the key from the AKM server. A second retrieval within 30 seconds of the first will just get the cached copy of the key, avoiding a second connection to the AKM server on the second retrieval. Since the retrievals occur back-to-back, setting a timespan of 30 seconds will be adequate. 


```
   private static void EncryptLocal(string keyName, string data)
   {
       using (var keyService = new KeyService())
       {
           // Turn on caching with a 30-second timespan. The key will
           // be cached on the first retrieval and so we won't go back
           // to the server on the second retrieval. We'll just get the
           // cached copy.
           keyService.KeyCacheTimeSpan = new TimeSpan(0, 0, 30);

           SetClientConfiguration(keyService.ClientConfiguration);

           var key = keyService.GetSymmetricKey(keyName);

           instance = key.Instance;
       }
   }

```

## The `SymmetricKey` object

The `GetSymmetricKey` method returns an object of class `SymmetricKey`. This object has properties of the key such as `Name`, `Instance`, `KeySize`, and `Expiration`. The actual key value is held encrypted in memory. To access the key value, you can call the method `KeyBytes`. Here, the key value is accessed to set the Key field of the algorithm object.


```
algorithm.Key = key.KeyBytes();

```

The key bytes are encrypted by an RSA key created in the Windows key store. See Chapter 8:  Key Caching for more information.

## Encrypt using Microsoft Crypto

Here, the Microsoft `RijndaelManaged` class is used to encrypt an array of bytes. In this simplest example, the `TransformFinalBlock` method of the encryptor object is called directly. In the more involved `EncryptFile` example, the Microsoft `CryptoStream` class is used.

The new `RijndaelManaged` object has a random IV (initialization vector) already set. This is saved along with the ciphertext to be used later in the decryption:

    `using (var algorithm = new RijndaelManaged())`

    `{`

        `initializationVector = algorithm.IV;`

        `algorithm.Key = key.KeyBytes();`

        `using (var encryptor = algorithm.CreateEncryptor())`

        `{`

            `ciphertext = encryptor.TransformFinalBlock(plaintext, 0, plaintext.Length);`

        `}`

    `}`

## Encrypting strings vs. bytes

Strings need to be converted to byte arrays before encryption, and back to strings after decryption. Here, the UTF-8 encoding is used to convert the input string to an array of plaintext bytes:

    `static void EncryptLocal(string keyName, string data)`

    `{`

        `byte[] plaintext;`

        `plaintext = Encoding.UTF8.GetBytes(data);`

    `}`

## Full example

Here all of the elements are shown together to locally encrypt the string, and immediately decrypt the string. The `EncryptString` sample code included with the AKM Client Library also shows remote encryption of the string.


```
using System;
using System.Text;

namespace EncryptDecryptString
{
    using System.Security.Cryptography;

    using TownsendSecurity.KeyClient;

    class Program
    {
        static void SetServerConfiguration(ServerConfiguration serverConfiguration)
        {
            serverConfiguration.ServerCertificateThumbprint =
                "ee fd 06 98 cc ee 2c 0e af d3 42 f3 6d 8a c5 46 46 f0 03 f8";

            serverConfiguration.AllowCertificateNameMismatch = true;

            serverConfiguration.Hostname = "192.168.1.80";
        }

        static void SetClientConfiguration(ClientConfiguration clientConfiguration)
        {
            clientConfiguration.ClientCertificateThumbprint =
                "e8 8b 7e 7a 33 7f d9 8e c2 cc 60 26 39 f9 79 59 83 bd 64 fd";

            SetServerConfiguration(clientConfiguration.ServerConfigurations[0]);
        }

        static void EncryptLocal(string keyName, string data)
        {
            string instance;
            byte[] plaintext;
            byte[] initializationVector;
            byte[] ciphertext;
            byte[] roundtrip;

            plaintext = Encoding.UTF8.GetBytes(data);

            using (var keyService = new KeyService())
            {
                // Turn on caching with a 30-second timespan. The key will
                // be cached on the first retrieval and so we won't go back
                // to the server on the second retrieval. We'll just get the
                // cached copy.
                keyService.KeyCacheTimeSpan = new TimeSpan(0, 0, 30);

                SetClientConfiguration(keyService.ClientConfiguration);

                var key = keyService.GetSymmetricKey(keyName);

                instance = key.Instance;

                using (var algorithm = new RijndaelManaged())
                {
                    initializationVector = algorithm.IV;

                    algorithm.Key = key.KeyBytes();

                    using (var encryptor = algorithm.CreateEncryptor())
                    {
                        ciphertext = encryptor.TransformFinalBlock(plaintext, 0, plaintext.Length);
                    }
                }

                key = keyService.GetSymmetricKey(keyName, instance);

                using (var algorithm = new RijndaelManaged())
                {
                    algorithm.Key = key.KeyBytes();

                    algorithm.IV = initializationVector;

                    using (var decryptor = algorithm.CreateDecryptor())
                    {
                        roundtrip = decryptor.TransformFinalBlock(ciphertext, 0, ciphertext.Length);
                    }
                }

                var result = Encoding.UTF8.GetString(roundtrip);

                Console.WriteLine("data: " + data);
                Console.WriteLine("plaintext: " + BitConverter.ToString(plaintext));
                Console.WriteLine("keyName: " + keyName);
                Console.WriteLine("instance: " + instance);
                Console.WriteLine("initializationVector: " + BitConverter.ToString(initializationVector));
                Console.WriteLine("ciphertext: " + BitConverter.ToString(ciphertext));
                Console.WriteLine("roundtrip: " + BitConverter.ToString(roundtrip));
                Console.WriteLine("result: " + result);
            }
        }

        static void Main(string[] args)
        {
        	    try
        	    {
                EncryptLocal("AES256", "Hello World");
            }
            catch (Exception e)
            {
                Console.WriteLine("Exception caught: " + e.Message);
            }

            EncryptLocal("AES256", "Hello World");

            Console.ReadLine();
        }
    }
}

```

# Chapter 8: Key Caching

## Overview

Key caching is a feature of the `KeyService` object and is used when retrieving encryption keys. When the key caching feature is activated, retrieved encryption keys are held in memory for a configurable period of time. To activate key caching, you will set a timespan in the `ClientConfiguration` object. Retrieved encryption keys will be encrypted by an RSA key and held in memory for this timespan or until the cache is cleared. This "key-caching key" is an RSA key created in the Windows key store, and it can be periodically rolled. 

## Setting the timespan

Retrieved keys are cached by the `KeyService` object when its `KeyCacheTimespan` property is set to a non-zero interval. For example, when the timespan is set to two hours, then once a key is retrieved from AKM Server, a copy is kept for two hours. During the two hours, regardless of what happens to the server or to the key (e.g. it could have been deleted or revoked) the key can be retrieved locally again and again from the `KeyService` cache. After the timespan expires, the next time the same key is retrieved, `KeyService` will go back to AKM server for a fresh copy.

You can set the timespan to whatever length of time you wish, but 4 to 6 hours might be an adequate balance between improved performance and maintaining the current state of the key.

Here is an example of setting the `KeyCacheTimespan` property to two hours in the client configuration of the `KeyService` object:


```
<add key="KeyCacheTimeSpan" value="02:00:00"/>

```

Or by setting the `KeyCacheTimeSpan` property:

`keyService.KeyCacheTimeSpan = new TimeSpan(2, 0, 0);`

The `EncryptString` sample code makes use of key caching. 

## Clearing the cache

The cache can be cleared by setting the `KeyCacheTimespan` property to a zero timespan, or by calling the `keyService.ClearKeyCache()` method.

## Key-caching key

Cached encryption keys are held in memory by `SymmetricKey` objects and the key value is encrypted by a "key-caching key". The key-caching key is an RSA key created by the `KeyService` object specifically for key caching. One key-caching key is shared by all `KeyService` objects, per Windows userid. If the key is rolled, there will be two key-caching keys - the previous and the current. Key caching keys are located in the Windows key store under the Windows signon of the current user. There can only be up to two key-caching keys created per user. The key-caching keys are manually managed by the user and can be deleted at any time. If you roll the key, older copies of the key will be deleted as only two instances of the key are maintained at any given time (see below). 

### Rolling the key-caching key

Rolling the key-caching key creates a second RSA key in the Windows key store. Subsequent rolls will replace the first of the two keys and create another new key, such that there are always two keys per user: the previous and the current.

When the key-caching key is rolled, existing `SymmetricKey` objects in the cache can still be used since the previous key still exists. But if the key is rolled twice in succession, any existing `SymmetricKey` objects become invalid; their key value is lost and they will need to be retrieved again from the AKM server.

The key-caching key can be rolled by calling the `keyService.RollKeyCachingKey()` method.

### Locating the key-caching key

The `keyService.KeyCachingKeyFilenames()` method returns an array of strings listing the filenames used to hold key-caching keys for the current Windows user. If you know the directory in which the keys are stored, you can locate the RSA key containers used to hold the key-caching key(s). The following website can help you locate the directory where the key-caching key files are stored: 

[http://msdn.microsoft.com/en-us/library/windows/desktop/bb204778%28v=vs.85%29.aspx](http://msdn.microsoft.com/en-us/library/windows/desktop/bb204778%28v=vs.85%29.aspx)

# Chapter 9: Perform Remote Encryption of a File

## Overview

This chapter outlines how to use the `EncryptFile` sample code included with the AKM Client Library to perform remote encryption and decryption of a file on the AKM server. The sample code also shows key retrieval/local encryption of a file. Code snippets below are in C# and are excerpted from the full sample. 

## Create the `EncryptionService` object

Here, the `using` statement is used. Note that class `EncryptionService` implements the `IDisposable` interface, and the using statement will call `Dispose` on the object when it goes out of scope.

The `EncryptionService` constructor is passed the name of a section in the .NET application configuration file, where it will read its configuration from.

    `private const string AppConfigSectionName = "EncryptDecryptFile";`

    `static void EncryptRemote()`

    `{`

        `using (var keyService = new EncryptionService(AppConfigSectionName))`

        `{`

        `}`

    `}`

## Create the `Encryptor` Object

The `CreateEncryptor` method of the `EncryptionService` object creates the `encryptor` object, which implements Microsoft’s ICryptoTransform interface. Here we create the `encryptor` to use AES CBC mode.


```
   private const string AppConfigSectionName = "EncryptDecryptFile";

    static void EncryptRemote()
    {
        using (var encryptionService = new EncryptionService(AppConfigSectionName))
        {
            using (var encryptor = encryptionService.CreateEncryptor(CipherMode.CBC))
            {
            }
        }
    }

```

## Set the key name

The `KeyName` property is set on the `encryptor` object to choose an encryption key on the AKM server by key name. You can also choose the encryption key by key name and instance. 


```
    static void EncryptRemote()
    {
        using (var encryptionService = new EncryptionService(AppConfigSectionName))
        {
            using (var encryptor = encryptionService.CreateEncryptor(CipherMode.CBC))
            {
                encryptor.KeyName = this.keyName;
            }
        }
    }

```

> **_NOTE:_** With the AKM Encryption Service, you cannot choose an encryption key by instance name alone. You must supply the key name with the correct instance. If you do not supply an instance name, AKM will use the current instance of the key.

## Store the initialization vector and key instance 

The new `encryptor` object has a fresh, random IV (initialization vector) which you can use. You will want to keep a copy of this to use later for the decryption.

The `encryptor` object also has an `Instance` property, but this is not set until after the encryption has started. Here, the instance name is stored after encryption has completed. (The `EncryptContent` method is performing the encryption, which is described below.)


```
    static void EncryptRemote()
    {
        using (var encryptionService = new EncryptionService(AppConfigSectionName))
        {
            using (var encryptor = encryptionService.CreateEncryptor(CipherMode.CBC))
            {
                encryptor.KeyName = this.keyName;

                // The brand new object comes with a new, random IV.
                // We will record this, later, in the output.
                this.initializationVector = encryptor.IV;

                this.EncryptContent(encryptor);

                // The instance is revealed after encrypting something.
                // We will record this, too, in the output.
                this.keyInstance = encryptor.Instance;
            }
        }
    }

```

## Encrypting a stream

Here we use Microsoft’s `CryptoStream` class to run the encryption. The input and output files are opened as streams, and then the CryptoStream is inserted in between the two as the input stream is copied to the output stream.

The `CryptoStream` class will use Microsoft’s ICryptoTransform interface, which the encryptor implements.

Before copying the input to the output, through the CryptoStream, the output stream is moved ahead to make room at the beginning of the output file. Later you will return and write the key instance and initialization vector here after you have encrypted the data.


```
private void EncryptContent(ICryptoTransform transform)
{
    using (var inputStream = this.OpenFile(this.inputPath, OpenParam.Read))
    using (var outputStream = this.OpenFile(this.outputPath, OpenParam.Create))
    {
        // Move the output stream ahead to make room to later write
        // info at the start of the output ahead of the ciphertext.
        outputStream.Position = InstanceSize + InitializationVectorSize;

        this.TransformStream(inputStream, outputStream, transform);
    }
}

private void TransformStream(Stream inputStream, Stream outputStream, ICryptoTransform transform)
{
    using (var cryptoStream = new CryptoStream(outputStream, transform, CryptoStreamMode.Write))
    {
        this.CopyStream(inputStream, cryptoStream);
    }
}

```

The `CopyStream` method is writing large chunks of the input stream at a time through the `CryptoStream`, which optimizes throughput of the AKM Client Library over the network connection to the AKM server.


```
private const int CopyStreamBufferSize = 128 * 1024;

private void CopyStream(Stream inputStream, Stream cryptoStream)
{
    // The buffer size is arbitrary, however a large buffer size will
    // optimize throughput when using remote encryption.
    var buffer = new byte[CopyStreamBufferSize];

    int count;

    while ((count = inputStream.Read(buffer, 0, buffer.Length)) > 0)
    {
        cryptoStream.Write(buffer, 0, count);
    }
}

```

## Write the initialization vector and key instance to the output file

After the encryption has completed, the output stream is closed. Here, you re-open the output file to store the key instance and initialization vector in the space left at the beginning of the file. Later, when you decrypt the file, you will need those two pieces of information.


```
private void WriteInfo()
{
    using (var outputStream = this.OpenFile(this.outputPath, OpenParam.Modify))
    {
        this.WriteInstance(outputStream, this.keyInstance);

        this.WriteBytes(outputStream, this.initializationVector);
    }
}

```

Here is the `OpenFile` method, for reference, showing the mode and access used.


```
private enum OpenParam
{
    Create,
    Modify,
    Read
}

private FileStream OpenFile(string path, OpenParam param)
{
    FileMode mode;
    FileAccess access;

    switch (param)
    {
        case OpenParam.Create:
            mode = FileMode.Create;
            access = FileAccess.Write;
            break;

        case OpenParam.Modify:
            mode = FileMode.Open;
            access = FileAccess.ReadWrite;
            break;

        default:
        case OpenParam.Read:
            mode = FileMode.Open;
            access = FileAccess.Read;
            break;
    }

    return new FileStream(path, mode, access);
}

```

## Sample code

The sample code for `EncryptFile` builds on this, and includes local encryption as an alternative to remote encryption. The sample code also includes adding HMAC authentication to the encrypted output file.

# Chapter 10: Problem Determination

## Error reporting

The AKM Client Library for Windows reports errors by throwing exceptions. The exceptions are either of Microsoft exception classes, such as `ArgumentException` or `InvalidOperationException`, or of exception classes specific to the client library, which derive from the `KeyClientException` class. 

When the exception represents an error encountered on the server then the server error code is reported in the exception in the field `ServerErrorCode`. These are errors the server has returned in response to a key retrieval or encryption request. The exception’s `ServerErrorCode` will match the error code logged in the AKM server error log. However, the message text of the exception may not match the error text that appears in the AKM server error log. View the `akmerror.log` file on the AKM server for more information. See the AKM Server Management Guide for information on viewing the `akmerror.log` file.

The exception classes are defined as follows:

Exception Class | Description
--- | ---
`KeyClientException` | This is the base class for the client library exception classes listed here.
`ConfigurationException` | This exception class is used for a number of configuration errors, for example, the client certificate is not found, or the associated private key is missing.
`ConnectAuthenticationException` | This exception class is used for errors with the TLS certificates, or other connection problems, other than TCP socket errors (which are reported as NetworkException -- cf. below.)
`KeyAccessDeniedException` | For the specific case of being denied access to a key. For example, the key may be set to Group Access, and the client certificate does not have the correct Group.
`KeyExpiredException` | For the specific case of attempting to retrieve an expired key.
`KeyNotFoundException` | For the specific case of the specified key (or instance) not found on the server.
`KeyPreActiveException` | For the specific case of attempting to retrieve a key that has not yet been activated.
`KeyRevokedException` | For the specific case of attempting to retrieve a revoked key.
`NetworkException` | Wraps a TCP SocketException adding more detail to the exception message.
`ServerException` | When a server request fails in a general, but expected way. For example, you are attempting to retrieve a key but connecting with an admin client (Crypto Officer) certificate.
`ServerFailureException` | When a server request fails in a completely unexpected way. For example, the request is rejected by the server because it is malformed. This could indicate a bug in the server or in the client library, and should be reported.



In some cases, the thrown exception carries (wraps) an inner exception pertaining to the problem encountered. For example, a failure to connect to the server due to a TCP networking error will cause a `NetworkException` to be thrown, which wraps an inner exception which is the actual TCP exception thrown by the Microsoft TCP API.

## Microsoft exception classes

In addition, the following Microsoft exception classes are included:

* `ArgumentException`

* `ArgumentNullException`

* `ArgumentOutOfRangeException`

* `CryptographicException`

* `InvalidOperationException`

* `RankException`

## Error handling

Here is a simplified example from the `EncryptString` sample code of catching an exception:

    `static void Main(string[] args)`

    `{`

        `try`

        `{`

            `EncryptLocal("AES256", "Hello World");`

            `EncryptRemote("AES256", "Hello World");`

        `}`

        `catch (Exception e)`

        `{`

            `Console.WriteLine("Exception caught: " + e.Message);`

        `}`

        `Console.ReadLine();`

    `}`

You could refine your exception catching, for example:

    `static void Main(string[] args)`

    `{`

        `try`

        `{`

            `EncryptLocal("AES256", "Hello World");`

            `EncryptRemote("AES256", "Hello World");`

        `}`

        `catch (KeyClientException e)`

        `{`

            `Console.WriteLine("Key client error, " + e.GetType().Name + ": " + e.Message);`

        `}`

        `catch (Exception e)`

        `{`

            `Console.WriteLine("Exception caught: " + e.Message);`

        `}`

        `Console.ReadLine();`

    `}`

There are many other resources available which cover exception handling in more detail.

## .NET tracing

The AKM Client Library for Windows uses .NET tracing to help diagnose problems. You can configure .NET tracing in an application configuration file (see below).

In some cases, details of the error encountered are included in the trace message text. For example, a failure to connect to the server may be due to a TCP networking error. In this case, the trace message will include details about the TCP networking error. For server-reported errors, the `ServerErrorCode` field is set to the error code returned in the server response.

## Tracing points

This section describes the tracing points in the AKM Client Library for Windows.

### Errors traced

Errors traced | Description
--- | ---
Failed to connect to server (no retry) | Occurs when the client library fails to make a connection to the server, and there are no more failover servers to try.
Key retrieval failed | Occurs when the client library sends a request to retrieve a key, and the server sends back an error response.
Encrypt or decrypt request failed | Occurs when the client library sends a request to remotely encrypt or decrypt data, and the server sends back an error response.
Failed to decrypt key value | Occurs when the client library has retrieved a key, but then fails to decrypt the key value when the client application calls the `SymmetricKey.KeyBytes` method, for example.



### Warnings traced

Warnings traced | Description
--- | ---
Failed to connect to server (retrying) | Occurs when the client library fails to make a connection to the server, and there is a failover server left to try, then only a warning is traced. The client library will then attempt to connect to the next failover server that is configured.



### Information-level traced

Information-level traced | Description
--- | ---
Connected to server | Occurs when the client library has successfully connected to the server.
Key retrieved | Occurs when the client library has retrieved a key (for the `KeyService.GetSymmetricKey` method), whether from the server, or from the `KeyService` key cache.
Remote encrypt or decrypt succeeded | Occurs when the client library has successfully remotely encrypted or decrypted data in response to the `ICryptoTransform.TransformBlock` or `ICryptTransform.TransformFinalBlock` methods of the encryptor or decryptor objects provided by the `EncryptionService` class.
Key-caching key rolled | Occurs when the key-caching key, which is used to protect the key value bytes for `SymmetricKey` objects, has been rolled.



### Verbose-level traced

Verbose-level traced | Description
--- | ---
Client certificate | Indicates the client certificate used to connect to the server.
Server certificate | Indicates the server certificate received from the server.
Server chain certificates | Indicates additional chain certificates for the server, either received from the server or found in the local certificate stores. These would be any intermediate CA certificates, and the final, root CA certificate.
SSL policy error | Indicates any TLS errors for the server certificate chain. For example, it is an error if the server certificate does not contain the server’s DNS name. However, this particular error may be ignored if the client is configured to ignore the error of the server’s DNS name not appearing in the server certificate (`ServerConfiguration.AllowCertificateNameMismatch`).
Server chain error | Indicates any TLS errors found in the server certificate chain. For example, a given certificate in the chain may be expired. Another example is if the server certificate is untrusted (if there is no root CA certificate installed in the local certificate stores that establishes trust for the server certificate). This particular TLS error, of an untrusted server certificate, is ignored by the client library and the connection is allowed if a copy of the server certificate is installed in the Trusted People certificate store, and if the server certificate itself is identified by thumbprint in the client configuration (`ServerConfiguration.ServerCertificateThumbprint`). This allows the client library to trust the server certificate without requiring that the server’s root CA certificate be installed on the client. See Appendix B: Certificate Pinning for more information.
Encryption API call | Each call to the `ICryptoTransform.TransformBlock` and `ICryptoTransform.TransformFinalBlock` methods the encryptor and decryptor objects of the `EncryptionService` are traced.



## Configure tracing

You can configure tracing to a text file in the application configuration file. The trace source is named `TownsendSecurity.KeyClient`. You can configure the following levels of tracing: `Error`, `Info`, `Off`, `Verbose`, and `Warning`. See Microsoft documentation for more information. You can specify a full path and file name for the trace file, or you can specify only a file name and the file will be placed in the application configuration file directory. You must have write permissions for this directory for this method to work.

The following lines in an application configuration file add logging to a text file named `Trace.txt` with `Verbose` level of tracing:


```
<configuration>
        <system.diagnostics>
          <trace autoflush="true" />
          <sources>
            <source name="TownsendSecurity.KeyClient">
              <listeners>
                <add initializeData="Trace.txt" traceOutputOptions="DateTime" type="System.Diagnostics.TextWriterTraceListener" name="AnyNameHere" />
              </listeners>
            </source>
          </sources>
          <switches>
            <add name="TownsendSecurity.KeyClient" value="Verbose" />
          </switches>
        </system.diagnostics>
	</configuration>

```


# Chapter 11: Support

 

Technical support is available via the website at[ http://townsendsecurity.com/support/ticket](http://townsendsecurity.com/support/ticket).

Please see the Townsend Security Maintenance Policy you received with your purchase for information on fees, online technical support, documentation, license transferability and upgrades, software maintenance, hardware maintenance, customer responsibilities, limitations, disclaimers, lapsed maintenance, enhanced maintenance services, and other topics.

 

## Enhanced maintenance services

Townsend Security offers an Enhanced Annual Maintenance contract that includes priority telephone support, 24/7 (24 hours a day, 7 days a week) support, and other benefits for an additional charge. 

# Appendix A: Verify the Connection with Tester

The sample application `Tester` can be used to verify connection to the AKM server and the proper operation of key retrieval. This application is located in your `Program Files\Townsend Security\AKM Client Library\bin` folder after the installation of the AKM Client Library. 

## Obtain a test encryption key

Before continuing, you will need to obtain the name of an encryption key on the AKM server from your Crypto Officer. 

> **_SECURITY ALERT:_**  It is strongly recommended that you use a test encryption key for this exercise to avoid accidental disclosure of a production key.

## Certificates

If you have already installed certificates to the Windows certificate store, you do not need to install them with Tester and you can skip the "Install the certificate files" section below. Note that the method of installing certificates below installs certificates to the certificates stores for “Current User”.

## Server information

You will need the IP address of the AKM server and the port number for key retrieval (the default is 6000).

## Install the certificate files

Browse to the directory that contains the client certificate/private key `.p12` file:

![image alt text](image_12.png)

Double click the client certificate/private key file.

Windows will launch the Certificate Import Wizard:

![image alt text](image_13.png)

Select **_Current User_** and click the **_Next_** button to start the import process.

 

The following dialog is displayed with the full path to the certificate file:

![image alt text](image_14.png)

Click the **_Next_** button to continue. 

The password dialog is displayed:

![image alt text](image_15.png)

Enter the password for the client private key. If you are deploying AKM for VMware, Windows Azure, or Amazon Web Services, a password has been assigned to the client private key. For HSM and Cloud HSM deployments, if you set a password on the client private key, enter it now. 

Leave the **_Enable strong private key protection_** box unchecked. Leave the box for **_Include all extended properties_** unchecked.

Click the **_Next_** button to continue.

 

The following dialog is displayed:

![image alt text](image_16.png)

Select the option to **_Automatically select the certificate store based on the type of certificate._**

Click the **_Next_** button to continue.

The completion dialog is displayed:

![image alt text](image_17.png)

Click the **_Finish_** button to continue.

 

The following warning dialog is displayed:

![image alt text](image_18.png)

Click **_Yes_** to install the certificate. Click **_OK_** on the completion dialog to complete the certificate import.

## Run the Tester application

When running the `Tester` application, you must know the IP address and port number for key retrieval of the AKM server, the name of the CA certificate and client certificates/private key, and the name of the encryption key you want to retrieve. 

> **_IMPORTANT:_**  If using AKM-generated certificates, be sure to select the option to **_Allow server certificate name mismatch_**. If this box is not checked, the connection to the server may fail.

Locate the Tester application and launch it by double-clicking the file:

![image alt text](image_19.png)

The application will start and display the following dialog:

![image alt text](image_20.png)

**Server(s):** Enter the server address and port number separated by a colon (`:`).  For example, "10.0.1.29: 6000". 6000 is the default key retrieval port. 

![image alt text](image_21.png)

**Client certificate thumbprint:** Click the **_Select_** button. This displays a list of certificate thumbprints:

![image alt text](image_22.png)

Select the client certificate/private key. Click the **_OK_** button to populate the "Client certificate thumbprint" field:

![image alt text](image_23.png)

**Server certificate thumbprint:** Click the **_Select_** button. A list of certificate authority certificates is displayed:

![image alt text](image_24.png)

Select the CA certificate. Click the **_OK_** button to populate the "Server certificate thumbprint" field:

![image alt text](image_25.png)

> **_IMPORTANT:_**  Check the box to **_Allow server certificate name mismatch_**.  

<!-- -->

> **_NOTE:_** You can tell Tester to use the XML configuration file by checking the “Use named section in application configuration file” box.

![image alt text](image_26.png)

**Key name:** Enter a name for the encryption key you want to retrieve. In this example, the encryption key is named "Key01-128".

![image alt text](image_27.png)

To test key caching, enter a timespan. You can also clear the cache, roll the key-caching key, and retrieve key-caching key filenames for the current Windows user.

Click the **_Retrieve_** button to retrieve the encryption key from the AKM server.

If the key retrieval is successful, the following dialog is displayed:

![image alt text](image_28.png)

Click the **_OK_** button to continue. You may also test the retrieval of other keys.

# Appendix B: Certificate Pinning

The client and AKM server use certificates and private keys to perform mutual authentication over TLS. In addition, the Windows Client library requires a pinned certificate. Any one of the certificates in the server certificate chain must be identified as the "server certificate" in the client configuration. It can be the CA certificate, an intermediate CA certificate, or the AKM server certificate itself. The pinned certificate must be installed in the appropriate Windows certificate store as described elsewhere in this document.

## Standard approach

The standard approach as described in this document is to install the root CA certificate onto the client in addition to the client certificate/private key. With the AKM Client Library for Windows, you can use the alternate method of installing AKM’s server certificates on the client instead of the root CA certificate. This creates a situation where the server certificates are essentially "whitelisted" above and beyond simple trust of the root CA certificate, and you avoid having the root CA certificate installed on the client.

## Using AKM server certificates as pinned certificates

In order to use the AKM server certificates as pinned certificates in the client configuration, first determine if you have multiple AKM servers in your high availability configuration with different server certificates. If so, you will need to install each AKM server certificate on the client. If you only have one AKM server or you have the same server certificates on each server, you will only need to install one certificate. Install server certificates in `.pem` format in the "Trusted People" store of Current User or Local Computer, and reference the thumbprint(s) of the AKM server certificate(s) in your client application. 

If you have more than one AKM server with unique server certificates, you will need to add the thumbprint for each AKM server certificate to your client configuration. If you are using an application configuration file, use the second format as documented above, as the first format does not support the use of multiple servers with unique certificate thumbprints. 

