default: test

# Run acceptance tests
.PHONY: test
test:
	docker rm -f remotehost
	docker rm -f remotehost2
	docker network rm remote || true
	docker network create remote
	docker build -t remotehost tests
	docker run --rm -d --net remote --name remotehost remotehost
	docker run --rm -d --net remote --name remotehost2 remotehost
	docker run --rm --net remote -v $(PWD):/app -v ~/go:/go --workdir /app -e "TF_ACC=1" -e "TF_ACC_TERRAFORM_VERSION=1.0.11" -e "TESTARGS=$(TESTARGS)" golang:1.16 bash tests/test.sh
	docker rm -f remotehost
	docker rm -f remotehost2
	docker network rm remote
