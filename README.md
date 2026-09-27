# Nginx Config Generator
Virtualmin hook to set Nginx config when creating/editing/deleting a virtualserver.
Useful if you want/need to make both servers, Apache & Nginx co-exist on the same server with Virtualmin.
-   Nginx will use the front facing ports (f.i. 80/443, it will use the "Port for use in HTTP/HTTPS URLs") - auto-detected
-   Apache is expected to sit on the web ports (f.i. 8080/8443)  - auto-detected 

## What is new in this fork:
- Adapted and tested to work on Virtualmin version 8.2.0 GPL & Debian v12
- Major refactoring and logging option in hook
- It ensures that the Nginx log directory exists
- Detection whether the force SSL-Redirect is enabled and apply it to nginx config files to (if you want to change this setting afterwards and apply it to nginx, change something in "Edit Virtual Server"-Page. For instance the Description-field)
- http-01 ssl certification challenge is still working even if the force SSL-Redirect is enabled
- If the auto-generated nginx config would cause nginx to stop working, it will not try to reload/restart nginx, thu savoiding down-time

## Requirements
It only work with virtualservers created after installed a version of webmin-virtual-server >= 6.01.gpl-3.

## Install instructions
As root run:
```
cp virtualmin-nginx-hook /usr/local/bin/virtualmin-nginx-hook
mkdir /usr/local/etc/nginx-templates
cp -r nginx-templates/* /usr/local/etc/nginx-templates/
mkdir /usr/local/etc/nginx-logrotate-templates
cp -r nginx-logrotate-templates/* /usr/local/etc/nginx-logrotate-templates/
```

Verify that `NGINX_SITES_AVAILABLE_DIRECTORY` and `NGINX_SITES_ENABLED_DIRECTORY` directories exists.

Verify also that `NGINX_LOGS_FOLDER` exists.

Now login to virtualmin with a user with root privileges and:

1. Go to System Settings -> Virtualmin Configuration.
2. Select the Actions upon server and user creation category.
3. In the Command to run after making changes to a server field, enter `/usr/local/bin/virtualmin-nginx-hook`.
4. Click Save.

## Specify custom nginx and nginx-logrotate templates
You can specify a custom nginx and nginx-logrotate template for a virtualserver.

You only have to set the description of the virtualserver using this patterns:
```
[nginx-template custom-template-name] [nginx-logrotate-template custom-logrotate-template]
```

With previous example (if default script config is not modified), the following files must exists:
```
/usr/local/etc/nginx-templates/custom-template-name/plantilla-nginx-ssl.conf
/usr/local/etc/nginx-templates/custom-template-name/plantilla-nginx.conf
/usr/local/etc/nginx-logrotate-templates/custom-logrotate-template/logrotate-nginx.conf
```

If custom template is not found, default template will be used instead.
