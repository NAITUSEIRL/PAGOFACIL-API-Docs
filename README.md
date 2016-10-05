# PagoFacil Payment Gateway (aka PFPG) API Docs

How to connect your application to PagoFacil Payment Gateway

## What is this gateway for

This gateway is a middleware that makes easy the connection between different applications and services that needs to accept payments with Chilean cards.


## What do I need to connect

Not much really, PFPG is designed to be easy to implement, does not matter the language, application or service that you use to connect. You just need to send a POST request to the APG Server including all the order information, and then you will have a reply telling that the process is finished. After that you will need to send an other request to verify the transaction information.

## How does PFPG server work.


## Documentation

* [V1 English version.](DOCS/V1/EN/README.md)
* [V2 English version. (Recommended)](DOCS/V2/EN/README.md)

## Already Build Apps / Plugins

* [Woocommerce plugin for APG](https://github.com/NAITUSEIRL/tbkaas-woo-gateway "Woocommerce plugin for APG")

## Creating your own connection.

For creating your own plugin or connection you must specified the callback url where the server will reply after the completition of the payment.
