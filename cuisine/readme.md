# Cuisine

*Version $version$*

[Source at Bitbucket](https://bitbucket.org/Hyrtwol/cuisine)

## How to build

Run the following line with msbuild:

	msbuild Build.recipe

## Todo

* Rename Cuisine.Tasks.MSDeploy to Cuisine.Tasks.Web
* Rename Cuisine app to Cuisine.Chef or just Chef
* Create Cuisine.WebUtils

## Notes

Some names for the future...

### [Chef](http://en.wikipedia.org/wiki/Chef)
 
*le chef de cuisine*

Clickonce app for setup and updates

### [Cashier](http://en.wikipedia.org/wiki/Cashier)

### [Bakery](http://en.wikipedia.org/wiki/Cashier)

### [Catering](https://en.wikipedia.org/wiki/Catering)

### [Buffet](https://en.wikipedia.org/wiki/Buffet)

### [Toaster](http://en.wikipedia.org/wiki/Toaster)

Build service

![toster](http://www.turnpixel.com/wp-content/uploads/2010/09/icon.toaster.jpg)

## Pantry

	Pantry
	  Artifacts
	    Artifact GetArtifact(name)
	    files PutArtifact(Artifact) { copy }
	  Tasks
	    files PutArtifact(Artifact) { unpack }
	  Tools
	    files PutArtifact(Artifact) { unpack }
	
	Depot
	  Artifacts
	    Artifact GetArtifact(name)
	    files PutArtifact(Artifact) { copy }

## Links

* [Semantic Versioning](http://semver.org/)
* [NuGet Versioning](http://docs.nuget.org/docs/reference/versioning)
* [Json Content Type](http://stackoverflow.com/questions/477816/the-right-json-content-type)

## Cooking

### Properties

	  <PropertyGroup>
	    <ArtifactName> name of the artifact </ArtifactName>
	    <MajorVersion>1</MajorVersion>
	    <MinorVersion>0</MinorVersion>
	    <PatchVersion>0</PatchVersion>
	    <ArtifactProjectUrl> url to a project site </ArtifactProjectUrl>
	    <ArtifactIconUrl> url to an icon e.g. used in nuspec's </ArtifactIconUrl>
	    <ArtifactLicenseUrl> licence url to an icon e.g. used in nuspec's </ArtifactLicenseUrl>
	    <ArtifactCompany> company name e.g. used in assembly info's </ArtifactCompany>
	    <ArtifactCopyright> copyright e.g. used in assembly info's </ArtifactCopyright>
	    <ArtifactDescription> default artifact description </ArtifactDescription>
	    <ArtifactSolution>$(ArtifactName)</ArtifactSolution>
	  </PropertyGroup>

### Items

	  <ItemGroup>
	    <Project Include="DummyLibrary" />
	    <Project Include="DummyUnitTest">
	      <Flavour>Test</Flavour>
	    </Project>
	    <Project Include="DummyWebApp">
	      <Flavour>Web</Flavour>
	    </Project>
	    <Project Include=" artifact name ">
	      <Flavour>NuGetTool</Flavour>
	      <Version> overwrite version </Version>
	      <Description> overwrite description </Description>
	      <Tags> overwrite tags </Tags>
	      <Poke> true or false - if true the nuspec will be updated </Poke>
	      <RootPath> if the project path is diffrent from the project name specify the sub path </RootPath>
	    </Project>
	    <Project Include="Cuisine.Tasks.Q3Map">
	      <Flavour>NuGetTask</Flavour>
	    </Project>
	  </ItemGroup>

#### Flavours

Flavour   | Output
----------| -------------
Library   | zip (default)
WinExe    | zip
Exe       | zip
Content   | zip
WiX       | msi
ClickOnce | zip
Web       | msdeploy package
NuGet     | nuget package (with nuspec)
NuGetLib  | nuget package with a library (no nuspec)
NuGetTool | nuget package with a tool (with nuspec, no check)
NuGetTask | nuget package with a msbuild task (with nuspec, no check)

## Other build tasks

### Cuisine.NASM
Build assembly code
### Cuisine.Q3Map2
Build Quake3 and Wolfenstein maps
### Cuisine.ILMerge
Merge assemblies
### Cuisine.TeamCity
TeamCity automation
### Cuisine.Database
Oracle, MySql
### Cuisine.Octopus
Octopus deploy automation
### Cuisine.Media
wave, mp3, flac tools
### Cuisine.VCS
Git, Subversion
### Cuisine.Web
Ftp, Http, MSDeploy
### Cuisine.Markdown
Generate html from markdown files
### Cuisine.DirectX
Call DirectX tools
### Cuisine.FxCop
Run FxCop
### Cuisine.Report
Generate build reports
### Cuisine.Sandcastle
Generate help files
### Cuisine.Format
Txt, Xml, Json
