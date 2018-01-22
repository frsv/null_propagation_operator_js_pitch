@title[Introduction]
# Null Propagation Operator in JS

---
@title[Resources]

## Resources

* [Ponyfoo "Null propagation Operator" article](https://ponyfoo.com/articles/null-propagation-operator)
* [GitHub: Proposal document](https://github.com/claudepache/es-optional-chaining)
* [Babel implementation](https://github.com/babel/babylon/issues/328)

---
@title[What happened]

## What happened?

Null Propagation Operator in JavaScript has entered stage-2. 

> The TC39 Committee (Ecma Internation Technical Committee 39) has a 5 stage process from staging-0 to staging-4
> A stage-2 is the phase, where initial spec text is written and babel plugin is released.

---
@title[So what]

## So what?

This operator is a real timesaver and it provides elegant syntax and fail-free approach as an alternative to very long null checks.

---
@title[Why]

## Why?

Very often we have to extract value from very deep object:

```javascript
  const tag = state.account.clan.tag
```

+++
@title[Pure JS example]

Of course, we try to avoid such situations, but clan might not be an object. Or account might be not an object. Or even state for some reason might not be an object! So, (it's very far-fetched example), we have to null-check all this thing:

```javascript
  const tag = (
    state &&
    state.account &&
    state.account.clan &&
    state.account.clan.tag ||
    'DEFAULT'
  )
```

+++
@title[Lodash example]

It's ugly, so we use a `lodash` to write simplier code:

```javascript
  import { get } from 'lodash'
  const tag = get(state, ['account', 'clan', 'tag'], 'DEFAULT')
```

+++
@title[Null propagation operator example]

#### Null propagation operator takes the stage 💥

Null Propagation operator natively provides an operator `?.` which allows to handle such cases more conveniently:

```javascript
  const tag = state?.account?.clan?.tag || 'DEFAULT'
```

---
@title[Idea and appearance]

## Idea

The idea is not new - the null propogation first appeared in C# 6.0 in October 2014:

```c#
  double minPrice = 0;
 
  if (product != null
      && product.PriceBreaks != null
      && product.PriceBreaks[0] != null)
  {
    minPrice = product.PriceBreaks[0].Price;
  }

  var minPrice = product?.PriceBreaks?[0]?.Price;
```

@[1-9](Before)
@[10](After null propagation operator the code turns into shiny fancy)

---
@title[Syntax]

## Syntax

The null propagation operator, or The Optional Chaining operator, as it said in proposal document, is marked as `?.`. It has three possible positions:

```javascript
  obj?.prop       // optional static property access
  obj?.[expr]     // optional dynamic property access
  func?.(...args) // optional function or method call
```

---
@title[How it works]

## How it works? 🤔

If the operand at the left-hand side of the `?.` operator evaluates to undefined or null, the expression evaluates to undefined. This concept is called _short-circuiting_. Otherwise the targeted property access, method or function call is triggered normally.

---
@title[Basic examples]

## Basic examples

```javascript
  const lowercasedTag = (state) => state?.account?.clan?.tag?.toLowerCase?.()
  lowercasedTag() // undefined, cause of state?.
  lowercasedTag({}) // undefined, cause of account?.
  lowercasedTag({ account: { clan: { tag: 1 } } }) // undefined, since number doesn't have toLowerCase
  lowercasedTag({ account: { clan: { tag: 'BOUHA' } } }) // bouha (sic!)
```

@[1]
@[2-5]

---
@title[Stacking example]

## Stacking

Since short-circuiting, when triggered, skips not only the current property access, but also the whole chain of the right side, we can stack null propagation operators together:

```javascript
  const x = a?.b[3].c?.(x).d

  const x = a === null ? undefined : a.b[3].c === null ? undefined : a.b[3].c(x).d
```

@[1](Null propagation operator example)
@[3](Desugaring)

---
@title[Try it by yourself]

## How to try it by yourself

```bash
  npm install --save-dev babel-plugin-transform-optional-chaining
```

In `.babelrc`:
```json
  {
    "plugins": ["transform-optional-chaining"]
  }
```

---
@title[What remains to do]

## Todo

* [x] [Initial spec text](https://tc39.github.io/proposal-optional-chaining/) (stage-2)
* [x] [Babel plugin](https://github.com/babel/babel/pull/5813) (stage-2)
* [ ] Finalize and reviewer signoff for spec text (stage-3)
* [ ] Test262 acceptance tests (stage-4)
* [ ] tc39/ecma262 pull request with integrated spec text (stage-4)
* [ ] Reviewer signoff (stage-4)

---
@title[The end]

# 🦄
## The end