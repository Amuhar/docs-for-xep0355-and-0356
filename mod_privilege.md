# mod_privilege

## Introduction
This module is an implementation of [XEP-0356](http://xmpp.org/extensions/xep-0356.html). This extension allows components to have privileged access to other entity data (send messages on behalf of the server, get/set user roster, access presence information). This may be applied for external PEP service.

## Usage
If you want to grant privileged access to a component, specify it in the component configurations, e.g.:

~~~ yaml
 listen:
  ...
  -
    port: 8888
    module: ejabberd_service
    hosts:
      "sat-pubsub.example.org":
        password: "secret"
    privilege_access:
      roster: "get"
      message: "outgoing"
  ...
~~~
In the example above, the `sat-pubsub.example.org` component can get the roster of every user of the server and send messages on behalf of the server.

## Permission list
By default a component does not have any priviliged access. It is worth noting that the permissions listed below give access to data belonging to all server users.

Possible permissions:

* presence
  * managed_entity: receive server user presence.
  * roster: the component is allowed to receive the presence of both the users and the contacts in their roster.
* message
  * outgoing: the component is allowed to send messages on behalf of either the server or a bare JID of server users. 
* roster
  * get: read access to a user's roster.
  * set: write access to a user's roster.
  * both: read/write access to a user's roster.

## Security issue
Privileged access gives components access to sensitive data, so permission should be granted carefully, only if you trust a component. 

## Note 
This module is complementary to `mod_delegation` ([XEP-0355](http://xmpp.org/extensions/xep-0355.html)) but can also be used separately. 