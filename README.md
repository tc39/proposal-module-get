# Additional export-get statements

## ES Proposal

**Stage:** undefined

**Author:** Caridy Patiño

**Specification:** TBD

**AST:** TBD

**Transpiler:** TBD.

## Rationale

As part of the effort to make "Reflective Module Records" to fulfill most use-cases in Node to support inter-operatibility between CJS and ES Modules, we came across some CJS modules that were exporting a getter via `Object.definedProperty()`.

Further investigation revealed that there is no way to have an equivalent ES Module. This proposal is aiming to fill that gap in the spec by introducing a declarative way to define an export getter.

Two new export statements will be needed to fulfill this use-case, the `export get ___ () {}` and `export default get ___ () {}` statements can export a computed value via a getter-like definition. This proposal attempts to re-use an existing grammatical form that is already well understood by the community:

```
let o = {
    get foo() {
        return "something";
    }
}
```

### Proposed additions to Modules Export Statements

Export Statement Form           | [[ModuleRequest]] | [[ImportName]] | [[LocalName]] | [[ExportName]]
---------------------           | ----------------- | -------------- | ------------- | --------------
`export get foo() {}`           | **null**          | **null**       | `"foo"`       | `"foo"`
`export default get () {}`      | **null**          | **null**       | `"*default*"` | `"*default*"`
`export default get foo() {}`   | **null**          | **null**       | `"foo"`       | `"*default*"`


### Examples

#### Exporting a default computed value via a getter:

```js
let firstname = "Caridy";
let lastname  = "Patiño";
export default get () {
    return `${firstname} ${lastname}`;
}
```

CJS Equivalent:

```js
let firstname = "Caridy";
let lastname = "Patiño";
Object.defineProperty(module, 'exports', {
    get: () => `${firstname} ${lastname}`
});
```

#### Exporting a computed value via a getter:

```js
let firstname = "Caridy";
let lastname  = "Patiño";
export get fullname () {
    return `${firstname} ${lastname}`;
}
```

CJS Equivalent:

```js
let firstname = "Caridy";
let lastname = "Patiño";
module.exports = {
    get fullname() {
        return `${firstname} ${lastname}`;
    }
};
```
