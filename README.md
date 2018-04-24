# JSON Schema

An implementation of [JSON Schema](http://json-schema.org/) in Swift.

## Changes
This is a list of changes from the original project.

- `multipleOf`, `minimum`, and `maximum` were only parsed as Doubles, failing for Integers. Now they (and the `validateMultipleOf` and `validateNumericLength` functions) use `NSNumber` and its `doubleValue`.  Now we can parse any `NSNumber` but still do logic using Doubles, meaning no real precision is lost.
- Renamed the project **JSONSchemaSwift** to avoid the **.swift** in the old name causing file/folder conflicts in certain build environments.

### Issues
- Doesn't support built-in formats `date-time`, `email`, and `hostname`.
- `null` type support seems sketchy.
- Remote `$ref` is not supported, but local ones are.
- Invalid `$ref` locations are not reported if not actually used.
- If an entry in a `properties` array is `[]`, then **all properties are seemingly ignored**.
- `dependencies` rhv must be an array even with one entry, but doesn't fail if it is not.

## Installation

[CocoaPods](http://cocoapods.org/) is the recommended installation method.

```ruby
pod 'JSONSchema'
```

## Usage

```swift
import JSONSchema

let schema = Schema([
    "type": "object",
    "properties": [
        "name": ["type": "string"],
        "price": ["type": "number"],
    ],
    "required": ["name"],
])

schema.validate(["name": "Eggs", "price": 34.99])
```

### Error handling

Validate returns an enumeration `ValidationResult` which contains all
validation errors.

```python
print(schema.validate(["price": 34.99]).errors)
>>> "Required property 'name' is missing."
```

JSONSchema has full support for the draft4 of the specification. It does not
yet support remote referencing [#9](https://github.com/kylef/JSONSchema.swift/issues/9).

## License

JSONSchema is licensed under the BSD license. See [LICENSE](LICENSE) for more
info.
