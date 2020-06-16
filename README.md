# springboot-jwt-jks-file-public-key
Here are simple steps to create jks file and public key

## Installation

Prerequisites:
- java8 or above
- openSSL

You need to install `java8` or above and `openSSL`. For windows click here to install [openSSL](http://gnuwin32.sourceforge.net/packages/openssl.htm).

Step 1:
```
keytool -genkeypair -alias authentication-server -keyalg RSA -dname "CN=GC,OU=DC,O=BatmanCorp,L=GT,C=Gotham" -keystore auth-server.jks -keypass batman20; -storepass bruce20; -validity 180 -keysize 2048
```

Step 2:
```
keytool -importkeystore -srckeystore auth-server.jks -srcstorepass bruce20; -srckeypass batman20; -srcalias authentication-server -destalias authentication-server -destkeystore auth-server.p12 -deststoretype PKCS12 -deststorepass batman_20; -destkeypass batman_20;
```

Step 3:
```
openssl pkcs12 -in auth-server.p12 -nodes -nocerts -out auth-server-private-key.pem
```
The password to type is the password used in -deststorepass, in this example is batman_20;

Step 4:
```
openssl rsa -in auth-server-private-key.pem -pubout -out PublicKey.pub
```
Now we have the public key

If you need a pem file:
```
openssl pkcs12 -in auth-server2.p12 -nokeys -out auth-client-cert2.pem
```

After step 1 you can type the following command to print the public key:
```
keytool -list -rfc --keystore auth-server.jks | openssl x509 -inform pem -pubkey -noout
```

Using the above command, most of the time the last lines printed are not in the correct order, but this is the simple way to get both files (JKS file and public key).
