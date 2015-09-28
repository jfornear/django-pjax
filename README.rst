django-pjax |Build Status| |Version|
====================================

An improvement of [Django-PJAX]: The Django helper for jQuery-PJAX.

What’s PJAX?
------------

PJAX is essentially [AHAH] (“Asynchronous HTML and HTTP”), except with
real permalinks and a working back button. It lets you load just a
portion of a page (so things are faster) while still maintaining the
usability of real links.

A demo makes more sense, so [check out the one defunkt put together].

Credits
-------

This project is an extension of [Django-PJAX] and all credits from the
original version goes to [Jacob Kaplan-Moss].

About
-----

This project keeps the original structure, but add new features to it,
and aims to keep django-pjax updated. Some goals are to keep this
project working with Python 2.7+ and 3.3+ and also Django 1.5+.

Feel free to submit a PR and contribute to this project.

Compatibility
-------------

-  Python 2.6+ or 3.2+
-  PyPy or PyPy3
-  CPython
-  Django 1.3+

Not all Django versions works with Python, PyPy or CPython. See the
Django docs to know more about supported versions.

Install
-------

Just run:

``pip install django-pjax``

Usage
-----

First, read about [how to use jQuery-PJAX][defunkt’s jquery-pjax] and
pick one of the techniques there.

Next, make sure the views you’re PJAXing are using [TemplateResponse].
You can’t use Django-PJAX with a normal ``HttpResponse``, only
``TemplateResponse``.

PJAX decorator
~~~~~~~~~~~~~~

The pjax decorator:

.. code:: python

    pjax(pjax_template=None, additional_templates=None, follow_redirects=False)

``pjax_template`` (str): default template.

``additional_templates`` (dict): additional templates for multiple
containers.

``follow_redirects`` (bool): if True, all django redirects will force a
page reload, instead of placing the content in the pjax context.

Decorate these views with the pjax decorator:

.. code:: python

    from djpjax import pjax

    @pjax()
    def my_view(request):
        return TemplateResponse(request, "template.html", {'my': 'context'})

After doing this, if the request is made via jQuery-PJAX, the
``@pjax()`` decorator will automatically swap out ``template.html`` for
``template-pjax.html``.

More formally: if the request is a PJAX request, the template used in
your ``TemplateResponse`` will be replaced with one with ``-pjax``
before the file extension. So ``template.html`` becomes
``template-pjax.html``, ``my.template.xml`` becomes
``my.template-pjax.xml``, etc. If there’s no file extension, the
template name will just be suffixed with ``-pjax``.

You can also manually pick a PJAX template by passing it as an argument
to the decorator:

.. code:: python

    from djpjax import pjax

    @pjax("pjax.html")
    def my_view(request):
        return TemplateResponse(request, "template.html", {'my': 'context'})

You can also pick a PJAX template for a PJAX container and use multiple
decorators to define the template for multiple cont

.. |Build Status| image:: https://travis-ci.org/eventials/django-pjax.svg?branch=master
   :target: https://travis-ci.org/eventials/django-pjax
.. |Version| image:: https://img.shields.io/pypi/v/django-pjax.svg
   :target: https://pypi.python.org/pypi/django-pjax