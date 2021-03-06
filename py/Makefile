BIN = $(abspath bin)
NOSE = $(BIN)/nosetests
PYTHON = $(BIN)/python
PYTHON_TESTS = $(abspath tests)
PIP = $(BIN)/pip


.PHONY: venv-present
venv-present:
	@virtualenv -q .

.PHONY: env
env: venv-present
	@. bin/activate && $(PIP) -q install -r requirements.txt 

.PHONY: websocket-test 
websocket-test:
	@$(PYTHON) $(PYTHON_TESTS)/ws_client.py --port 3114 --log INFO --handler ws_logic

.PHONY: uats
uats:
	( cd $(PYTHON_TESTS) && $(NOSE)  --nocapture -c $(PYTHON_TESTS)/nose.cfg --tc-file $(PYTHON_TESTS)/integration/setup.yaml --tc-format yaml $(PYTHON_TESTS)/integration/; )

.PHONY: single-uat
single-uat:
	( cd $(PYTHON_TESTS) && $(NOSE)  --nocapture -c $(PYTHON_TESTS)/nose.cfg --tc-file $(PYTHON_TESTS)/integration/setup.yaml --tc-format yaml $(PYTHON_TESTS)/integration/$(TEST_NAME); )

.PHONY: release-test
release-test:
	@$(PYTHON) $(PYTHON_TESTS)/release.py --workdir="$(WORKDIR)" --tarball=$(TARBALL) --blocks=100 --version=$(VER)

.PHONY: swagger-version-check
swagger-version-check:
	@$(PYTHON) $(PYTHON_TESTS)/swagger-version-check.py "$(VERSION)" "$(SWAGGER_YAML)" "$(SWAGGER_JSON)"
