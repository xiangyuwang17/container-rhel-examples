CONTEXT = acme
VERSION = v3.2
IMAGE_NAME = starter-rhel-atomic

# Allow user to pass in OS build options
ifeq ($(TARGET),centos7)
	DFILE := Dockerfile.${TARGET}
else
	TARGET := rhel7
	DFILE := Dockerfile
endif

all: build
build:
	docker build --pull -t ${CONTEXT}/${IMAGE_NAME}:${TARGET}-${VERSION} -t ${CONTEXT}/${IMAGE_NAME} -f ${DFILE} .
	@if docker images ${CONTEXT}/${IMAGE_NAME}:${TARGET}-${VERSION}; then touch build; fi

lint:
	dockerfile_lint -f Dockerfile

test:
	$(eval CONTAINERID=$(shell docker run -tdi -u $(shell shuf -i 1000010000-1000020000 -n 1) \
	--cap-drop=KILL \
	--cap-drop=MKNOD \
	--cap-drop=SYS_CHROOT \
	--cap-drop=SETUID \
	--cap-drop=SETGID \
	${CONTEXT}/${IMAGE_NAME}:${TARGET}-${VERSION}))
	@sleep 3
	@docker exec ${CONTAINERID} env
	@docker rm -f ${CONTAINERID}

run:
	docker run -tdi -u $(shell shuf -i 1000010000-1000020000 -n 1) \
	--cap-drop=KILL \
	--cap-drop=MKNOD \
	--cap-drop=SYS_CHROOT \
	--cap-drop=SETUID \
	--cap-drop=SETGID \
	${CONTEXT}/${IMAGE_NAME}:${TARGET}-${VERSION}

clean:
	rm -f build