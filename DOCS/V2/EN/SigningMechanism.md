### Signing mechanism

All requests and responses must be signed/verified using HMAC-SHA256 where:

* key is a value known to both the Gateway and you. This is the "Token Secret" field on the dashboard.
* message is a string of all key-value pairs that start with ct_ prefix, sorted alphabetically, and concatenated without separators.

## Php Signing example


    function firmarArreglo($arreglo) {
        //Ordeno Arreglo
        ksort($arreglo);
        //Concateno Arreglo
        $mensaje = $this->concatenarArreglo($arreglo);
        //Firmo Mensaje
        $mensajeFirmado = $this->firmarMensaje($mensaje, $this->ct_token_secret);
        //Guardo y retorno el mensaje firmado
        $this->ct_firma = $mensajeFirmado;
        return $mensajeFirmado;
    }
    function firmarMensaje($mensaje, $claveCifrado) {
        $mensajeFirmado = hash_hmac('sha256', $mensaje, $claveCifrado);
        return $mensajeFirmado;
    }
    public function concatenarArreglo($arreglo) {
        $resultado = "";
        foreach ($arreglo as $field => $value) {
            $resultado .= $field . $value;
        }
        return $resultado;
    }
