============
webtest-plus
============

.. image:: https://badge.fury.io/py/webtest-plus.png
    :target: http://badge.fury.io/py/webtest-plus

.. image:: https://travis-ci.org/sloria/webtest-plus.png?branch=master
    :target: https://travis-ci.org/sloria/webtest-plus

An extension of `WebTest <http://webtest.pythonpaste.org/en/latest/>`_  with useful extras, including `requests <http://docs.python-requests.org/en/latest/>`_-style authentication.

Install
-------
.. code-block:: bash

    $ pip install -U webtest-plus

Usage
-----

.. code-block:: python

    import unittest
    from myapp import app
    from webtest_plus import TestApp

    class TestMyApp(unittest.TestCase):

        def setUp(self):
            self.app = TestApp(app)

        def test_protected_endpoint(self):
            response = self.app.get("/secret/", expect_errors=True)
            assert response.status_code == 401
            # Requests-style authentication
            response = self.app.get("/secret/", auth=("admin", "passw0rd"))
            assert response.status_code == 200

        def test_more_secrets(self):
            # Another way to authenticate
            self.app.authenticate(username="admin", password="passw0rd")
            assert self.app.get("/secret/").status_code == 200
            self.app.deauthenticate()
            assert self.app.get("/secret/", expect_errors=True).status_code == 401

Features
--------

* Basic HTTP authentication
* Auto-follow redirects

Requirements
------------

- Python >= 2.6 or >= 3.3

License
-------

MIT licensed. See the bundled `LICENSE <https://github.com/sloria/webtest-plus/blob/master/LICENSE>`_ file for more details.
