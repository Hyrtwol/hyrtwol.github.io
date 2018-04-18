# MSBuild

![Work in progress](http://hyrtwol.dk/img/gear.gif)

* [Home](../index.md)

## Tasks

### Inline

	  <UsingTask TaskName="GetLatestGitTagVersionTask" TaskFactory="CodeTaskFactory" AssemblyFile="$(MSBuildToolsPath)\Microsoft.Build.Tasks.v4.0.dll" >
	    <ParameterGroup>
	      <Major ParameterType="System.Int32" Output="True"/>
	      <Minor ParameterType="System.Int32" Output="True"/>
	      <Build ParameterType="System.Int32" Output="True"/>
	      <Revision ParameterType="System.Int32" Output="True"/>
	      <ShortVersionStr ParameterType="System.String" Output="True"/>
	      <LongVersionStr ParameterType="System.String" Output="True"/>
	    </ParameterGroup>
	    <Task>
	      <Reference Include="mscorlib" />
	      <Using Namespace="System" />
	      <Code Type="Fragment" Language="cs">
	        <![CDATA[
	            var version = new Version(0, 0);
	            var di = new DirectoryInfo(@".git\refs\tags\");
	            foreach (var tag in di.GetFiles())
	            {
	                Version v;
	                if (Version.TryParse(tag.Name, out v))
	                {
	                    if (version < v) version = v;
	                }
	            }
	            Major = version.Major;
	            Minor = version.Minor;
	            Build = version.Build >= 0 ? version.Build : 0;
	            Revision = version.Revision >= 0 ? version.Revision : 0;
	            ShortVersionStr = version.ToString();
	            version = new Version(Major, Minor, Build, Revision);
	            LongVersionStr = version.ToString();
	        ]]>
	      </Code>
	    </Task>
	  </UsingTask>

Using it

	  <Target Name="GetTagVersion" BeforeTargets="Clean;Prepare;Build;Test">
	    <GetLatestGitTagVersionTask>
	      <Output PropertyName="MajorVersion" TaskParameter="Major" />
	      <Output PropertyName="MinorVersion" TaskParameter="Minor" />
	      <Output PropertyName="PatchVersion" TaskParameter="Build" />
	      <Output PropertyName="RevisionVersion" TaskParameter="Revision" />
	      <Output PropertyName="ShortBuildVersion" TaskParameter="ShortVersionStr" />
	      <Output PropertyName="BuildVersion" TaskParameter="LongVersionStr" />
	    </GetLatestGitTagVersionTask>
	    <Message Text="Compose Version: $(MajorVersion).$(MinorVersion).$(PatchVersion).$(RevisionVersion)" Importance="High"/>
	    <Message Text="Short Version:   $(ShortBuildVersion)" Importance="High"/>
	    <Message Text="Build Version:   $(BuildVersion)" Importance="High"/>
	  </Target>