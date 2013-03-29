python-iptools
==============

The [iptools][] package is a collection of utilities for dealing with IPv4
addresses.

[![Build Status][ci-status]][ci-home]

A few useful functions and objects for manipulating IPv4 addresses in python.
This was all inspired by a desire to be able to use CIDR address notation to
designate `INTERNAL_IPS` in a [Django][] project's settings file.

Functions
---------

* `validate_ip`: Validate a dotted quad IPv4 address.
* `ip2long`: Convert a dotted quad IPv4 address to a network byte order 32-bit
  integer.
* `long2ip`: Convert a network byte order 32-bit integer to a dotted quad IPv4
  address.
* `validate_cidr`: Validate a CIDR notation IPv4 address.
* `cidr2block`: Convert a CIDR notation IPv4 address into a tuple containing
  network block start and end addresses.

Objects
-------

* `IpRange`: Range of IPv4 addresses providing `in` and iteration.
* `IpRangeList`: List of IpRange objects providing `in` and iteration.

Using with Django
-----------------

The IpRangeList object can be used in a Django settings file to allow [CIDR][]
notation and/or `(start, end)` ranges to be used in the `INTERNAL_IPS` list.

There are many internal and add-on components for Django that use the
[INTERNAL_IPS][] configuration setting to alter application behavior or make
debugging easier. When you are developing and testing an application by
yourself it's easy to add the ip address that your web browser will be coming
from to this list. When you are developing in a group or testing from many ips
it can become cumbersome to add more and more ip addresses to the setting
individually.

The `iptools.IpRangeList` object can help by replacing the standard tuple of
addresses recommended by the Django docs with an intelligent object that
responds to the membership test operator in. This object can be configured
with dotted quad IP addresses like the default INTERNAL_IPS tuple (eg.
'127.0.0.1'), [CIDR][] block notation (eg. '127/8', '192.168/16') for entire
network blocks, and/or (start, end) tuples describing an arbitrary range of IP
addresses.

Django's internal checks against the INTERNAL_IPS tuple take the form `if addr
in INTERNAL_IPS` or `if addr not in INTERNAL_IPS`. This works transparently with
the IpRangeList object because it implements the magic method `__contains__`
which python calls when the `in` or `not in` operators are used.

### Example: ###

    #!/usr/bin/env python
    import iptools

    INTERNAL_IPS = iptools.IpRangeList(
        '127.0.0.1',                # single ip
        '192.168/16',               # CIDR network block
        ('10.0.0.1', '10.0.0.19'),  # arbitrary inclusive range
    )

Additional Documentations
-------------------------

Pydoc documantation available at [Read the Docs][].

Local documentation can be built using [Sphinx][]:

    cd docs
    make html

Python Version Compatibility
----------------------------

[Travis CI][ci-home] automatically runs tests against python 2.5, 2.6, 2.7, 3.2, 3.3 and pypy on 32-bit Ubuntu Linux 11.10.

Installing
----------

Install the latest stable version using pip:

    pip install iptools

or easy_install:

    easy_install iptools

Install the latest development version:

    git clone https://github.com/bd808/python-iptools.git
    cd python-iptools
    python setup.py install

---
[iptools]: http://pypi.python.org/pypi/iptools
[ci-status]: https://secure.travis-ci.org/bd808/python-iptools.png
[ci-home]: http://travis-ci.org/bd808/python-iptools
[CIDR]: http://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[Django]: http://www.djangoproject.com/
[INTERNAL_IPS]: http://docs.djangoproject.com/en/dev/ref/settings/#internal-ips
[Read the Docs]: http://python-iptools.readthedocs.org/
[Sphinx]: http://sphinx.pocoo.org/
