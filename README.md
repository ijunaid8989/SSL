SSL for Alpha!
===================

Create a Private.key file 

    openssl genrsa 2048 > private.key

create a .cfg file

reason: On AlphaSSL configuration you need `subjectAltName` 

yourfile.cfg **content**

    # -------------- BEGIN custom multicert.cnf -----

    HOME = .
    oid_section = new_oids
    [ new_oids ]
    [ req ]
    default_days = 730
    distinguished_name = req_distinguished_name
    encrypt_key = no
    string_mask = nombstr
    req_extensions = v3_req # Extensions to add to certificate request
    [ req_distinguished_name ]
    commonName              = *.evercam.io
    commonName_default      = *.evercam.io
    localityName            = Dublin 1
    organizationName        = Camba.tv Ltd
    emailAddress            = junaid@evercam.io
    countryName             = IE
    commonName_max = 64
    [ v3_req ]
    subjectAltName=DNS:ftp.evercam.io,DNS:blog.evercam.io,DNS:*.evercam.io

    # -------------- END custom openssl.cnf -----

create a `CSR` file with ` openssl req -new -key private.key -out multicert.csr -config multicert.cfg`


go to Configuration of AlphaSSL link and copy past the `multicert.csr` content there.

Keep `private.key` file.

`certchain.crt` this file will be provided by SSL provider. in case of ALPHASSL use this link 

https://www.ssl2buy.com/wiki/alphassl-intermediate-root-ca-certificates/ (Not given in any documentation on in email given by AlphaSSL)

use this command to upload on `aws`
`aws iam upload-server-certificate --server-certificate-name CERTNAME --certificate-body file://evercam.io.crt --private-key file://my-private-key.pem --certificate-chain file://certchain.crt`


for `Azure` it doesnt accept straight crt files create PFX file first

	
`openssl pkcs12 -export -out certificate.pfx -inkey private.key -in evercam.io.crt`

add password as well, it will be asked while uploading PFX to Azure.

