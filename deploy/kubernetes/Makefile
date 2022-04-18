PWD = $(shell pwd)
MANIFESTS = $(or $(shell printenv MANIFESTS), manifests)
NAMESPACE ?= "sock-shop"
GROUP ?= "weaveworksdemo"
TAG_PREFIX ?= ""
.PHONY: gen-complete-demo
gen-complete-demo:
	awk -v ns="${NAMESPACE}" \
            -v group="${GROUP}" \
	    -v tp="${TAG_PREFIX}" \
	    '{gsub(/__NAMESPACE__/,ns); \
	    gsub(/__GROUP__/,group); \
	    gsub(/__TAG_PREFIX__/,tp); \
	    print}' ${MANIFESTS}/* > complete-demo.yaml
.PHONY: check-complete-demo
check-complete-demo:
	awk -v ns="${NAMESPACE}" \
            -v group="${GROUP}" \
	    -v tp="${TAG_PREFIX}" \
	    '{gsub(/__NAMESPACE__/,ns); \
	    gsub(/__GROUP__/,group); \
	    gsub(/__TAG_PREFIX__/,tp); \
	    print}' ${MANIFESTS}/* | diff complete-demo.yaml -
.dockerimage: Dockerfile
	docker build -t manifests-image .
	touch .dockerimage

.PHONY: docker-%
docker-%: .dockerimage
	docker \
	  run \
	  --rm \
	  -it \
	  -u ${UID}:${GID} \
	  -v ${PWD}:/workdir \
	  manifests-image \
	  make $*
