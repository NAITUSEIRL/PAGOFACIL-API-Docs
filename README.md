# AASLabs Payment Gateway (aka APG) API Docs

How to connect your application to AASLabs Payment Gateway

## What is this gateway for

This gateway is a middleware that makes easy the connection between different applications and services that needs to accept payments with Chilean cards.


## What do I need to connect

Not much really, APG is designed to be easy to implement, does not matter the language, application or service that you use to connect. You just need to send a POST request to the APG Server including all the order information, and then you will have a reply telling that the process is finished. After that you will need to send an other request to verify the transaction information.

## How does APG server work.


### Endpoints

#### Production Endpoints

* Server : https://sv1.tbk.cristiantala.cl/tbk/v1/
* Create Transaction [POST] : https://sv1.tbk.cristiantala.cl/tbk/v1/initTransaction
* Verify Transaction [POST] : https://sv1.tbk.cristiantala.cl/tbk/v1/estadoOrden
* Transaction Details [POST] : https://sv1.tbk.cristiantala.cl/tbk/v1/getOrden

#### Development EndPoints 

* Server : https://dev-env.sv1.tbk.cristiantala.cl/tbk/v1
* Create Transaction [POST] : https://dev-env.sv1.tbk.cristiantala.cl/tbk/v1/initTransaction
* Verify Transaction [POST] : https://dev-env.sv1.tbk.cristiantala.cl/tbk/v1/estadoOrden
* Transaction Details [POST] : https://dev-env.sv1.tbk.cristiantala.cl/tbk/v1/getOrden

### Creating an order 

You create an order sending the following information as POST. You need to redirect the post also to the ENDPOINT, so we recommend using a form to POST the data.

        $pago_args = array(
            'monto'
            'order_id'
            'codigo_comercio'
            'token_service'
            'token_tienda'
        );

| Variable        | Type           | Size  | Description |
| ------------- |:-------------:| -----:| -----:|
| monto     | int | 11 | This value corresponds to the ammount of the order. Chilean currency only uses int values |
| order_id     | varchar      |   45 | This variable correspond to the Order Id of the store. It is AlphaNumeric |
| codigo_comercio | varchar     |    45| This code corresponds to the code that Transbank gaves you after hiring teh service |
| token_service | varchar      |    255 | This variable corresponds to the token that APG gives you to use teh service |
| token_tienda | varchar     |    61 | This variable is random, and is the one that makes possible to retrieve the information of the transaction later on |

### Verifying the order.

## Already Build Apps / Plugins

* Woocommerce plugin.

## Creating your own connection.



