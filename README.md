# Welcome to the BitSlime Project

The BitSlime Project is focused on generating random, cryptographically validated Slime.

I created program that generates the precious Slime. Each slime we create has these attributes:

### 1. X509v3 Certificates

For each BitSlime that gets created by the BitSlime generator, it creates an [RSA Keypair](http://people.csail.mit.edu/rivest/Rsapaper.pdf) with a key size of 2048 bits. It then creates a CSR, defining the BitSlime's full name and Slime ID. It places those values in the Common Name field of the CSR - `CN = fullname:slime_id`.
It then submits the CSR to the Slime Signing Intermediate CA, which is Intermediate Certificate Authority using a prime256v1 (ASN.1) ECC keypair. The Slime Signing Intermediate CA signs/issues an X509v3 end-entity certificate according to the values in the CSR. It signs using ecdsa-with-SHA256. These certificates authenticate each BitSlime in the Slime database which prevents any forged slimes. If you own a BitSlime, you can look up its fullname/slime ID in the Certificates folder and decode it using openssl (`openssl x509 -in BitSlimeCertificate.pem -noout -text`) or an [online decoder](https://decoder.link/result). In addition, each BitSlime has the signature embedded in the picture so you can compare it with the signature on the certificate. Please check out the Cryptography section for more information about checking validation for the BitSlimes.

### 2. Randomly generated name. 

Each BitSlime has a semi-random name. They often come up really slime sounding. For instance, how does "Lorx Roqwkdl" not sound like a slime name? ;) Here is how the names are generated.

	rand_num = random.randint(1,9) # for the last name, it uses (1,10)
	firstletter = random.choice(string.ascii_uppercase)
	secondletter = random.choice(vowels)
	rest = ''.join(random.choices(string.ascii_lowercase, k = rand_num))    
	name = firstletter + secondletter + rest
	

### 3. Randomly generated color. 
We generate a random color for the BitSlime.  

### 4. Unique Slime ID
The unique Slime ID is placed in our BitSlime Database to ensure that no two slimes have the same ID. The ID will show you the version of the BitSlime. For instance, "v1-" means version 1 BitSlimes.

More BitSlime attributes to come in later versions of slime!

## Example

![Lorx_Roqwkd](https://raw.githubusercontent.com/BitSlimes/BitSlime_Project/main/etc/Lorx_Roqwkd_definitions.png)


The Signature attribute can we found in the decoded BitSlime certificate. Look towards the bottom in the Signature Algorithm field. The Signature in the BitSlime picture should be the same, except for the ":" characters. For example, this is Lorx's signature.

	30:44:02:20:30:6c:9c:4c:22:bc:63:31:12:a7:2b:85:d3:00:
	de:6d:94:48:6a:48:f0:37:02:63:ad:af:fd:db:27:01:49:16:
	02:20:25:56:34:00:bf:8c:14:3c:5b:80:62:08:75:50:20:a6:
	36:06:9f:e2:48:e7:e9:59:e1:fb:5b:85:c5:9c:66:ce


## Cryptographic Features

### How to verify the BitSlime Certificate was signed by our Intermediate CA (Slime Signing Intermediate CA)

You can use the Certificate chain (the Intermediate and Root CA Certificates) to do this with openssl. The CA chain is found [here](https://raw.githubusercontent.com/BitSlimes/BitSlimes/main/Slime_Certificate_Authority/CA_Chain.pem)

`openssl verify -CAfile CA_chain.pem BitSlimeCertificate.pem`

You can also use the decoder [here](https://decoder.link/ca_matcher).  Just remember to place the intermediate certificate as the CA Certificate and the BitSlime certificate as the end-entity.

Here are the links to the CAs. You can also check out the Slime_Certificate_Authority directory in this repo.

[Intermediate CA](https://raw.githubusercontent.com/BitSlimes/BitSlime_Project/main/Slime_Certificate_Authority/IntermediateCA.pem)

    Signature Algorithm: ecdsa-with-SHA256
        Issuer: C = US, CN = Slime Root CA
        Validity
            Not Before: Sep 12 01:25:46 2021 GMT
            Not After : May 22 01:25:46 2035 GMT
        Subject: C = US, CN = Slime Signing Intermediate CA
        Subject Public Key Info:
            Public Key Algorithm: id-ecPublicKey
                Public-Key: (256 bit)
                ASN1 OID: prime256v1
                NIST CURVE: P-256


[RootCA](https://raw.githubusercontent.com/BitSlimes/BitSlime_Project/main/Slime_Certificate_Authority/RootCA.pem)

 
    Signature Algorithm: ecdsa-with-SHA256
        Issuer: C = US, CN = Slime Root CA
        Validity
            Not Before: Sep 12 01:08:20 2021 GMT
            Not After : Sep  7 01:08:20 2041 GMT
        Subject: C = US, CN = Slime Root CA
        Subject Public Key Info:
            Public Key Algorithm: id-ecPublicKey
                Public-Key: (256 bit)
                 ASN1 OID: prime256v1
                NIST CURVE: P-256
