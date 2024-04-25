# Database setup
create database project-db 
create shema keycloak

# Certificates-Setup
keytool -genkeypair -alias jdg.digital -keyalg RSA -keysize 2048 -storetype PKCS12 -keystore jdg.digital.p12 -validity 3650
keytool -importkeystore -srckeystore jdg.digital.p12 -destkeystore intermediate.p12 -deststoretype PKCS12
openssl pkcs12 -in intermediate.p12 -out certificate.pem -nokeys
openssl pkcs12 -in intermediate.p12 -out private.key -nocerts