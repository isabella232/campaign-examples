ACTION=action.json

search-scope:
	src actions scope-query -f $(ACTION)

build:
	docker image pull node:13
	docker image build -t sourcegraph/rxjs-upgrade:latest .

exec:
	src actions exec -timeout 45m -keep-logs -f $(ACTION) > diffs.json

campaign-plan:
	cat diffs.json | src campaign plan create-from-patches

rxjs-upgrade-docker-command:
	git clone https://github.com/sourcegraph/codeintellify.git
	cat rxjs-upgrade.sh | docker container run \
		--name rxjs-upgrade \
		--rm \
		-i \
		--workdir /work \
		-v $(PWD)/codeintellify:/work \
		node:13-alpine sh
	cd codeintellify && git --no-pager diff
	rm -fr codeintellify
