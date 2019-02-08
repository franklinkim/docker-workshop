up:
	CONTEXT=Production docker-compose up -d
	
up-dev:
	CONTEXT=Development docker-compose up -d

foo:
	echo 

# Export dump
export: date=$(shell date +%Y%m%d_%H%M)
export:
	mkdir -p tmp/dump
	docker-compose run --rm --name export -v $(PWD)/tmp/dump:/dump database \
		sh -c 'mysqldump -h database -utypo3 -ptypo3 --all-databases --add-drop-database > /dump/dump.sql'
	cp -r fileadmin tmp/dump/fileadmin
	cd tmp/ && tar -czvf dump-${date}.tar.gz dump
	rm -rf tmp/dump

# Import dump
import:
	rm -rf tmp/dump
	cd tmp/ && tar -xzvf dump.tar.gz
	docker-compose run --rm --name export -v $(PWD)/tmp/dump:/dump database \
		sh -c 'mysql -h database -utypo3 -ptypo3 < /dump/dump.sql'
	rm -rf fileadmin
	mv tmp/dump/fileadmin fileadmin
	rm -rf tmp/dump