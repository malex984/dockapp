mkfile_path := $(abspath $(lastword $(MAKEFILE_LIST)))
COPYFILES   := x11vnc.sh
include ../Makefile.inc


RUNTERM=-it -a stdout -a stderr --ipc=host --net=host \
        -v /etc/localtime:/etc/localtime:ro -v /tmp/:/tmp/:rw

# -v /dev/shm:/dev/shm -v /dev/dri:/dev/dri 

OPTS=/sbin/my_init --skip-startup-files --quiet --skip-runit

CMD=/bin/bash -c 'setup_ogl.sh; x11vnc.sh'

check: $(TS)
	### [ -z "${DISPLAY}" ] && exit 1
	echo "CMD: '${CMD}' '${ARGS}'"
	xhost + || exit 1
	docker ps -a
	docker run --rm -e XAUTHORITY -e DISPLAY -e VNC_PASSWD=4321 ${RUNTERM} --label is_top_app=0 $(IMG) ${OPTS} -- ${CMD} ; echo $$? > .returncode
	cat .returncode && rm .returncode

build: build_full
