# AASLabs Payment Gateway (aka APG) API Docs

How to connect your application to AASLabs Payment Gateway

## What is this gateway for

This gateway is a middleware that makes easy the connection between different applications and services that needs to accept payments with Chilean cards.


## What do I need to connect

Not much really, APG is designed to be easy to implement, does not matter the language, application or service that you use to connect. You just need to send a POST request to the APG Server including all the order information, and then you will have a reply telling that the process is finished. After that you will need to send an other request to verify the transaction information.

## How does APG server work.


### Endpoints

#### Production Endpoints

* Server : https://sv1.tbk.cristiantala.cl/tbk/v2/
* Create Transaction [POST] : https://sv1.tbk.cristiantala.cl/tbk/v2/initTransaction

#### Development EndPoints

* Server : https://dev-env.sv1.tbk.cristiantala.cl/tbk/v2
* Create Transaction [POST] : https://dev-env.sv1.tbk.cristiantala.cl/tbk/v1/initTransaction


### Creating an order

You create an order sending the following information as POST. You need to redirect the post also to the ENDPOINT, so we recommend using a form to POST the data.

EndPoint : https://sv1.tbk.cristiantala.cl/tbk/v2/initTransaction
        $pago_args = array(
            'ct_monto'
            'ct_order_id'
            'ct_email'
            'ct_token_service'
            'ct_token_tienda'
            'ct_firma'
        );

| Variable        | Type           | Size  | Description |
| ------------- |:-------------:| -----:| -----:|
| ct_monto     | int | 11 | This value corresponds to the ammount of the order. Chilean currency only uses int values |
| ct_order_id     | varchar      |   45 | This variable correspond to the Order Id of the store. It is AlphaNumeric |
| ct_codigo_comercio | varchar     |    45| This code corresponds to the code that Transbank gaves you after hiring teh service |
| ct_token_service | varchar      |    255 | This variable corresponds to the token that APG gives you to use teh service |
|ct_ token_tienda | varchar     |    61 | This variable is random, and is the one that makes possible to retrieve the information of the transaction later on. It must be keep on the server to make teh query later |

If all the information was sent correctly and you have permissions to create a transaction you will see the following screen.

![alt text](img/INITTRANSACTION.png "AASLabs INITTRANSACTION")

After this point the server will take care of the payment and it will reply after the completition of it. This could be successful or a failure.

The reply consist only on the order_id .

    $variablesRetornoFijas = array(
        "order_id" => $order_id,
    );

Where order_id corresponds to the "Your" order identification ( Store's order id ).


### Verifying the order.

If you notice the reply of the server does not include the status of the order. This is done to increse the security of the transaction and eliminate the possibility of having a man in the middle attack. The server will always have a valid SSL certificate.

To ask for the status of the order, we use the order_id returned from the server to check if the order exists, if it exists we will ask the server the status of the order through posting the following values.

EndPoint : https://dev-env.sv1.tbk.cristiantala.cl/tbk/v1/estadoOrden

    $fields = array(
        'codigo_comercio',
        'token_service',
        'order_id',
        'monto',
        'token_tienda',
    );


  All the fields have the same meaning that in the previus section. You must remember that the "token_tienda" that was generated randomly should be the same that we use before. If one of the following fields don't match to the information that the server has, the answer will be null.

If the reply of  is "COMPLETADA" the order is successfully paid, and we can continue. For example :

    {
      "ESTADO":"COMPLETADA"
    }

If the reply of the server is any other the order is not yet paid. For example :

    {
      "ESTADO":null
    }




To get the details of the order that is already paid ( "COMPLETADA" ), we will ask for the information with the same variables but to a different EndPoint.

EndPoint : https://dev-env.sv1.tbk.cristiantala.cl/tbk/v1/getOrden

    $fields = array(
        'codigo_comercio'
        'token_service'
        'order_id'
        'monto'
        'token_tienda'
    );



#### Successful Reply
On a successful reply the answer will be like tho following :

    {
    "detalles_transaccion": {
      "order_id": 63,
      "authorization_code": "1213",
      "payment_type_code": "VN",
      "amount": "400.00",
      "card_number": "6623",
      "card_expiration_date": null,
      "shares_number": 0,
      "accounting_date": "0616",
      "transaction_date": "2016-06-16"
      }
    }



| Variable        | Type           | Size  | Description |
| ------------- |:-------------:| -----:| -----:|
| order_id     | varchar | 45 | Order identification of the store. |
| authorization_code     | varchar      |   255 | This is the authorization code that Transbank replies of a successfull order. |
| payment_type_code | varchar     |    4| corresponds to the type of payment received |
| amount | decimal      |    10,2 | corresponds to the amount of the order in decimal  |
| card_number | varchar     |    16 |  Corresponds to the number on the card. Generally will be only the last 4 numbers on teh card |
| card_expiration_date | varchar     |    4 | date that the card expires. generally null unless you have more privileges |
| shares_number | int     |    2 | The quantity of shares for the payment |
| accounting_date | varchar     |    4 | Accounting Date |
| transaction_date | varchar     |    10 | Transaction Date |


#### Null reply

    {
      "detalles_transaccion":null
    }


## Already Build Apps / Plugins

* [Woocommerce plugin for APG](https://github.com/NAITUSEIRL/tbkaas-woo-gateway "Woocommerce plugin for APG")

## Creating your own connection.

For creating your own plugin or connection you must specified the callback url where the server will reply after the completition of the payment.
