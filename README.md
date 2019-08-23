# UnixServerConfiguration
Udacity Project

##
This project's purpose was to completely setup and secure a linux-based server on the internet. Basically, I went through Firewall configurations and used SSH keys to make sure this server will be acessed only by authorized personnel. Also, I left one of my projects running on http, which you can check it out by clicking [here](http://adautovjr.codes).

## Important info

* IP: 167.71.176.162
* SSH Port: 2200
* URL: [http://adautovjr.codes](http://adautovjr.codes)

## Tools used

1. [Uncomplicated Firewall](https://wiki.ubuntu.com/UncomplicatedFirewall?action=show&redirect=UbuntuFirewall) was used to block all incoming requests. Then allowed specific ports to be opened.
2. For this server setup I Used [Nginx](https://www.nginx.com/) to handle requests.
3. To handle the python code I used [GUnicorn](https://gunicorn.org/).
4. In order to keep the app running at all times, [Supervisor](http://supervisord.org/) was setup.
5. The app running on http uses [Flask](https://github.com/pallets/flask), [SQLAlchemy](https://www.sqlalchemy.org/) and [auth0](https://auth0.com).
