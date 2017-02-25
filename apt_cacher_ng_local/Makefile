include ../docker_make_utils/Makefile.help
include ../docker_make_utils/Makefile.functions
include ../docker_make_utils/Makefile.container

# docker image version
VERSION := latest
# The name of the docker image.
IMAGE_NAME := minimum2scp/apt-cacher-ng:$(VERSION)
# docker container name
NAME := apt-cacher-ng
# Set the option PORTS='-p 53:53/udp' to use the DNS server on localhost.
PORTS := -p 3142:3142
# Cache directory.
CACHE_DIR := /var/cache/apt-cacher-ng

DATA_NAME := $(NAME)-data

define DOCKER_CMD :=
docker run \
-d \
--name $(NAME) \
-e "ACNG_BACKENDS_DEBIAN=http://ftp.plusline.de/debian/" \
$(PORTS) \
--volumes-from="$(DATA_NAME)" \
$(IMAGE_NAME)
endef

define DATA_DOCKER_CMD :=
docker run \
--name $(DATA_NAME) \
-v "$(CACHE_DIR):/var/cache/apt-cacher-ng" \
$(IMAGE_NAME) \
bash -c 'chown -R apt-cacher-ng /var/cache/apt-cacher-ng'
endef

.PHONY +: run rerun rm clean test restart bash

run: $(CACHE_DIR) _data-run _run ##@default Starts the container.

rerun: _rerun ##@targets Stops and starts the container.

rm: _rm _data-rm ##@targets Stops and removes the container.

clean: _clean ##@targets Stops and removes the container and removes all created files.
	sudo rm -rf $(CACHE_DIR)

test: _test ##@targets Tests if the container is running.
	
restart: _restart ##@targets Restarts the container.

bash: test _bash ##@targets Executes the bash inside the running container.

$(CACHE_DIR):
	sudo mkdir -p "$(CACHE_DIR)"
