# Signing mechanism

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
## Ruby Signing Example
      def sign(fields, key=@key)
        Digest::HMAC.hexdigest(fields.sort.join, key, Digest::SHA256)
      end
## Javascript Signing Example
    const crypto = require('crypto');
    // Receives a map with the params
    module.exports = function(params){
        // Sorts, concatenates and hashes the parameters
        var message = Object.keys(params)
            .map(k=>[k, params[k]])
            .filter(p=>p[0].indexOf('ct_') === 0 && p[0] !== 'ct_firma')
            .map(p=>p.join(''))
            .sort()
            .join('');

        var hmac = crypto.createHmac('sha256', config.pagoFacilSecret);
        hmac.setEncoding('hex');
        hmac.write(message);
        hmac.end();
        return hmac.read();
    };
