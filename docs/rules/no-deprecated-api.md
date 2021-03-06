# Disallow deprecated API (no-deprecated-api)

Node has many deprecated API.
The community is going to remove those API from Node in future, so we should not use those.

## Rule Details

The following patterns are considered problems:

```js
/*eslint no-deprecated-api: 2*/

var fs = require("fs");
fs.exists("./foo.js", function() {}); /*ERROR: 'fs.exists' was deprecated since v4. Use 'fs.stat()' or 'fs.access()' instead.*/

// Also, it can report the following patterns.
var exists = require("fs").exists; /*ERROR: 'fs.exists' was deprecated since v4. Use 'fs.stat()' or 'fs.access()' instead.*/
const {exists} = require("fs"); /*ERROR: 'fs.exists' was deprecated since v4. Use 'fs.stat()' or 'fs.access()' instead.*/


// And other deprecated API below.
```

This rule reports the following deprecated API.

- buffer
    - [Buffer constructors](https://nodejs.org/dist/v6.0.0/docs/api/buffer.html#buffer_class_buffer)
    - [SlowBuffer class](https://nodejs.org/dist/v6.0.0/docs/api/buffer.html#buffer_class_slowbuffer)
- crypto
    - [createCredentials](https://nodejs.org/dist/v0.12.0/docs/api/crypto.html#crypto_crypto_createcredentials_details)
- [domain](https://nodejs.org/dist/v4.0.0/docs/api/domain.html#domain_domain)
- events
    - [EventEmitter.listenerCount](https://nodejs.org/dist/v4.0.0/docs/api/events.html#events_class_method_eventemitter_listenercount_emitter_event)
- fs
    - [exists](https://nodejs.org/dist/v4.0.0/docs/api/fs.html#fs_fs_exists_path_callback)
    - [existsSync](https://nodejs.org/dist/v4.0.0/docs/api/fs.html#fs_fs_existssync_path)
- globals
    - [require.extensions](https://nodejs.org/dist/v0.12.0/docs/api/globals.html#globals_require_extensions)
- http
    - [createClient](https://nodejs.org/dist/v0.10.0/docs/api/http.html#http_http_createclient_port_host)
- REPL
    - [process.env.NODE_REPL_HISTORY_FILE](https://nodejs.org/dist/v4.0.0/docs/api/repl.html#repl_node_repl_history_file)
- tls
    - [CleartextStream](https://nodejs.org/dist/v0.10.0/docs/api/tls.html#tls_class_tls_cleartextstream)
      (this class was removed on v0.11.3, but never deprecated in documents)
    - [CryptoStream](https://nodejs.org/dist/v0.12.0/docs/api/tls.html#tls_class_cryptostream)
    - [SecurePair](https://nodejs.org/dist/v6.0.0/docs/api/tls.html#tls_class_securepair)
    - [createSecurePair](https://nodejs.org/dist/v6.0.0/docs/api/tls.html#tls_tls_createsecurepair_context_isserver_requestcert_rejectunauthorized_options)
- tty
    - [setRawMode](https://nodejs.org/dist/v0.10.0/docs/api/tty.html#tty_tty_setrawmode_mode)
- util
    - [debug](https://nodejs.org/dist/v0.12.0/docs/api/util.html#util_util_debug_string)
    - [error](https://nodejs.org/dist/v0.12.0/docs/api/util.html#util_util_error)
    - [isArray](https://nodejs.org/dist/v4.0.0/docs/api/util.html#util_util_isarray_object)
    - [isBoolean](https://nodejs.org/dist/v4.0.0/docs/api/util.html#util_util_isboolean_object)
    - [isBuffer](https://nodejs.org/dist/v4.0.0/docs/api/util.html#util_util_isbuffer_object)
    - [isDate](https://nodejs.org/dist/v4.0.0/docs/api/util.html#util_util_isdate_object)
    - [isError](https://nodejs.org/dist/v4.0.0/docs/api/util.html#util_util_iserror_object)
    - [isFunction](https://nodejs.org/dist/v4.0.0/docs/api/util.html#util_util_isfunction_object)
    - [isNull](https://nodejs.org/dist/v4.0.0/docs/api/util.html#util_util_isnull_object)
    - [isNullOrUndefined](https://nodejs.org/dist/v4.0.0/docs/api/util.html#util_util_isnullorundefined_object)
    - [isNumber](https://nodejs.org/dist/v4.0.0/docs/api/util.html#util_util_isnumber_object)
    - [isObject](https://nodejs.org/dist/v4.0.0/docs/api/util.html#util_util_isobject_object)
    - [isPrimitive](https://nodejs.org/dist/v4.0.0/docs/api/util.html#util_util_isprimitive_object)
    - [isRegExp](https://nodejs.org/dist/v4.0.0/docs/api/util.html#util_util_isregexp_object)
    - [isString](https://nodejs.org/dist/v4.0.0/docs/api/util.html#util_util_isstring_object)
    - [isSymbol](https://nodejs.org/dist/v4.0.0/docs/api/util.html#util_util_issymbol_object)
    - [isUndefined](https://nodejs.org/dist/v4.0.0/docs/api/util.html#util_util_isundefined_object)
    - [log](https://nodejs.org/dist/v6.0.0/docs/api/util.html#util_util_log_string)
    - [print](https://nodejs.org/dist/v0.12.0/docs/api/util.html#util_util_print)
    - [pump](https://nodejs.org/dist/v0.10.0/docs/api/util.html#util_util_pump_readablestream_writablestream_callback)
    - [puts](https://nodejs.org/dist/v0.12.0/docs/api/util.html#util_util_puts)
    - [_extend](https://nodejs.org/dist/v6.0.0/docs/api/util.html#util_util_extend_obj)

## Known Limitations

### Cannot report non-static properties:

- cluster
    - [worker.suicide](https://nodejs.org/dist/v6.0.0/docs/api/cluster.html#cluster_worker_suicide)
- crypto
    - [ecdh.setPublicKey](https://nodejs.org/dist/v6.0.0/docs/api/crypto.html#crypto_ecdh_setpublickey_public_key_encoding)
- net
    - [server.connections](https://nodejs.org/dist/v0.10.0/docs/api/net.html#net_server_connections)

### Cannot report dynamic things:

```js
require(foo).aDeprecatedProperty;
require("http")[A_DEPRECATED_PROPERTY]();
```

### Cannot understand assignments to properties:

```js
var obj = {
    Buffer: require("buffer").Buffer
};
new obj.Buffer(); /* missing. */
```

```js
var obj = {};
obj.Buffer = require("buffer").Buffer
new obj.Buffer(); /* missing. */
```

### Cannot understand assignments to arguments:

```js
(function(Buffer) {
    new Buffer(); /* missing. */
})(require("buffer").Buffer);
```

### Cannot understand reassignments:

```js
var Buffer = require("buffer").Buffer;
Buffer = require("another-buffer");
new Buffer(); /*ERROR: 'buffer.Buffer' constructor was deprecated.*/
```

## When Not To Use It

If you don't want to be warned on deprecated API, then it's safe to disable this rule.
