DOCKER_BUILD_ARG := "FOO="

all: clean copy

build:
	docker build . --build-arg ${DOCKER_BUILD_ARG} -t local-vips-builder

copy: build
	docker create -ti --name local-vips-dummy vips-builder sh
	docker cp local-vips-dummy:/dist/ .
	docker rm -fv local-vips-dummy

clean:
	rm -rf dist/
