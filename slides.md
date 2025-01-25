---
theme: seriph
background: https://source.unsplash.com/collection/94734566/1920x1080
class: 'text-center'
highlighter: shiki
lineNumbers: true
info: |
  ## JSONSchema Deep Dive
  Advanced concepts and applications of JSONSchema
drawings:
  persist: false
css: unocss
---


# JSONSchema as a runtime typesystem
Advanced concepts and practical applications

Damodharan J | Zoho

[Linkedin](https://www.linkedin.com/in/damojay/) | [Github](https://github.com/damodharanj) | [X.com](https://x.com/twitdamojay)

---
layout: section
---

# Introduction to JSONSchema
Core concepts and fundamentals

---

# Primitives & Constraints

```json
{ "type": "string" }
```

* [String](http://ai-schema-lab.phaged.in/?prompt=create+a+list+of+numbers&schema=%7B%0A++%22type%22%3A+%22string%22%0A%7D&output=%221%22)

```json
{ "type": "number" }
```

* [Number](http://ai-schema-lab.phaged.in/?prompt=create+a+list+of+numbers&schema=%7B%0A++%22type%22%3A+%22number%22%0A%7D&output=1)

* Constraints (Refinements)

---

# Collections

```json
{ "type": "array", "items": {  } }
```

* [Array](http://ai-schema-lab.phaged.in/?prompt=create+a+list+of+numbers&schema=%7B%0A++%22type%22%3A+%22array%22%2C%0A++%22items%22%3A+%7B+%22type%22%3A+%22number%22+%7D%0A%7D&output=%5B1%2C2%2C3%5D)


```json
{ 
  "type": "object",
  "properties": {  } 
}
```

* [Object](http://ai-schema-lab.phaged.in/?prompt=generate+a+person&schema=%7B%0A++%22type%22%3A+%22object%22%2C%0A++%22properties%22%3A+%7B%0A++++%22name%22%3A+%7B+%22type%22%3A+%22string%22+%7D%2C%0A++++%22age%22%3A+%7B+%22type%22%3A+%22number%22+%7D%0A++%7D%2C%0A++%22required%22%3A+%5B%22name%22%2C+%22age%22%5D%2C%0A++%22additionalProperties%22%3A+false%0A%7D&output=%7B%0A++%22name%22%3A+%22Alice+Smith%22%2C%0A++%22age%22%3A+29%0A%7D )

---

# Connectives

* Conntectives - used to compose schemas 
* [AllOf - And](http://ai-schema-lab.phaged.in/?prompt=what+is+2+%2B+2&schema=%7B%0A++%22allOf%22%3A+%5B%0A++++%7B+%22type%22%3A+%22string%22+%7D%2C%0A++++%7B+%22maxLength%22%3A+5+%7D%0A++%5D%0A%7D%0A%0A&output=%22funct%22)
* [AnyOf - Or](http://ai-schema-lab.phaged.in/?prompt=what+is+2+%2B+2&schema=%7B%0A++%22anyOf%22%3A+%5B%0A++++%7B+%22type%22%3A+%22string%22%2C+%22maxLength%22%3A+5+%7D%2C%0A++++%7B+%22type%22%3A+%22number%22%2C+%22minimum%22%3A+0+%7D%0A++%5D%0A%7D%0A%0A&output=-1)
* [OneOf - Xor](http://ai-schema-lab.phaged.in/?schema=%7B%0A++%22oneOf%22%3A+%5B%0A++++%7B%0A++++++%22type%22%3A+%22object%22%2C%0A++++++%22properties%22%3A+%7B%0A++++++++%22kind%22%3A+%7B+%22const%22%3A+%22circle%22+%7D%2C%0A++++++++%22radius%22%3A+%7B+%22type%22%3A+%22number%22+%7D%0A++++++%7D%2C%0A++++++%22required%22%3A+%5B%22kind%22%2C+%22radius%22%5D%0A++++%7D%2C%0A++++%7B%0A++++++%22type%22%3A+%22object%22%2C%0A++++++%22properties%22%3A+%7B%0A++++++++%22kind%22%3A+%7B+%22const%22%3A+%22square%22+%7D%2C%0A++++++++%22sideLength%22%3A+%7B+%22type%22%3A+%22number%22+%7D%0A++++++%7D%2C%0A++++++%22required%22%3A+%5B%22kind%22%2C+%22sideLength%22%5D%0A++++%7D%0A++%5D%0A%7D&output=%7B%0A%22kind%22%3A+%22circle%22%2C%0A%22radius%22%3A+6%0A%7D)

---

# Recursion in JSONSchema

```json
{
  "type": "object",
  "properties": {
    "value": { "type": "string" },
    "children": {
      "type": "array",
      "items": { "$ref": "#" }
    }
  }
}
```

[Tree Example](http://ai-schema-lab.phaged.in/?schema=%7B%0A++%22type%22%3A+%22object%22%2C%0A++%22properties%22%3A+%7B%0A++++%22value%22%3A+%7B+%22type%22%3A+%22number%22+%7D%2C%0A++++%22left%22%3A+%7B%0A++++++%22oneOf%22%3A+%5B%0A++++++++%7B+%22type%22%3A+%22null%22+%7D%2C%0A++++++++%7B+%22%24ref%22%3A+%22TreeNode%22+%7D%0A++++++%5D%0A++++%7D%2C%0A++++%22right%22%3A+%7B%0A++++++%22oneOf%22%3A+%5B%0A++++++++%7B+%22type%22%3A+%22null%22+%7D%2C%0A++++++++%7B+%22%24ref%22%3A+%22TreeNode%22+%7D%0A++++++%5D%0A++++%7D%0A++%7D%2C%0A++%22required%22%3A+%5B%22value%22%2C+%22left%22%2C+%22right%22%5D%0A%7D&output=%7B%0A++%22value%22%3A+6%2C%0A++%22left%22%3A+null%2C%0A++%22right%22%3A+%7B%0A++++%22value%22%3A+6%2C%0A++++%22left%22%3A+null%2C%0A++++%22right%22%3A+null%0A++%7D%0A%7D)

---
layout: section
---

# ADT in JSONSchema
Algebraic Data Types representation

---

# Functional Tree Definition

```json
{
  "type": "object",
  "properties": {
    "type": { "const": "node" },
    "value": { "type": "string" },
    "left": { 
      "oneOf": [
        { "$ref": "#" },
        { "type": "null" }
      ]
    },
    "right": {
      "oneOf": [
        { "$ref": "#" },
        { "type": "null" }
      ]
    }
  }
}
```

[Tree](https://ai-schema-lab.phaged.in/?prompt=what+is+2+%2B+2&schema=%7B%0A++%22%24id%22%3A+%22TreeNode%22%2C%0A++%22type%22%3A+%22object%22%2C%0A++%22properties%22%3A+%7B%0A++++%22value%22%3A+%7B+%22type%22%3A+%22number%22+%7D%2C%0A++++%22left%22%3A+%7B%0A++++++%22oneOf%22%3A+%5B%0A++++++++%7B+%22type%22%3A+%22null%22+%7D%2C%0A++++++++%7B+%22%24ref%22%3A+%22TreeNode%22+%7D%0A++++++%5D%0A++++%7D%2C%0A++++%22right%22%3A+%7B%0A++++++%22oneOf%22%3A+%5B%0A++++++++%7B+%22type%22%3A+%22null%22+%7D%2C%0A++++++++%7B+%22%24ref%22%3A+%22TreeNode%22+%7D%0A++++++%5D%0A++++%7D%0A++%7D%2C%0A++%22required%22%3A+%5B%22value%22%2C+%22left%22%2C+%22right%22%5D%0A%7D&output=%7B%0A++%22value%22%3A+6%2C%0A++%22left%22%3A+null%2C%0A++%22right%22%3A+%7B%0A++++%22value%22%3A+6%2C+%0A++++%22left%22%3A+null%2C%0A++++%22right%22%3A+null%0A++%7D%0A%7D)

---
layout:
---

# Generic Tree with $ref

* Equivalent of Generics
    * Tree\<string\>
    * Tree\<number\>

* [Generic Tree Example](https://ai-schema-lab.phaged.in/?prompt=what+is+2+%2B+2&schema=%7B%0A++++%22%24schema%22%3A+%22https%3A%2F%2Fjson-schema.org%2Fdraft%2F2020-12%2Fschema%22%2C%0A++++%22%24id%22%3A+%22http%3A%2F%2Ftest.com%2Fbounded-number-tree%22%2C%0A++++%22%24defs%22%3A+%7B%0A++++++++%22T%22%3A+%7B%0A++++++++++++%22%24id%22%3A+%22T%22%2C%0A++++++++++++%22type%22%3A+%22number%22%0A++++++++%7D%2C%0A++++++++%22zero%22%3A+%7B%0A++++++++++++%22%24id%22%3A+%22zero%22%2C%0A++++++++++++%22type%22%3A+%22number%22%2C%0A++++++++++++%22const%22%3A+0%0A++++++++%7D%2C%0A++++++++%22succ%22%3A+%7B%0A++++++++++++%22%24id%22%3A+%22succ%22%2C%0A++++++++++++%22type%22%3A+%22object%22%2C%0A++++++++++++%22properties%22%3A+%7B%0A++++++++++++++++%22next%22%3A+%7B%0A++++++++++++++++++++%22oneOf%22%3A+%5B%0A++++++++++++++++++++++++%7B%22%24ref%22%3A+%22zero%22%7D%2C%0A++++++++++++++++++++++++%7B%22%24ref%22%3A+%22succ%22%7D%0A++++++++++++++++++++%5D%0A++++++++++++++++%7D%0A++++++++++++%7D%2C%0A++++++++++++%22required%22%3A+%5B%22next%22%5D%0A++++++++%7D%2C%0A++++++++%22list%22%3A+%7B%0A++++++++++++%22%24id%22%3A+%22list%22%2C%0A++++++++++++%22type%22%3A+%22object%22%2C%0A++++++++++++%22properties%22%3A+%7B%0A++++++++++++++++%22next%22%3A+%7B%0A++++++++++++++++++++%22oneOf%22%3A+%5B%0A++++++++++++++++++++++++%7B+%22%24ref%22%3A+%22zero%22+%7D%2C%0A++++++++++++++++++++++++%7B+%22%24ref%22%3A+%22list%22+%7D%0A++++++++++++++++++++%5D%0A++++++++++++++++%7D%2C%0A++++++++++++++++%22value%22%3A+%7B%0A++++++++++++++++++++%22%24dynamicRef%22%3A+%22T%22%0A++++++++++++++++%7D%0A++++++++++++%7D%2C%0A++++++++++++%22required%22%3A+%5B%22next%22%2C+%22value%22%5D%0A++++++++%7D%0A++++%7D%2C%0A++++%22%24ref%22%3A+%22list%22%0A%7D%0A&output=%7B+%0A++%22value%22%3A+0%2C++%0A++%22next%22%3A+%7B++++%0A++++%22value%22%3A+9%2C%0A++++%22next%22%3A+0+++%0A++%7D%0A%7D+)

---
layout: section
---

# Structural Dependent Types
Advanced type relationships

---

# Dependent Requirements

```json
{
  "type": "object",
  "properties": {
    "payment": { "enum": ["card", "bank"] },
    "cardNumber": { "type": "string" }
  },
  "dependentRequired": {
    "payment": ["cardNumber"]
  }
}
```

[Example](http://ai-schema-lab.phaged.in/?prompt=what+is+2+%2B+2&schema=%7B%0A++%22type%22%3A+%22object%22%2C%0A%0A++%22properties%22%3A+%7B%0A++++%22name%22%3A+%7B+%22type%22%3A+%22string%22+%7D%2C%0A++++%22credit_card%22%3A+%7B+%22type%22%3A+%22number%22+%7D%2C%0A++++%22billing_address%22%3A+%7B+%22type%22%3A+%22string%22+%7D%0A++%7D%2C%0A%0A++%22required%22%3A+%5B%22name%22%5D%2C%0A%0A++%22dependentRequired%22%3A+%7B%0A++++%22credit_card%22%3A+%5B%22billing_address%22%5D%0A++%7D%0A%7D%0A%0A&output=%7B%0A++%22name%22%3A+%22John+Doe%22%2C%0A++%22credit_card%22%3A+5555555555555555%2C%0A++%22billing_address%22%3A+%22555+Debtor%27s+Lane%22+%0A%7D)

---

# Conditional Schemas

```json
{
  "type": "object",
  "properties": {
    "type": { "enum": ["user", "admin"] },
    "permissions": { "type": "array" }
  },
  "if": {
    "properties": { "type": { "const": "admin" } }
  },
  "then": {
    "required": ["permissions"]
  }
}
```

* [If-Then-Else](http://ai-schema-lab.phaged.in/?prompt=what+is+2+%2B+2&schema=%7B%0A++%22type%22%3A+%22object%22%2C%0A++%22properties%22%3A+%7B%0A++++%22street_address%22%3A+%7B%0A++++++%22type%22%3A+%22string%22%0A++++%7D%2C%0A++++%22country%22%3A+%7B%0A++++++%22default%22%3A+%22United+States+of+America%22%2C%0A++++++%22enum%22%3A+%5B%22United+States+of+America%22%2C+%22Canada%22%5D%0A++++%7D%0A++%7D%2C%0A++%22if%22%3A+%7B%0A++++%22properties%22%3A+%7B%0A++++++%22country%22%3A+%7B+%22const%22%3A+%22United+States+of+America%22+%7D%0A++++%7D%0A++%7D%2C%0A++%22then%22%3A+%7B%0A++++%22properties%22%3A+%7B%0A++++++%22postal_code%22%3A+%7B+%22type%22%3A+%22string%22%2C+%22pattern%22%3A+%22%5B0-9%5D%7B5%7D%28-%5B0-9%5D%7B4%7D%29%3F%22+%7D%0A++++%7D%0A++%7D%2C%0A++%22else%22%3A+%7B%0A++++%22properties%22%3A+%7B%0A++++++%22postal_code%22%3A+%7B+%22type%22%3A+%22string%22%2C+%22pattern%22%3A+%22%5BA-Z%5D%5B0-9%5D%5BA-Z%5D+%5B0-9%5D%5BA-Z%5D%5B0-9%5D%22+%7D%0A++++%7D%0A++%7D%0A%7D%0A%0A&output=%7B%0A++%22street_address%22%3A+%221600+Pennsylvania+Avenue+NW%22%2C%0A++%22country%22%3A+%22United+States+of+America%22%2C+%0A++%22postal_code%22%3A+%2220500%22+%0A%7D%0A%0A+)

* [P -> Q](http://ai-schema-lab.phaged.in/?prompt=what+is+2+%2B+2&schema=%7B%0A++%22type%22%3A+%22object%22%2C%0A++%22properties%22%3A+%7B%0A++++%22restaurantType%22%3A+%7B+%22enum%22%3A+%5B%22fast-food%22%2C+%22sit-down%22%5D+%7D%2C%0A++++%22total%22%3A+%7B+%22type%22%3A+%22number%22+%7D%2C%0A++++%22tip%22%3A+%7B+%22type%22%3A+%22number%22+%7D%0A++%7D%2C%0A++%22anyOf%22%3A+%5B%0A++++%7B%0A++++++%22not%22%3A+%7B%0A++++++++%22properties%22%3A+%7B+%22restaurantType%22%3A+%7B+%22const%22%3A+%22sit-down%22+%7D+%7D%2C%0A++++++++%22required%22%3A+%5B%22restaurantType%22%5D%0A++++++%7D%0A++++%7D%2C%0A++++%7B+%22required%22%3A+%5B%22tip%22%5D+%7D%0A++%5D%0A%7D%0A%0A&output=%7B%0A++%22restaurantType%22%3A+%22fast-food%22%2C+%0A++%22total%22%3A+16.99%0A%7D++++%0A%0A)

---
layout: section
---

# JSONSchema Ecosystem
Tools and integrations

---

# AJV - JSON Schema Validation

```typescript
import Ajv from 'ajv';
const ajv = new Ajv();

const schema = {
  type: 'object',
  properties: {
    foo: { type: 'string' },
    bar: { type: 'number' }
  },
  required: ['foo']
};

const validate = ajv.compile(schema);
console.log(validate({ foo: 'abc' })); // true
```

* [Custom Vocabulary](https://medium.com/@damodharanjay/extending-json-schema-with-custom-vocabulary-3a5d3c37d9e1)

---

# Faker Integration

```typescript
import { faker } from '@faker-js/faker';
import { JSONSchemaFaker } from 'json-schema-faker';

const schema = {
  type: 'object',
  properties: {
    name: { type: 'string' },
    email: { 
      type: 'string',
      format: 'email'
    }
  }
};

const fakeData = JSONSchemaFaker.generate(schema);
```

---
layout: section
---

# TypeScript & JSONSchema
Bridging compile-time and runtime

---

# Type Generation

```typescript
import { Value } from '@sinclair/typebox/value';
import { Type, Static, TFunction, TLiteral } from '@sinclair/typebox';

// ================

const BugLifeCycle = Type.Object({
    open: Type.Literal('testing'),
    testing: Type.Literal('closed'),
    closed: Type.Literal('open')
})
```

* [TypeBox Demo](https://stackblitz.com/edit/stackblitz-starters-eiyaw64i?description=Starter%20project%20for%20Node.js,%20a%20JavaScript%20runtime%20built%20on%20Chrome%27s%20V8%20JavaScript%20engine&file=index.ts&title=node.new%20Starter)

---
layout: section
---

# Structured Generation
Structured Generation with JSONSchema

---

# Schema to Grammar

```json
{
  "type": "object",
  "properties": {
    "steps": { "type": "array" }
    },
    "final_result": { "type": "string" }
  },
  "additionalProperties": false
}
```

* [Demo](http://ai-schema-lab.phaged.in/) 
* [Structured Generation](https://medium.com/@damodharanjay/llm-based-structured-generation-using-jsonschema-139568c4f7c9)

---

# Future Directions




- Faster Validators for runtime efficiency - [Typebox compiler](https://github.com/sinclairzx81/typebox?tab=readme-ov-file#typecompiler)
- [From JSONSchema -> to Generic Sampler like LSP](https://blog.dottxt.co/coalescence.html)
- [Reducers and Interpreters for JSONSchema]()

---
layout: center
class: text-center
---

# Thank You!

[PlayGround](http://ai-schema-lab.phaged.in/)
[Github for Playground](https://github.com/damodharanj/structured-output)
