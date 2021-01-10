Solr Field-Type and Field Setup Script
--------------------------------------

This script aims to manage the field-type and field definitions of the apache solr.

Examples
--------

To learn how use this script, please check the following examples.
Before moving to the following examples, please setup the "solrbook" core.

```
## if you can use the solr command,
# solr create_core -c solrbook

## in case of the docker,
$ sudo docker pull solr:8.7
$ sudo docker run -it --rm --name solr -p 8983:8983 solr:8.7
$ sudo docker exec -it solr solr create_core -c solrbook
```

## How to setup the field-type

If you would like to setup the solr.TextField using the solr.EdgeNGramTokenizerFactory which named text_ja_edgengram,
prepare an arbitrary definition file likes as the examples/field-type/text-ja-edgengram.json.

```
{
  "field-type": {
    "name": "text_ja_edgengram",
    "class": "solr.TextField",
    "autoGeneratePhraseQueries": "true",
    "positionIncrementGap": "100",
    "analyzer": {
      "charFilter": [
        {
          "class": "solr.ICUNormalizer2CharFilterFactory"
        }
      ],
      "tokenizer": {
        "class": "solr.EdgeNGramTokenizerFactory",
        "minGramSize": "1",
        "maxGramSize": "1"
      },
      "filters": [
        {
          "class": "solr.CJKWidthFilterFactory"
        },
        {
          "class": "solr.LowerCaseFilterFactory"
        }
      ]
    }
  }
}

```

## How to setup the field

If you would like to setup the solr field using the text_ja_edgengram defined at the previous section,
prepare an arbitrary file likes as the examples/field/text-ja-edgengram.json.

```
{
  "field" : {
    "name" : "content_edgengram",
    "type" : "text_ja_edgengram",
    "multiValued" : "true",
    "indexed" : "true",
    "required" : "true",
    "stored" : "true"
  }
}

```

## How to inject the setup definitions

Please see the examples/Makefile.

```
SOLR_URL = http://localhost:8983/solr/solrbook/schema

SCRIPT_FILEPATH = ../setup-solr-schema.rb

.PHONY: example-add example-update

example-add:
	env SOLR_URL=$(SOLR_URL) \
	$(SCRIPT_FILEPATH) add field-type/*.json field/*.json

example-update:
	env SOLR_URL=$(SOLR_URL) \
	$(SCRIPT_FILEPATH) update field-type/*.json field/*.json
```
