build:
	docker build -t workshop/docker-run .

run:
	# docker | run | arguments | image                | cmd
	docker     run     --rm        workshop/docker-run   ls -la

run-mount:
	docker run --rm -v $(PWD):/foo workshop/docker-run  ls -la foo

run-nginx:
	docker run --rm -v $(PWD)/html:/usr/share/nginx/html -p 8080:80 nginx
