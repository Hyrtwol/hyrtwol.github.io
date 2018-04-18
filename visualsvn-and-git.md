# VisualSVN Server and Git

## Links

* [VisualSVN][1]
* [msysgit][2]
* [SubGit][3]
* [VisualSVN Subversion Server and Git][4]

[1]:http://www.visualsvn.com/visualsvn/download/
[2]:http://msysgit.github.io/
[3]:http://subgit.com/documentation/index.html
[4]:http://blog.subgit.com/visualsvn-subversion-server-and-git/

## Setup

### httpd.config

append the following line to httpd.config

	Include conf/httpd-git.conf

### httpd-git.config

reuse the authentication from VisualSVN for git

	LoadModule cgi_module bin/mod_cgi.so
	LoadModule authz_user_module bin/mod_authz_user.so
	
	SetEnvIf Request_URI "^/git/.*$" GIT_PROJECT_ROOT=D:/Repositories
	SetEnvIf Request_URI "^/git/.*$" GIT_HTTP_EXPORT_ALL
	
	ScriptAlias /git/ "C:/Program Files (x86)/Git/libexec/git-core/git-http-backend.exe/"
	
	<Location /git>
	  Options +ExecCGI
	
	  Require valid-user
	  AuthName "VisualSVN Server"
	  
	  #AuthType Basic
	  #AuthBasicProvider file
	  #AuthUserFile "D:/Repositories/htpasswd"
	  
	  SVNParentPath "D:/Repositories/"
	  SVNPathAuthz short_circuit
	  
	  AuthType VisualSVN
	  AuthzVisualSVNAccessFile "D:/Repositories/authz-windows"
	  AuthnVisualSVNBasic on
	  AuthnVisualSVNIntegrated off
	  AuthnVisualSVNUPN Off
	  
	</Location>

