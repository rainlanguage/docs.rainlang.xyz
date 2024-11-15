# Learn DotRain in 10 minutes

## What are `.rain` Files?

`.rain` files are a specialized format for describing **Rainlang fragments**, enabling sharing and auditing in public, decentralized environments such as blockchains. `.rain` files serve as a wrapper/container/medium for Rainlang to be shared and audited simply in a permissionless and adversarial environment such as a public blockchain.

Think of `.rain` files as a building block for creating trustable, reusable code that operates in adversarial environments like public blockchains.

---

## Design Goals

### Primary Goals

1. **Portability and Auditability**:
 `.rain` files ensure code can be shared and verified easily. Dotrain files support "overlay-style" code audits where the entire composition of the metadata is shared.

2. **Iterative Development**:
  Composition natively support iterative development allowing for versioned changes to the rain document so that the *state* of the document can be tracked accross the development cycle. Composition is also amenable to standard development environment such as git.

3. **Structure and Composition**:
  Much as the bytecode syntax of Rainlang is 1:1 with bytecode, a .rain file is 1:1 with a cbor-seq of metadata as described by the metadata spec. That is to say, given a compatible cbor-seq of metadata it is possible to recover an equivalent .rain file to the one that produced the metadata, give or take some incidental concerns such as insignificant whitespace.
  - Example, the following bytecode : 
  ```
  0xff0a89c674ee7874a3005902092f2a20302e2063616c63756c6174652d696f202a2f200a7573696e672d776f7264732d66726f6d20307836363264466436643542364446393445303741363039353439303144333030316332344638353661203078443642333446393764344138436233384430353434644232343143423366333335383636663439300a20200a202063757272656e742d70726963653a20756e69737761702d76332d71756f74652d65786163742d696e707574280a2020202030783833333538396643443665446236453038663463374333324434663731623534626441303239313320307834323030303030303030303030303030303030303030303030303030303030303030303030303036200a20202020310a20202020307833333132386138664331373836393839376463453638456430323664363934363231663646446644205b756e69737761702d76332d696e69742d636f64655d0a202020205b756e69737761702d76332d6665652d6d656469756d5d0a2020292c0a20205f3a20626c6f636b2d6e756d62657228292c0a202066696e616c2d616d6f756e743a2063616c6c3c323e28292c0a202066696e616c2d726174696f3a20302e303030343b0a0a2f2a20312e2068616e646c652d696f202a2f200a3a3b0a0a2f2a20322e206765742d6f75747075742d616d6f756e742d70726f64202a2f200a6d61782d6f75747075743a203130303b011bff13109e41336ff20278186170706c69636174696f6e2f6f637465742d73747265616d
  ``` 
  corresponds to the following rainlang document : 
  ```
  /* 0. calculate-io */ 
  final-amount final-ratio: 100 1;

  /* 1. handle-io */ 
   :;
  ```
  and vice-versa.

### Secondary Goals

- Adopt **common conventions** for usability.
- Avoid unnecessary complexity to ensure extensibility.

---

## Allowed Characters
ONLY ascii printable and whitespace characters are allowed in .rain files.
As a regex this is `[\s!-~]`.
utf8 is backwards compatible with ascii so the codepoints are the same in either encoding.

### Additional Characters
- `#` binds a name to a fragment
- `!` is an elided fragment
- `.` delimits paths in a namespace
- `'` quotes names
- `@` imports .rain/metadata
- `---` separates .rain documents within a single file

## Structure of `.rain` files
`.rain` documents can be delimited by `---`. The --- can also be used to start/end a .rain file if it is embedded directly in some other file like a yaml file.
- This allows defining and importing several namespaces within a single physical file.
```     
#a 1
---
#b 2
```
- As --- defines entirely different .rain documents, it allows names to be reused in a single file, where they would otherwise have been disallowed due to collision/shadowing rules.
```
#a 1
---
#a 2
```

