install:
	docker run --rm -it -v $(PWD):/app composer install

build:
	docker-compose build

up:
	docker-compose up -d

export:
	rm -f var/archive.tar.gz
	mkdir -p var/archive
	docker-compose run --rm --name export -v $(PWD)/var/archive:/archive database \
		sh -c 'mysqldump -h database -utypo3 -ptypo3 --all-databases --add-drop-database > /archive/dump.sql'
	cp -r public/fileadmin var/archive/fileadmin
	cd var/ && tar -czvf archive.tar.gz archive
	rm -rf var/archive

import:
	rm -rf var/archive
	cd var/ && tar -xzvf archive.tar.gz
	docker-compose run --rm --name export -v $(PWD)/var/archive:/archive database \
		sh -c 'mysql -h database -utypo3 -ptypo3 < /archive/dump.sql'
	rm -rf public/fileadmin
	mv var/archive/fileadmin public/fileadmin 
	rm -rf var/archive
