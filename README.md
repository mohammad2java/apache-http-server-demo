Apache http server documentation
--------------------------------

Basic Concept:
----------------
    It is web server working as front controller and its made using c language.
    its having list of modules bydefault in setup packge some of preloaded or few are commented.
    Module having list of directives which use as commond or tag element to configure web server as per customer requirement.

In one line
------------
    Apache-Http-Server = Collection of Modules
    Module = Collection of Directive
    Directive = Command or tag element that use to configure server based on requirement


you can check every module with their directive
-------------------------------------------------
https://httpd.apache.org/docs/2.4/mod/



how to download and install(windows)
-----------------------------------
    1-visit google.com and search apache http server
    2-open https://httpd.apache.org/ and click download button.
    3-click on stable realease-(2.4.39 (released 2019-04-01)) for today(17/july/2019)
    4- click for Files for Microsoft Windows 
    5-click for Apache Lounge
    6-click for vc_redist_x64 or vc_redist_x86. (pre-requisite for apache lounge)
    7-click for httpd-2.4.39-win64-VS16.zip /or httpd-2.4.39-win32-VS16.zip  (based on system-type(64bit or 32bit))
    8-install vc_redist_x64 (using next next techniq)
    9-unzip httpd-2.4.39-win64-VS16.zip this and paste into c drive.
    10-create path for bin dir
    11-open cmd(administrator) and run following command to install server as service.
    httpd.exe -k install
    12(optional-if required)- to remove apache service from windows service.
    httpd.exe -k uninstall
    13-to verify hit localhost in web browser.



How to Start/Stop Server
------------------------
    In Linux: (service name is httpd)
      service httpd start/stop

     In Windows: (service name is Apache2.4) you can use UI window to start/stop service manually
     net start/stop Apache2.4




Terms in Apache web servers
-----------------------------------

important directive/commands

    1-ServerRoot
    1.1-DocumentRoot  -define deployable directory
    2-Define
    3-Listen
    4-LoadModule  --load the module (it extends functionality interm of directive every module having their own directive) 
    5-Include     --include other conf file
    6-LogLevel    --set web server log-level
    7-ErrorLog    --defined log file
    8-CustomLog   --use to defined custom log  
    9-Require all denied  
    10-Require all denied
    11-AllowOverride None
    12-Options Indexes   ---to listing directory and sub-directories
    13-User daemon  
    14-Group daemon
    15-ServerAdmin admin@example.com   --to send mail by server incase of some issue
    16-ServerName www.example.com:80 (optional- not need to defined)




important tags/elements

    1-<IfModule moduleName>
    2-<Directory directortyPath>
    3-<Files ".ht*">
    4-<VirtualHost *:80>
         
    configuration section link:
     http://httpd.apache.org/docs/2.0/sections.html


ServerRoot
----------
    This use to define main directory of apache-server setup
    bydefault /etc/httpd for Linux and c:/Apache24 for windows
    you can change customise location based on your apache-server setup;

    Example:
        ServerRoot custom-location
        Define SRVROOT "c:/amir/Apache24"
        ServerRoot "${SRVROOT}"

    define is use to declare variable syntax- Define variableName variableValue
    ${} is use to get value of variable
        
DocumentRoot
--------------

    To define deployable/site directory
    Syntax : DocumentRoot dir_path

    default config in Linux is DocumentRoot "/var/www/html"
    default config in Windows is DocumentRoot "${SRVROOT}/htdocs"


Listen
--------
    This directive is use to make whitelist ip and/or port to access webservers
    example 
    1) Listen 12.34.56.78:80
    2) Listen 82

    Listen 82 --means this server will be accessible by port 82
    Listen 12.34.56.78:80  -means this server will be accessible by port 82 with specific ip.

How to listing dir or not listing directory
-------------------------------------------
    <Directory /var/www/html/index_allow> 
      Options Indexes
    </Directory>

    <Directory /var/www/html/index_deny> 
    </Directory>



    root path of deployable dir is -/var/www/html (in linux)
    root path of deployable dir is - "${SRVROOT}/htdocs" (in windows)
    Define SRVROOT "c:/Apache24"
    root path of deployable dir is is "c:/Apache24/htdocs" (in windows)
    