## Named Fragements
The top level of the .rain file is only named fragments, named fragments take the form of `#<name><whitespace><fragment>.`, fragments are useful.
Example : 
```
#length 3
#width 4
```
Fragements can also be nested : 
```
#slow-three add(1 3)
#multiline-example
a _: slow-three 3
_: mul(a 5)
```
Fragements can also be bound to another fragment : 
```
#a 1
#b a
```
Names can point to elided fragments which are then later bound by the tooling. Fragment elision syntax is `!<message>`.The message is the error message to show if the fragment remains elided at the time it needs to be bound.
```
#a !must bind a to calculate rate of change
#b mul(sub(1000000 now()) a)
```
- For a deatiled explaination refer [fragments](https://github.com/rainlanguage/specs/blob/main/dotrain.md#named-fragments) from the offcial documentation.

---

## Namespaces
Namespaces are just branches on a tree of names. `.rain` document use `.` to delimit names as it often brings a kind of "path", "drilling down" or "chaining" feeling in the wild.Namespaces are the standard way to handle name collisions in basically every language and data structure.

Imports take the form of `@<namespace><whitespace><hash>` OR `@<hash>`. Example : 
```
#a 1
---
#a 2
/* import the above document */
@b 0x..
```

If the namespace is ommitted that means "import into the current namespace".
`.` also means "current namespace" where it needs to, or benefits from, being made explicit, much like "current directory" in a file system.

Any namespace specified in an import will be prepended to all the names found in the imported document. Example:
```
#pi
/* pi is roughly 3 */
3
---
/* import .rain doc above */
@math 0xdeadbeef...
#two-pies
add(math.pi math.pi)
```
This expands to : 
```
#math.pi
/* pi is roughly 3 */
3
#two-pies
add(math.pi math.pi)
```

### Rebindings Names

Imports allow explicit dynamic (re)binding of values upon import. Rebinding enables imported fragments to adapt to the specific needs of the importing file by overriding or linking their values to local definitions.

#### Example 1: Default Behavior

```rain
#pi
3
#two-pies
add(pi pi)
---
#pi
4
@x 0x..
```

This will expand to:

```rain
#pi
4
#x.pi
3
#x.two-pies
add(x.pi x.pi)
```

The local `#pi` is redefined as `4` in the second document, while the imported namespace `x` retains its original definition of `pi` as `3`. As a result, `x.two-pies` evaluates to `add(3 3)` because it refers to `x.pi`.

---

#### Example 2: Rebinding `pi` Directly

```rain
#pi
3
#two-pies
add(pi pi)
---
#pi
4
@x 0x.. pi 4
```

This will expand to:

```rain
#pi
4
#x.pi
4
#two-pies
add(x.pi x.pi)
```

Here, the rebind directive `pi 4` explicitly overwrites the value of `x.pi` with `4`. Consequently, `x.two-pies` evaluates to `add(4 4)` because of the rebinding.

---

#### Example 3: Using Local `pi`

```rain
#pi
3
#two-pies
add(pi pi)
---
#pi
4
@x 0x.. pi pi
```

This will expand to:

```rain
#pi
4
#x.pi
pi
#two-pies
add(x.pi x.pi)
```

In this case, the rebind directive `pi pi` links `x.pi` to the local `#pi`, which is defined as `4`. Therefore, `x.two-pies` evaluates to `add(4 4)`.

---

#### Example 4: Explicit Path Rebinding

Rebinding Names

Imports allow explicit dynamic (re)binding of values upon import. Rebinding enables imported fragments to adapt to the specific needs of the importing file by overriding or linking their values to local definitions.

Example 1:
```
#pi
3
#two-pies
add(pi pi)
---
#pi
4
@x 0x..
```
This will expand to:
```
#pi
4
#x.pi
3
#x.two-pies
add(x.pi x.pi)
```
The local #pi is redefined as 4 in the second document, while the imported namespace x retains its original definition of pi as 3. As a result, x.two-pies evaluates to add(3 3) because it refers to x.pi.

Example 2: Rebinding pi Directly
```
#pi
3
#two-pies
add(pi pi)
---
#pi
4
@x 0x.. pi 4
```
This will expand to:
```
#pi
4
#x.pi
4
#two-pies
add(x.pi x.pi)
```
Here, the rebind directive pi 4 explicitly overwrites the value of x.pi with 4. Consequently, x.two-pies evaluates to add(4 4) because of the rebinding.

Example 3: Using Local pi
```
#pi
3
#two-pies
add(pi pi)
---
#pi
4
@x 0x.. pi pi
```
This will expand to:
```
#pi
4
#x.pi
pi
#two-pies
add(x.pi x.pi)
```
In this case, the rebind directive pi pi links x.pi to the local #pi, which is defined as 4. Therefore, x.two-pies evaluates to add(4 4).

Example 4: Explicit Path Rebinding
```
#pi
3
#two-pies
add(pi pi)
---
#pi
4
@x 0x.. .pi pi
```
This will expand to:
```
#pi
4
#x.pi
pi
#two-pies
add(x.pi x.pi)
```
Here, the . prefix explicitly specifies the current namespace for rebinding. x.pi is rebound to the local #pi, which is 4, making x.two-pies evaluate to add(4 4).

---

### Importing Words
Words in .rain files refer to specific operations or functions that can be executed. These words are tied to either an internal interpreter or an external contract and must be explicitly imported to use.

#### Example 1: Basic Import of Words
```
/* import words */
@ 0x123...

/* use add word */
#a add(1 2)
```
Here:

The `@ 0x123...` imports metadata that defines the word `add`.

The fragment `#a` uses the `add` operation to compute `1 + 2`.

---

#### Example 2: Handling Word Collisions

If words from different interpreters are imported, they can cause ambiguity. For example:
```
/* import words from interpreter A */
@ 0x123...
#a add(1 2)
---
/* import words from interpreter B */
@ 0x456...

/* import document A */
@docA 0x...

/* ambiguous use of add */
#b add(3 4)
```
This setup introduces ambiguity because both interpreters define `add`, and it is unclear which one is used in `#b`. To resolve this, words can be elided:
```
/* import words from interpreter A */
@wordsA 0x123...

/* import words from interpreter B */
@wordsB 0x456...

/* elide words from interpreter A */
@docA 0x..
 wordsA !

/* use add from interpreter B */
#b add(3 4)
```
Here:

Words from `interpreter A` are explicitly elided using `wordsA !`.

`#b` now uses the `add` word from `interpreter B` without ambiguity.

---

#### Example 3: Namespacing External Words

External words (from contracts) follow the same namespace rules as other fragments. For clarity and to avoid collisions, they should be placed in dedicated namespaces:
```
/* import external words under a namespace */
@extern 0x789...

/* use an external word */
#result extern.someWord(5 10)
```
Here:

`extern.someWord` explicitly calls the external operation `someWord` under the namespace `extern`.

This prevents conflicts with other similarly named operations.

---
#### Summary

Importing words allows .rain files to use operations defined in interpreters or external contracts. Care must be taken to:

Avoid ambiguity by eliding conflicting words.

Use namespaces for clarity, especially with external words.

By managing imports and namespaces effectively, .rain files remain modular and robust.

---

## Summary

- `.rain` files are a powerful format for creating auditable, composable code in decentralized systems.
- They prioritize structure, clarity, and extensibility.
- With features like named fragments, namespaces, and imports, `.rain` files are easy to modularize and integrate into workflows.

Explore `.rain` files to build robust, trustable blockchain applications!


