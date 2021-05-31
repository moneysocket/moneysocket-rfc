# BOM #3: Provider Info

The Provider Info object describes the menu of API facilities a Provider application can provide to the Consumer application.

Also, most importantly, an indication of the balance for the account being presented to the Consumer.

## Wad Object

An object to represent a quantity of value denominated in millisatoshis (msats).

Also optionally, specifies the non-BTC asset that is the primary, user-understood asset of this quantity of value. If specified, it is understood that the `msat` value will be regularly updated to maintain stability of asset value.


- MUST specify a name/value pair `msat` with a value of Number type
    - MUST be set to indicate a quantity of millisatoshis
    - MUST NOT be set to `null`
    - MAY be a positive quantity
    - MAY be a negative quantity
    - MAY be a zero quantity
    - MAY be floating-point precision for sub-msat values

- MUST specify a name/value pair `non_btc` with a value of `null` or an Object type
    - if this Wad's primary, user-understood denomination is Bitcoin (BTC):
        - MUST be set to `null`
    - if this Wad's primary, user-understood denomination is something other than Bitcoin (BTC):
        - MUST be set to an Object value
            - MUST specify a name/value pair `units` with a value of Number type.
                - MUST be set to indicate a quantity of the asset denomination
                - MUST NOT be set to `null`
                - MAY be a positive quantity
                - MAY be a negative quantity
                - MAY be a zero quantity
                - MUST match negative/positive/zero classification of `msat` field of Wad object.
                - MAY be floating-point precision values
            - MUST specify a name/value pair `name` with String value that provides a descriptive human-undestandable name for the asset. (eg. `"United States dollar"`, or `"Bushel of Corn"`)
                - MUST NOT be set to `null`
                - MUST NOT be an empty string.
                - MAY be in a non-English language if appropriate to the context of end users.
                - SHOULD not be excessively long
                - SHOULD be written to be helpful to end-users to disambiguate denominations from one-another.
                - SHOULD NOT be ambiguous or deliberately confusing (eg. "Bitcoin")
                - SHOULD be in the sigular tense (eg. `dollar` not `dollars`)
            - MAY include a name/value pair `rate_timestamp` with a value of Number type
                - MUST be set to a UNIX timestamp
                - MUST indicate time the exchange rate between BTC and the non-BTC asset was calculated
                - MUST be a value no later than the generating computer OS's system time.
                - MAY be floating point or integer specified
            - if the denomination has been assigned an [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217) code:
                - MUST specify a name/value pair `code` with a value of String type
                - MUST specify a name/value pair `iso_num` with a value of Number type
                    - MUST be set to the number assigned by ISO 4217
            - if the denomiation has NOT been assigned an [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217) code:
                - if the denomination has a commonly-understood abbreviated code value:
                    - MUST be set to a commonly-understood abbreviated code value for the denomination (eg. `"LTC"`, `"ETH"`, `"DOGE"`, `"RAREPEPE"`)
                - if the denomination has no commonly-understood abreviated code value:
                    - MUST set value to a string
                    - MAY set value to be an empty string: `""`
                    - SHOULD be set to a reasonable approximation of a code descriptive of the asset.
                        - MUST, when set to a non-empty string, be minimum three and maximum twelve capital English letters. (eg. `"MARBLES"`, `"EGGPLANTS"`)
                    - SHOULD NOT be the same as commonly understood code values (eg. `"BTC"`, `"USD"`)

            - if the denomination has a commonly-understood Unicode currency symbol or sequence of Unicode character (eg. `$` for United States dollar, `€` for Euro, `ع.د` for Iraqi dinar, etc.)
                - MUST specify a name/value pair `symbol` with a value of String type
                - MUST be set to the symbol character or characters with no leading or trailing whitespace.
            - if the denomination has no commonly-understood currnecy symbol:
                - MUST set to at least an empty string `""`
                - MAY specify a reasonable choice to use in place of a currency symbol (eg. an emoji)
                - MUST NOT use more than a sequence of 6 Unicode characters

            - if the denomination has a commonly-understood expectations of precision when displayed for reading (eg. 2 for United states dollar, 0 for Japanese yen, etc.)
                - SHOULD specify a name/value pair `fmt_decimal` with a value of Number type:
                    - MUST be set to a positive or zero integer value
                    - MUST NOT be larger than 12

### Exchange rate

The `msat` value and `units` in the `non_btc` objects imply an exchange rate between these assets which can be mathematically derived. It is not redundantly specified in the object, but this value is typically important for calculations involving this object.

### Examples

#### BTC

Bitcoin-denominated wad:
```
{ 'msat': 12350,
  'non_btc': null}
```
Suggested end-user display: `₿ 12.350 sat`

#### USD

USD-denominated wad:
```
{ 'msat': 1234567890,
  'non_btc': {'units':          123.456,
              'name':           "United States dollar",
              'rate_timestamp': 1622048707,
              'code':           "USD",
              'iso_num':        840,
              'symbol':         "$",
              'fmt_decimals':   2,
             }
}
```

