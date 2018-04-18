#Storage

##Hosts

	type	cname						uri									host

	iis		storage.dev.247ms.com		http://storage.dev.247ms.com/		ixv-w-d-sto-01.247ms.com
	
	nfs		storage-01.dev.247ms.com	file://storage-01.dev.247ms.com/	ix2-w-d-sto-01.247ms.com
	nfs*	storage-02.dev.247ms.com	file://storage-02.dev.247ms.com/	ix2-w-d-sto-02.247ms.com

	iis		statcon.dev.247ms.com		http://statcon.dev.247ms.com/		ixv-w-d-cdn-01.247ms.com

	iis		cds.dev.247ms.com			http://cds.dev.247ms.com/			?

\* *host does not exists*

##Apps

###Filemonger

* [TeamCity](http://teamcity.247ms.com/viewType.html?buildTypeId=bt172)
* Package: Filemonger

###Storage

* [TeamCity](http://teamcity.247ms.com/viewType.html?buildTypeId=bt200)
* [Octopus](http://octopusdeploy.247ms.com/projects/e247-shop-storage)
* Package: E247.Storage.Web or E247.Storage.SelfHost
* Role: storage
* [Demo](http://storage.dev.247ms.com/)

###Statcon

* [TeamCity](http://teamcity.247ms.com/viewType.html?buildTypeId=bt202)
* [Octopus](http://octopusdeploy.247ms.com/projects/e247-shop-statcon)
* Package: E247.Shop.Statcon.Web
* Role: statcon
* [Demo](http://statcon.dev.247ms.com/)

###Transcoder

* [TeamCity](http://teamcity.247ms.com/viewType.html?buildTypeId=bt77)
* [Octopus](http://octopusdeploy.247ms.com/projects/e247-cms-transcoder)
* Package: E247.Shop.Statcon.Web
* Role: statcon

##Todo

* Speed test
* Transcoder test