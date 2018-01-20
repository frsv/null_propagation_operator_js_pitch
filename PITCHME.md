
# Null Propagation Operator in JS

---

## Resources

* Ponyfoo article: [https://ponyfoo.com/articles/null-propagation-operator](https://ponyfoo.com/articles/null-propagation-operator)
* Proposal document on GitHub: [https://github.com/claudepache/es-optional-chaining](https://github.com/claudepache/es-optional-chaining)
* Babel implementation: [https://github.com/babel/babylon/issues/328](https://github.com/babel/babylon/issues/328)

---

## What happened?

Null Propagation Operator in JavaScript has entered stage-1. The TC39 Committee (Ecma Internation Technical Committee 39) has a 5 stage process from staging-0 to staging-4 and a stage-1 is the *proposal* phase.

---

## So what?

This operator is a real timesaver and it provides elegant syntax and fail-free approach as an alternative to very long null checks.

---

## Why?

Very often we have to extract value from very deep object:

```javascript
  const tag = state.account.clan.tag
```

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

It's ugly, so we use a `lodash` to write simplier code:

```javascript
  import { get } from 'lodash'
  const tag = get(state, ['account', 'clan', 'tag'], 'DEFAULT')
```

Null Propagation operator natively provides an operator `?.` which allows to handle such cases more conveniently:

```javascript
  const tag = state?.account?.clan?.tag || 'DEFAULT'
```

---

## Idea

The idea is not new - the null propogation first appeared in C# 6.0 in October 2014 and it was great improvement in laguage, as construction such following:

```c#
  double minPrice = 0;
 
  if (product != null
      && product.PriceBreaks != null
      && product.PriceBreaks[0] != null)
  {
    minPrice = product.PriceBreaks[0].Price;
  }
```

With null propagation operator turns into shiny fancy:

```c#
  var minPrice = product?.PriceBreaks?[0]?.Price;
```