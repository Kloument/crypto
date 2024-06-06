## Revision Crypto 

#### Généré clé publique / privée
`openssl genpkey -algorithm RSA -out private_key.pem`

`openssl rsa -pubout -in private_key.pem -out public_key.pem`

#### Vérifier si le message à été modifier :
`openssl dgst -sha256 -verify public_key.pem -signature signature.bin message.txt`


#### Genre une cle RSA avec mot de passe de taille 1024bit
`openssl genpkey -algorithm RSA -out maCle.pem -aes256 -pass pass:Clement -pkeyopt rsa_keygen_bits:1024`

#### Hachage de fichier et de texte : 

`md5sum fichier.txt`

`echo -n "texte à hacher" | md5sum`

`sha1sum fichier.txt`

`sha256sum fichier.txt`

`sha512sum fichier.txt`

`echo -n "texte à hacher" | sha1sum`
`echo -n "texte à hacher" | sha1sum`
`echo -n "texte à hacher" | sha512sum`



# 1. Génération d'une paire de clés privée/publique

## Générer une clé privée RSA (2048 bits par exemple)
`openssl genpkey -algorithm RSA -out private.key -aes256`
# Note : L'option -aes256 chiffre la clé privée avec AES-256. Vous pouvez omettre cette option si vous ne souhaitez pas chiffrer la clé.

## Générer une clé privée RSA sans chiffrement
`openssl genpkey -algorithm RSA -out private.key`

# 2. Génération d'une CSR (Certificate Signing Request)

## Générer une CSR en utilisant une clé privée existante
`openssl req -new -key private.key -out request.csr`
# Cette commande vous demandera de fournir des informations pour le certificat, comme le nom commun (CN), l'organisation, etc.

# 3. Auto-signature d'un certificat (pour créer un certificat auto-signé)

## Générer un certificat auto-signé (valable un an par exemple)
`openssl req -x509 -new -nodes -key private.key -sha256 -days 365 -out certificate.crt`

# 4. Conversion de certificats entre différents formats

## Convertir un certificat PEM en DER
`openssl x509 -outform der -in certificate.crt -out certificate.der`

## Convertir un certificat DER en PEM
`openssl x509 -inform der -in certificate.der -out certificate.crt`

## Convertir une clé privée PEM en DER
`openssl rsa -outform der -in private.key -out private.der`

## Convertir une clé privée DER en PEM
`openssl rsa -inform der -in private.der -out private.key`

# 5. Visualisation d'un certificat

## Afficher les informations d'un certificat
`openssl x509 -in certificate.crt -text -noout`

## Afficher les informations d'une CSR
`openssl req -in request.csr -text -noout`

# 6. Comparaison de certificats et de clés

## Vérifier qu'une clé privée correspond à un certificat
`openssl x509 -noout -modulus -in certificate.crt | openssl md5`
`openssl rsa -noout -modulus -in private.key | openssl md5`
# Si les deux valeurs de hachage (MD5) sont identiques, la clé privée correspond au certificat.

## Vérifier qu'une CSR correspond à une clé privée
`openssl req -noout -modulus -in request.csr | openssl md5`
`openssl rsa -noout -modulus -in private.key | openssl md5`
# Si les deux valeurs de hachage (MD5) sont identiques, la CSR correspond à la clé privée.

# 7. Chiffrement et déchiffrement de fichiers avec une clé publique/privée

## Chiffrer un fichier avec une clé publique
`openssl rsautl -encrypt -inkey public.pem -pubin -in fichier.txt -out fichier.chiffre`

## Déchiffrer un fichier avec une clé privée
`openssl rsautl -decrypt -inkey private.key -in fichier.chiffre -out fichier.dechiffre`

# 8. Créer un PKCS#12 (PFX) à partir d'une clé privée et d'un certificat

## Générer un fichier PFX
`openssl pkcs12 -export -out fichier.pfx -inkey private.key -in certificate.crt -certfile ca-bundle.crt`
# Note : Le fichier ca-bundle.crt contient les certificats des autorités de certification intermédiaires.
