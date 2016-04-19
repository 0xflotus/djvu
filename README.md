# djvu

Utils for [DJV Dynamic Json Schema Validator](https://github.com/korzio/djv) - useful api to use json-schema and djv tool.
This package is a clone of [jjv-utils](https://www.npmjs.com/package/jjv-utils). The difference is in using another core json schema validator. The version is synced with jjv-utils.
The plan is to create common utils for json schema. So, in the future (> 1 year) this two packages will be replaced by general utils packages.

## Installation

  `npm install djvu`

## Usage

Lets have an example - jsonSchema

```
jsonSchema = {
    "name": "test",
    "common": {
        "properties": {
            "type": {
                "enum": ["common"]
            }
        },
        "required": [
            "type"
        ]
    }
};
```

Utils will create an Djv env for further usage with a `test` (jsonSchema.name) namespace. If jsonSchema is not given as an argument - an envirionment will be created without any namespace.
```
schema = require(pathToDjvUtils)(jsonSchema);
```

Use `add` to add json schema after initialization
```
schema.add('test1', jsonSchema);
```

Use `is` to validate an object by schema reference, like Djv validate method
```
commonObj = { type: 'common' };
schema.is('test#/common', commonObj) => true
```
Use `generate` to generate a function to compare conditions. Easy to use in `each`, `find` and other `lodash` iterable functions.
```
var testCommon = schema.generate('test#/common');
[commonObj].map(testCommon) => [true]

var testNotCommon = schema.generate('test#/common', true);
[commonObj].map(testNotCommon) => [false]
```

Use `find` to easy `switch` condition. Usable for cozy factory functions.
```
var references = {
    'test#/common': 1
};

schema.find(references, commonObj) => 1
schema.find(references, unknownObj) => undefined
```

## API

- **add(String namespace, Object jsonSchema)** add schema to existing Djv environment
- **is(String reference, Object data)** validate object by schema reference
- **generate(String reference[, Boolean isReverse])** generates function to use in functional expressions
- **find(Object/Array types, Object data)** iterates through an object or array to find appropriate schema to given object. Returns `undefined` if not found.
- **env** original Djv-environment

## Tests

  `npm test`

## Resources

- [DJV Dynamic Json Schema Validator](https://github.com/korzio/djv)
- [npm package](https://www.npmjs.com/package/djvu)
- [github source code](https://github.com/korzio/djvu)
- [jjv-utils](https://www.npmjs.com/package/jjv-utils)