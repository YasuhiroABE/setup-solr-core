
SOLR_URL = http://localhost:8983/solr/solrbook/schema

SCRIPT_FILEPATH = ../setup-solr-schema.rb

.PHONY: example-add example-update

example-add:
	env SOLR_URL=$(SOLR_URL) \
	$(SCRIPT_FILEPATH) add field-type/*.json field/*.json

example-update:
	env SOLR_URL=$(SOLR_URL) \
	$(SCRIPT_FILEPATH) update field-type/*.json field/*.json
