.. image:: https://img.shields.io/badge/License-MIT-green.svg
    :alt: ip-netblocks-py license
    :target: https://opensource.org/licenses/MIT

.. image:: https://img.shields.io/pypi/v/ip-netblocks.svg
    :alt: ip-netblocks-py release
    :target: https://pypi.org/project/ip-netblocks

.. image:: https://github.com/whois-api-llc/ip-netblocks-py/workflows/Build/badge.svg
    :alt: ip-netblocks-py build
    :target: https://github.com/whois-api-llc/ip-netblocks-py/actions

========
Overview
========

The client library for
`IP Netblocks API <https://ip-netblocks.whoisxmlapi.com/>`_
in Python language.

The minimum Python version is 3.6.

Installation
============

.. code-block:: shell

    pip install ip-netblocks

Examples
========

Full API documentation available `here <https://ip-netblocks.whoisxmlapi.com/api/documentation/making-requests>`_

Create a new client
-------------------

.. code-block:: python

    from ipnetblocks import *

    client = Client('Your API key')

Make basic requests
-------------------

.. code-block:: python

    # Get netblocks for a given IPv4 or IPv6 address.
    response = client.get('8.8.8.8')
    print(response)

    # Get netblocks by AS number
    response = client.get_by_asn(15169)
    # or
    client.get(asn=15169)

    # Find IP Netblocks which have the specified search terms in their
    # Netblock (netname, description, remarks),
    # or Organisation (org.org, org.name, org.email, org.address) fields.
    response = client.get_by_org(['google', 'cloud'])
    # or
    client.get(org='google')

    # Get raw API response in XML format
    raw_result = client.get_raw('2.2.2.2',
        output_format=Client.XML_FORMAT)

Advanced usage
-------------------

Extra request parameters

.. code-block:: python

    result = client.get(
        '1.1.1.1',
        mask=24,
        limit=10)

Response model overview
-----------------------

.. code-block:: python

    Response:
        - search: [str]
        - count: int
        - limit: int
        - inetnums: [Inetnum]
            - inetnum: str
            - inetnum_first: str
            - inetnum_last: str
            - parent: str
            - AS: AutonomousSystem
                - asn: int
                - name: str
                - type: str
                - route: str
                - domain: str
            - netname: str
            - nethandle: str
            - description: [str]
            - modified: datetime
            - country: str
            - city: str
            - address: [str]
            - org: Org
                - org: str
                - name: str
                - email: str
                - phone: str
                - country: str
                - city: str
                - postal_code: str
                - address: [str]
            - abuse_contact: [Contact]
                - id: str
                - person: str
                - role: str
                - email: str
                - phone: str
                - country: str
                - city: str
                - address: [str]
            - admin_contact: [Contact]
            - tech_contact: [Contact]
            - mnt_by: [Maintainer]
                - mntner: str
                - email: str
            - mnt_domains: [Maintainer]
            - mnt_lower: [Maintainer]
            - mnt_routes: [Maintainer]
            - remarks: [str]
            - source: str


Sample response
---------------

.. code-block:: python

  { 'search': '8.8.8.8', 'count': '7', 'limit': '100',
    'inetnums': [
        {'inetnum': '8.8.8.0 - 8.8.8.255',
         'inetnum_first': 281470816487424,
         'inetnum_last': 281470816487679,
         'parent': '',
         'AS': {'asn': 15169, 
                'name': 'Google LLC',
                'type': 'Content',
                'route': '8.8.8.0/24',
                'domain': 'https://about.google/intl/en/'}",
         'netname': 'LVLT-GOGL-8-8-8', 'nethandle': 'NET-8-8-8-0-1',
         'description': [],
         'modified': '2014-03-14 00:00:00',
         'country': 'US',
         'city': 'Mountain View',
         'address': ['1600 Amphitheatre Parkway'],
         'abuse_contact': [], 'admin_contact': [], 'tech_contact': [],
         'org': {'org': 'GOGL',
                 'name': 'Google LLC',
                 'email': 'arin-contact@google.com\\nnetwork-abuse@google.com',
                 'phone': '+1-650-253-0000',
                 'country': 'US',
                 'city': 'Mountain View',
                 'postal_code': '94043',
                 'address': ['1600 Amphitheatre Parkway']},
         'mnt_by': [], 'mnt_domains': [], 'mnt_lower': [], 'mnt_routes': [],
         'remarks': [],
         'source': 'ARIN'}
        {'inetnum': '8.0.0.0 - 8.15.255.255',
         'inetnum_first': 281470815961088,
         'inetnum_last': 281470817009663,
         'parent': '8.0.0.0 - 8.127.255.255',
         'AS': {'asn': 3356,
                'name': 'Lumen AS 3356',
                'type': 'NSP',
                'route': '8.0.0.0/12',
                'domain': 'http://www.lumen.com'}",
         'netname': 'LVLT-ORG-8-8', 'nethandle': 'NET-8-0-0-0-1',
         'description': [],
         'modified': '2018-04-23 00:00:00',
         'country': 'US',
         'city': 'Monroe',
         'address': ['100 CenturyLink Drive'],
         'abuse_contact': [], 'admin_contact': [], 'tech_contact': [],
         'org': {'org': 'LPL-141',
                 'name': 'Level 3 Parent, LLC',
                 'email': 'abuse@level3.com nipaddressing@level3.com',
                 'phone': '+1-877-453-8353',
                 'country': 'US',
                 'city': 'Monroe',
                 'postal_code': '71203',
                 'address': ['100 CenturyLink Drive']},
         'mnt_by': [], 'mnt_domains': [], 'mnt_lower': [], 'mnt_routes': [],
         'remarks': [],
         'source': 'ARIN'}
        ...
    ]
  }


