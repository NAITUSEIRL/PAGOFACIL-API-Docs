# AASLabs Payment Gateway (aka APG) API Docs

How to connect your application to AASLabs Payment Gateway

## What is this gateway for

This gateway is a middleware that makes easy the connection between different applications and services that needs to accept payments with Chilean cards.


## What do I need to connect

Not much really, APG is designed to be easy to implement, does not matter the language, application or service that you use to connect. You just need to send a POST request to the APG Server including all the order information, and then you will have a reply telling that the process is finished. After that you will need to send an other request to verify the transaction information.

## How does APG server work.

### Creating an order 

        $pago_args = array(
            'monto'
            'order_id'
            'codigo_comercio'
            'token_service'
            'token_tienda'
        );

### verifying the order.

## Already Build Apps / Plugins

* Woocommerce plugin.

## Creating your own connection.



