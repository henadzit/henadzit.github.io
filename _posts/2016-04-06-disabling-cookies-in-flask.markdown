---
layout: post
title:  "Disabling Cookies in Flask"
date:   2016-04-06 01:02:03
tags:
    - python
    - flask
---

A Flask's session is stored as an encrypted string in cookies. If _Flask.session_ is changed, the server adds a header to a response like

  Set-Cookie:session=eyJrZXkiOnsiIGIiOiJkbUZzZFdVPSJ9fQ.Ceb-lQ.oy7fSZf9sPcDli8mGHAisRn15Dc; HttpOnly; Path=/

The session cookie contains all session information. That's great and works
for a web application. But you don't want to have cookies if you create a REST service.

While developing a Flask REST service, at some point I learnt that my service
responded with a session _Set-Cookie_ header despite the fact that I didn't use session.
If it wasn't me then it was one of the libraries I was using. At that moment it didn't hit me that
I can look through the content of session and figure out what data gets stored and then what libraries do it. But I decided that I want to disable session persistence completely.

There is no such option in Flask like disabling cookie completely. If something is
stored in the session object, the changes are stored in cookies. But Flask provides
a way to override the session interface.

{% highlight python %}
from flask import Flask, session, request
from flask.sessions import SessionMixin, SessionInterface


class WeakSession(dict, SessionMixin):
    pass


class WeakSessionInterface(SessionInterface):
    def open_session(self, app, request):
        return WeakSession()

    def save_session(self, app, session, response):
        pass


class NoSessionsFlask(Flask):
    session_interface = WeakSessionInterface()


app = NoSessionsFlask(__name__)
app.secret_key = 'SomeRandomKey'


@app.route('/')
def hello_world():
    resp = 'session: {}, \ncookies: {}\n'.format(session, request.cookies)
    # this won't persist till next request
    session['key'] = 'value'
    return resp

if __name__ == '__main__':
    app.run()
{% endhighlight %}

The code above makes the app's session behave as the request scope.

But it smells like a hack. The proper solution was to go through the session
object and see what was stored there. The session had two entries
_'identity.id'_ and _'identity.auth_type'_. After little searching I found that
these were originated in [Flask-Principal](http://127.0.0.1:4000/) and that
it can be disabled with a configuration option.

{% highlight python %}
principal = Principal(app, use_sessions=False)
{% endhighlight %}
