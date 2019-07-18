Apache http server documentation
--------------------------------

how to download and install(windows)
-----------------------------------
    1-visit google.com and search apache http server
    2- open https://httpd.apache.org/ and click download button.
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
