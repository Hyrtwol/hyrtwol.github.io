# Java

How to install java without the Ask toolbar shit

	Windows Registry Editor Version 5.00
	
	[HKEY_LOCAL_MACHINE\SOFTWARE\JavaSoft]
	"SPONSORS"="DISABLE"
	
	[HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\JavaSoft]
	"SPONSORS"="DISABLE"

Add this to the registry to make the installer stop asking about the "sponsor" install.
