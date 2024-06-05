# sriov-network-operator

This repository is the mirror of [sriov-network-operator](https://github.com/k8snetworkplumbingwg/sriov-network-operator) with some modifications for EdgeStack.

## Image Build Example

``` bash
docker login registry.gitlab.com -u sonaproject -p ${LOGIN_TOKEN}

DOCKERFILE=("Dockerfile" "Dockerfile.sriov-network-config-daemon" "Dockerfile.webhook")
TAG_PREFIX="registry.gitlab.com/sonaproject"
TAG_IMG=("${TAG_PREFIX}/sriov-network-operator" "${TAG_PREFIX}/sriov-network-config-daemon" "${TAG_PREFIX}/sriov-network-webhook")

for ((i=0; i<${#DOCKERFILE[@]}; i+=1)) do
docker buildx build --push -f ${DOCKERFILE[i]} -t ${TAG_IMG[i]}:amd64 --provenance false --platform linux/amd64 .
docker buildx build --push -f ${DOCKERFILE[i]} -t ${TAG_IMG[i]}:arm64 --provenance false --platform linux/arm64 .

docker manifest create ${TAG_IMG[i]} \
--amend ${TAG_IMG[i]}:amd64 \
--amend ${TAG_IMG[i]}:arm64
docker manifest push ${TAG_IMG[i]}
done
```
