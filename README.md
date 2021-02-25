# Project2
System Hardening Mitigation

This file is to explain a mitigation technique for preventing access to an Apache Web Server.

First we start off with creating a file for blacklisted ip addresses. 

  sudo nano /etc/apache2/ipblacklist.conf

This will open a new file in the nano editor, add the following 

  Require not ip 192.168.1.90

After creating the file we need to modify apache web server config file in order to use the newly created ip blacklist file 

  cd /etc/apache2
  nano apache2.conf
  
Add the following to the apache2 config file 

  # Default to allow through all reverse proxies.
  ProxyRequests Off
  ProxyVia Off
  <Proxy *>
     Require all granted
  </Proxy>
 
  # Block ip addresses in our ipblacklist.conf file
  <Location />
     <RequireAll>
       Require all granted
        Include /etc/apache2/ipblacklist.conf
    </RequireAll>
  </Location>.
  
  # Add a 403 forbidden message for blacklisted ips
  ErrorDocument 403 "<h3>Unusual unauthorized access has been detected from this IP address.</h3><p>access has been denied.</p>"
  
Save the config file and reload the apache service

  sudo service apache2 reload
