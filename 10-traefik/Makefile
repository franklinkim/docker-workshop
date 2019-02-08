SHELL := /bin/bash

# Vars
DOMAIN 	= "mydomain.com"
SUBJECT = "/C=DE/ST=HESSE/L=NEUSTADT/O=MYCOMP/CN=*.$(DOMAIN)"

# Color
ECHO_INFO = \033[0m[MAKE] `date '+%m.%d.%Y %H:%M:%S'`:\033[32;01m

.PHONY: ssl
ssl: cert-create cert-show cert-trust

cert-create:
	@echo "$(ECHO_INFO) Creating self signed SSL files..."
	@rm -f ./config/tls/$(DOMAIN).*
	@openssl req -newkey rsa:2048 -nodes -keyout config/tls/$(DOMAIN).key -subj $(SUBJECT) -out config/tls/$(DOMAIN).csr
	@openssl x509 -req -extfile <(printf "subjectAltName=DNS:*.$(DOMAIN),DNS:localhost,DNS:127.0.0.1") -days 365 -signkey config/tls/$(DOMAIN).key -in config/tls/$(DOMAIN).csr -out config/tls/$(DOMAIN).crt

cert-show:
	@echo "$(ECHO_INFO) Your new certificate..."
	@openssl x509 -in config/tls/$(DOMAIN).crt -text -noout

cert-trust:
	@echo "$(ECHO_INFO) Trusting self signed certificate..."
	sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain config/tls/$(DOMAIN).crt


up: ssl
	docker-compose up

down:
	docker-compose down