Suggested end-user display: `$ 123.46 USD`

#### Other

Cucumber-denominated wad:
```
{ 'msat': 222333444,
  'non_btc': {'units':        55.55,
              'name':         "Cucumber vegetables",
              'code':         "CUCUMBER",
              'symbol':       "🥒",
              'fmt_decimals': 1,
             }
}
```

Suggested end-user display: `🥒 55.6 CUCUMBER`



## Provider Info Object


An language object passed over the socket connection inside a message language object to communicate the API capabilities of the Provider application as well as the particular details of the account and balance available.

- MUST include a name/value pair `account_uuid` with value of String type
    - MUST be a string-formatted Universal Unique Identifier version 4 corresponding to the Provider account.

- MUST include a name/value pair `payer` with value of Boolean type
    - MUST be set to `true` if the account is permitted to fulfil invoice payment requests.
    - MUST be set to `false` if the account is set to reject invoice payment requests.

- MUST include a name/value pair `payee` with value of Boolean type
    - MUST be set to `true` if the account is permitted to generate invoices to receive payments.
    - MUST be set to `false` if the account is set to reject invoice generation requests.

- MUST include a name/value pair `wad` with value of Object type
    - MUST be a wad object to represent the balance of an account as specified above.
    - SHOULD NOT be a wad with a negative `msat` value

- MUST include a name/value pair `public_keys` with value of Object type
    - MAY be an empty Object
    - MAY include name/value pairs with hex-formatted public key strings with values of Object type
        - MUST have a name/value pair `curve` with a value of String type
            - MUST have a non-empty string as value
            - MUST specify the elliptic curve used to derive the public key from the private key counterpart
            - MUST NOT be an empty string
        - MUST have a name/value pair `features` with a value of Array type
            - MAY be an empty Array
            - MAY include a String with a indentifier for each public key function
                - MAY NOT be an empty string
                - MAY include `SCHNORR_SIGN_ARMOR` to indicate the capability to provide PGP-style signature of a provided message for the public key
                - MAY include `SCHNORR_SIGN_BINARY` to indicate the capability to provide a binary signature for a binary message for the public key
                - MAY include `ENCRYPT_BIN` to... TODO
                - MAY include `DECRYPT_BIN` to... TODO
                - SHOULD NOT be an excessive amount of data that taxes the computational resources of applications and message parsers.
        - MAY include a name/value pair `friendly_name` with a value of String type
            - MUST NOT be an empty string
            - MUST NOT be longer than 64 characters
            - SHOULD set to a string of a name that the holder of the private key would prefer their public key to be referred to.
            - SHOULD NOT contain whitespace
        - MAY have additional name/value pairs included in the Object value for a public key.
            - SHOULD NOT be an excessive amount of data that taxes the computational resources of applications and message parsers.

- MUST have a name/value pair `features` with a value of Array type
    - MAY be an empty Array
    - MAY include String values in the array to describe non-specification features that the sender application supports
        - SHOULD select a unique, descriptive string for the feature
        - SHOULD select a string that reasonably short
        - SHOULD NOT include feature data encoded in the string
        - SHOULD include feature data, if applicable, in the `feature_data` data structure instead

- MUST have a name/value pair `feature_data` with a value of Object type
    - MAY be an empty Object
    - MAY include name/value pairs in the Object with arbitrary value as determined by the non-specification feature
        - MUST set `name` corresponding to the String value in the `features` array.
        - SHOULD NOT be an excessive amount of data that taxes the computational resources of applications and message parsers.

- MAY have additional name/value pairs included in the JSON object.
    - SHOULD use `features` and `feature_data` to pass non-standard signals and data instead of additional name/value pairs whenever possible
    - SHOULD NOT be an excessive amount of data that taxes the computational resources of applications and message parsers.

### Examples
```
{
   "account_uuid": "a5b32936-3143-4856-bdea-7b3dda7aa5e8",
   "wad":          {'msat': 654321,
                    'non_btc': null},
   "payer":        true,
   "payee":        false,
   "public_keys":  {
        "5891b5b522d5df086d0ff0b110fbd9d21bb4fc7163af34d08286a2e846f6be03":
            {"curve":    "secp256k1",
             "features": ["SCHNORR_SIGN_ARMOR",
                          "SCHNORR_SIGN_BINARY",
                          "ENCRYPT_BIN",
                          "DECRYPT_BIN"],
             "friendly_name": "UnicornPrincess",
            }
    }
    "features":     [],
    "feature_data": {},
}
```

## Appendix A - Wad Test Vectors

* [Wad Encode](test-vectors/03-wad_encode.json)
* [Wad Decode Errors](test-vectors/03-wad-decode-error.json)

## Appendix B - Provider Info Test Vectors
