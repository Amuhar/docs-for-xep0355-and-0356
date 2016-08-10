# mod_delegation

## Introduction
This module is an implementation of [XEP-0355](http://xmpp.org/extensions/xep-0355.html). Only admin mode has been implemented by now. Namespace delegation allows external services to handle IQ using specific namespace. This may be applied for external PEP service.

## Usage
To use the module add `mod_delegation` to `modules` section:

~~~ yaml
 modules:
   mod_delegation: {}
~~~

If you want to delegate namespaces to a component, specify them in the component configurations, e.g.:

~~~ yaml
 listen:
  ...
  -
    port: 8888
    module: ejabberd_service
    hosts:
      "sat-pubsub.example.org":
        password: "secret"
    delegations:
      "urn:xmpp:mam:1":
        filtering: ["node"]
      "http://jabber.org/protocol/pubsub":
        filtering: []
  ...
~~~
In the example above `sat-pubsub.example.org` will receive all `pubsub` requests and all `MAM` requests with the `node` filtering attribute presented in `<query/>`.

Make sure you do not delegate the same namespace to several services at the same time. As in the example above to have the `sat-pubsub.example.org` component perform correctly disable the `mod_pubsub` module.

## Options
At present only the `iqdisc` option is available.

## Security issue
Namespace delegation gives components access to sensitive data, so permission should be granted carefully, only if you trust the component. 

## Note 
This module is complementary to `mod_privilege` ([XEP-0356](http://xmpp.org/extensions/xep-0356.html)) but can also be used separately. 
