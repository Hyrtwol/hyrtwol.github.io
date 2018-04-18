# RSA

## Links

* [Walkthrough: Encrypting Configuration Information Using Protected Configuration](http://msdn.microsoft.com/en-us/library/dtkwfdky.aspx)
* [Encrypting and Decrypting Configuration Sections](http://msdn.microsoft.com/en-us/library/zhhddkxy.aspx)
* [RSACryptoServiceProvider Class](http://msdn.microsoft.com/en-us/library/system.security.cryptography.rsacryptoserviceprovider.aspx)
* [Using RSA in C#](http://stackoverflow.com/questions/4698161/using-rsa-in-c-sharp)
* [Using ASPNet_Regiis to encrypt custom configuration section](http://stackoverflow.com/questions/786661/using-aspnet-regiis-to-encrypt-custom-configuration-section-can-you-do-it)

## Key locations

The user keys are stored in:

	%APPDATA%\Microsoft\Crypto\RSA

Machine keys are in:

	%ALLUSERSPROFILE%\Microsoft\Crypto\RSA\MachineKeys

## Command line using aspnet_regiis

### Grant local user

You need to grant access to the user that runs the app:

	aspnet_regiis -pa "NetFrameworkConfigurationKey" "NT AUTHORITY\NETWORK SERVICE"

If you don't run as "NETWORK SERVICE" (Running iis express in VS will run as the local user) use:

	aspnet_regiis -pa "NetFrameworkConfigurationKey" "247MS\tlc"

Result:

	Microsoft (R) ASP.NET RegIIS version 4.0.30319.17929
	Administration utility to install and uninstall ASP.NET on the local machine.
	Copyright (C) Microsoft Corporation.  All rights reserved.
	Adding ACL for access to the RSA Key container...
	Succeeded!  

### Export the key-pair

You need to export the key-pair and later import it on the host that need access to the config:

	aspnet_regiis -px "NetFrameworkConfigurationKey" rsa-keypair.xml -pri

Result:

	Microsoft (R) ASP.NET RegIIS version 4.0.30319.17929
	Administration utility to install and uninstall ASP.NET on the local machine.
	Copyright (C) Microsoft Corporation.  All rights reserved.
	Exporting RSA Keys to file...
	Succeeded!
 
### Encrypt web.config

This command will encrypt and change the web.config (so remember to commit before you run it just in case)

	aspnet_regiis -pef "connectionStrings" "D:\E247\experimental\RSA\RSA.Web"

Result:

	Microsoft (R) ASP.NET RegIIS version 4.0.30319.17929
	Administration utility to install and uninstall ASP.NET on the local machine.
	Copyright (C) Microsoft Corporation.  All rights reserved.
	Encrypting configuration section...
	Succeeded!

### Import the key-pair

The key pair must be imported on the host that need to read the encrypted config section:

	aspnet_regiis -pi "NetFrameworkConfigurationKey" "D:\E247\experimental\RSA\rsa-keypair.xml"

Result:

	Importing RSA Keys from file..
	Succeeded!


 