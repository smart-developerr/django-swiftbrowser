django-swiftbrowser
===================

[![Build Status](https://travis-ci.org/cschwede/django-swiftbrowser.png?branch=master)](https://travis-ci.org/cschwede/django-swiftbrowser)

Simple web app build with Django and Twitter Bootstrap to access Openstack Swift.

* No database needed
* Works with keystone, tempauth & swauth
* Support for public containers. ACL support in the works
* Minimal interface, usable on your desktop as well as on your smartphone
* Screenshots anyone? See below!

Quick Install
-------------

1) Install swiftbrowser:

    pip install django-swiftbrowser

2) Please make sure that "tempurl" and "formpost" middlewares are activated in your proxy server. Extract from /etc/swift/proxy-server.conf:

    [pipeline:main]
    pipeline = catch_errors gatekeeper healthcheck proxy-logging cache tempurl formpost tempauth proxy-logging proxy-server

    [filter:tempurl]
    use = egg:swift#tempurl

    [filter:formpost]
    use = egg:swift#formpost

3) Run development server:

    django-admin runserver --settings=swiftbrowser.settings

4) Open "http://127.0.0.1:8000/" in your browser and use 'account:username' to login (or tenant/project:username if using Keystone).


Running with Docker
-------------------

The included Dockerfile makes running django-swiftbrowser even easier. First
build the Docker container:

    docker build . -t swiftbrowser

Now run the container and point it to your Swift cluster, eg:

    docker run -d -p 8000:8000 \
        -e SECRET_KEY=CHANGE_THIS_TO_SOME_RANDOM_VALUE \
        -e SWIFT_AUTH_VERSION=1 \
        -e SWIFT_AUTH_URL=http://192.168.2.200:8080/auth/v1.0 \
        -e STORAGE_URL=http://192.168.2.200:8080/v1 \
        swiftbrowser

You can also run the tox test environment inside the container:

    docker run swiftbrowser tox

Screenshots
-----------

![Login screen](screenshots/00.png)
![Container view](screenshots/01.png)
![Object view](screenshots/02.png)
