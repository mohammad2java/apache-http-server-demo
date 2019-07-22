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


How to Start/Stop Server and compile the conf file
------------------------------------------------------
    In Linux: (service name is httpd)
      service httpd start/stop

     In Windows: (service name is Apache2.4) you can use UI window to start/stop service manually
     net start/stop Apache2.4

    for compile
    httpd -t
    
    for showing vhost configuration
    httpd -S
    
    for restart appache server
    httpd -k restart


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
    





VirtualHost directive
---------------------
    This is very important to make multiple hostserver inside single web-server.
    This is very good to hide application server using reverse proxy rule
    
setup:
----------    
    1) in windows default file for virtualhost setup is 
    C:\Apache24\conf\extra\httpd-vhosts.conf

    2) include vhost file in main conf file.(main conf file is httpd.conf)
    Include conf/extra/httpd-vhosts.conf
    
    3)load follwing module
    LoadModule proxy_module modules/mod_proxy.so
    LoadModule proxy_http_module modules/mod_proxy_http.so


VirtualHost using same webserver with seperate directory
-----------------------------------------------------------
    as you know htdocs is default root directtory for localhost with port:80

    <VirtualHost dummy-host.example.com:80>
        ServerAdmin webmaster@dummy-host.example.com
        DocumentRoot "${SRVROOT}/htdocs/host1"
        ServerName dummy-host.example.com
        ErrorLog "logs/dummy-host.example.com-error.log"
        CustomLog "logs/dummy-host2.example.com-access.log" common
    </VirtualHost>

    syntax : <VirtualHost vadrress:vport>
    <VirtualHost *:*> * -accept for all(all ips and port) ,if not all you can put specific one like 
    <VirtualHost *:80>  -accept all ip with port 80  
    <VirtualHost dummy-host.example.com:80>  --accept only dummy-host.example.com address with port 80
    
flow step:
--------
    when client hit any address in address bar then browser local hosts file to find associated ip (if address entry is not present in hosts file then client looks dns provider and ask for server machine ip)
    when get ip it will send request to that machine -machine server (apache-web-server) accept the request and match in conf file based on following orders.

    1- match vaddress and port
    2- match servername
    3- if found more than one matches then first one will be final matches.
    
    

VirtualHost using external app server (reverse proxy)
-----------------------------------------------------
    <VirtualHost shlocal:80>
    ProxyPreserveHost On
    ProxyPass / http://localhost:8088/
    ProxyPassReverse / http://localhost:8088/
    ServerName shlocal
    </VirtualHost>


    ProxyPreserveHost On --means vaddress will be preserved instead of Actual address
    ProxyPass  --vAddress will be act as proxy for actual address
    ProxyPassReverse --vAddress will be act as proxy for actual address 
    ServerName shlocal   --sencond matching for vaddress.


RewriteRule , RewrireEngine and RewriteCond
---------------------------------------------
    This is completly directory scope concept to match url pattern(RegularExpression to call something else url).
    This can be use in 2 ways.
    1) using .htaccess file  (inside the directory which you want to associate with RewriteEngine)
    2) using httpd.conf file 

    if you want to apply RewriteEngine as root level means (in htdocs directory)
    open httpd.conf and written below code inside the htdocs dir.
    
    1)enable the Rewrite_module
    LoadModule rewrite_module modules/mod_rewrite.so
    
    2) add following code inside htdocs dir
    <Directory "${SRVROOT}/htdocs">
        AllowOverride All
        Require all granted
        Options indexes FollowSymLinks SymLinksIfOwnerMatch 
        RewriteEngine On                               
        RewriteCond %{REQUEST_FILENAME} !-f       --means  if file exist dont apply RewriteRule
        RewriteCond %{REQUEST_FILENAME} !-d        --means  if directory exist dont apply RewriteRule
        RewriteRule "^(.*)$"  "/myproj/index2.html"    --syntax :  RewriteRule "url-pattern(regularexpress)"  "target-url"
    </Directory>

    if you want to use .htaccess file ..you can move Rewrite related code into .htaccess file and file must be inside htdocs(if you want to apply at root level) else in which you want to assciate)
    
    .htaccess---sample
    #RewriteEngine On
    #RewriteCond %{REQUEST_FILENAME} !-f
    #RewriteCond %{REQUEST_FILENAME} !-d
    #RewriteRule "^(.*)$"  "/myproj/index2.html"


Redirect in Apache server
--------------------------
    # Redirect also is similer to RewriteRule and also can write inside .htaccess file
    #syntax: redirect [status-code] "pattern" "target"
    Redirect 301 /gituser https://github.com/mohammad2java/apache-http-server-demo
    #Syntax --RedirectMatch regular-express target-url
    RedirectMatch 301 /perl/.* https://alvinalexander.com/web/apache-redirectmatch-examples-wildcard-301



