# UnixServerConfiguration
Udacity Project

##
This project's purpose was to completely setup and secure a linux-based server on the internet. Basically, I went through Firewall configurations and used SSH keys to make sure this server will be acessed only by authorized personnel. Also, I left one of my projects running on http, which you can check it out by clicking [http://adautovjr.codes](here).

## Important info

* IP: 167.71.176.162
* SSH Port: 2200
* URL: [http://adautovjr.codes](http://adautovjr.codes)

## Tools used

1. [https://wiki.ubuntu.com/UncomplicatedFirewall?action=show&redirect=UbuntuFirewall](Uncomplicated Firewall) was used to block all incoming requests. Then allowed specific ports to be opened.
2. For this server setup I Used [https://www.nginx.com/](Nginx) to handle requests.
3. To handle the python code I used [https://gunicorn.org/](GUnicorn).
4. In order to keep the app running at all times, [http://supervisord.org/](Supervisor) was setup.
5. The app running on http uses [https://github.com/pallets/flask](Flask), [https://www.sqlalchemy.org/](SQLAlchemy) and [https://auth0.com](auth0).
