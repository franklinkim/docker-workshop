# Install deps
# NOTE Win: $(PWD) = C:\\path\to\07-typo3-full
install:
	docker run --rm -it -v $(PWD):/app composer install

# Change file permissions (server only)
chown:
	chown -R www-data:www-data .

# Create docker network
network:
	docker network create public

# Build container
build:
	docker-compose build

# Start up
up:
	docker-compose up -d

# Export dump
export:
	rm -f var/archive.tar.gz
	mkdir -p var/archive
	docker-compose run --rm --name export -v $(PWD)/var/archive:/archive database \
		sh -c 'mysqldump -h database -utypo3 -ptypo3 --all-databases --add-drop-database > /archive/dump.sql'
	cp -r public/fileadmin var/archive/fileadmin
	cd var/ && tar -czvf archive.tar.gz archive
	rm -rf var/archive

# Import dump
import:
	rm -rf var/archive
	cd var/ && tar -xzvf archive.tar.gz
	docker-compose run --rm --name export -v $(PWD)/var/archive:/archive database \
		sh -c 'mysql -h database -utypo3 -ptypo3 < /archive/dump.sql'
	rm -rf public/fileadmin
	mv var/archive/fileadmin public/fileadmin 
	rm -rf var/archive
	# TODO: clear typo3 cache
