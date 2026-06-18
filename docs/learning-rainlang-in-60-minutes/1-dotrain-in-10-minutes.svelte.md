# Learn dotrain in 10 minutes

## What is dotrain?

dotrain (`.rain`) is a format for creating and sharing smart contract logic in decentralized environments. It serves as a container for Rainlang fragments, making code easily shareable and auditable in permissionless, adversarial environments like public blockchains.

Think of dotrain files as building blocks for creating trustworthy, reusable smart contract logic that anyone can verify and understand.

## Key Features

### 1. Composability and Auditability

dotrain files support an "overlay-style" audit system where code composition and metadata can be shared at a social layer. This creates an audit trail of audit trails, allowing participants to build and verify trust incrementally.

### 2. Version Control Ready 

dotrain natively supports iterative development practices by tracking the exact state of code over time. It works seamlessly with standard development tools like git while adding blockchain-specific benefits:

- Clear tracking of code changes 
- Unambiguous references to specific states
- Compatibility with standard development workflows

### 3. Bidirectional Compatibility

A key feature of dotrain is its 1:1 relationship with on-chain bytecode. This means:

```rain
# In a .rain file
amount price: 100 2;
```

Can be deployed on-chain and later recovered to an equivalent .rain file without losing meaning. This bidirectional compatibility ensures transparency and auditability.

### 4. Structured Imports

dotrain files can reference other code and metadata using cryptographic hashes:

```rain
# Import words from a specific interpreter
@ 0x123...

# Import a shared utility
@utils 0x456...
```

This hash-based import system ensures code integrity while enabling composition and reuse.

## Design Philosophy

dotrain prioritizes:

1. **Simplicity**: The format is minimal yet expressive
2. **Portability**: Code can be shared and verified easily
3. **Security**: Built for adversarial blockchain environments
4. **Tooling Support**: Easy to build automated tools and analysis

# dotrain Syntax and Structure

## Character Set

dotrain files use a simple character set:
- Only ASCII printable characters and whitespace are allowed (`[\s!-~]` in regex)
- UTF-8 encoding is supported (as it's backwards compatible with ASCII)

### Special Characters

dotrain uses several special characters with specific meanings:
- `#` - Defines a named fragment
- `!` - Marks an elided (placeholder) fragment
- `.` - Separates namespace paths
- `'` - Quotes names for explicit references
- `@` - Imports other dotrain files or metadata
- `---` - Separates multiple documents within a file

## Document Structure 

### Multiple Documents
dotrain files can contain multiple documents separated by `---`. This allows you to:
- Define multiple independent code sections
- Import several namespaces in one file
- Reuse names across different documents

Example of multiple documents:
```rain
#price 100
---
#amount 50
```

This separation lets you reuse names that would otherwise conflict:
```rain
#token-a "USDC"
---
#token-a "DAI"  # Valid because it's in a different document
```

### Named Fragments

The basic building blocks of dotrain are named fragments. Each fragment:
- Starts with `#`
- Has a unique name
- Contains a value or expression

Simple fragment examples:
```rain
#decimals 18
#symbol "RAIN"
```

Fragments can reference other fragments:
```rain
#base-amount 1000
#total-supply mul(base-amount 1000000)
```

Multi-line fragments are supported:
```rain
#calculate-price
amount: 100,
price: div(total supply),
value: mul(amount price);
```

### Elided Fragments

You can create placeholder fragments that must be defined later using `!`:
```rain
#start-time !must set a valid start timestamp
#duration mul(start-time 86400)  # 24 hours in seconds
```

## Namespaces

Namespaces help organize and avoid naming conflicts in your code. They use `.` for path separation, similar to file systems or package names in other languages.

### Importing with Namespaces

Import syntax: `@<namespace> <hash>` or just `@<hash>`

Example importing into a namespace:
```rain
#constants
max-supply: 1000000,
decimals: 18;
---
@token 0x123...  # Import previous document into 'token' namespace
#configure
supply: token.constants.max-supply,
scale: exp(10 token.constants.decimals);
```

When no namespace is specified, imports go into the current namespace. The `.` character can explicitly reference the current namespace when needed.

### Namespace Resolution

Imports automatically prepend the namespace to imported names:

```rain
#config
price: 100;
---
@market 0xabc...
#total mul(market.config.price 50)
```

This helps prevent naming collisions while keeping code organized and readable.

# Rebinding Names in dotrain

## Understanding Rebinding

Rebinding is a powerful feature in dotrain that allows you to modify imported fragments dynamically. When importing code, you can:
- Override values from imported fragments
- Link imported values to local definitions
- Resolve naming conflicts elegantly

## Basic Rebinding Examples

### 1. Default Import Behavior

Without explicit rebinding, imported fragments maintain their original values:

```rain
# Original document
#pi 3
#two-pies add(pi pi)
---
# Importing document
#pi 4
@x 0x..
```

This expands to:
```rain
#pi 4             # Local definition
#x.pi 3           # Original imported value
#x.two-pies add(x.pi x.pi)  # Uses imported pi (3)
```

The imported code maintains its original value (`3`) while local code uses the new value (`4`).

### 2. Direct Value Rebinding 

You can explicitly override imported values:

```rain
# Original document
#pi 3
#two-pies add(pi pi)
---
# Importing document with rebinding
#pi 4
@x 0x.. pi 4  # Rebind pi to 4
```

Expands to:
```rain
#pi 4
#x.pi 4                    # Overridden to 4
#x.two-pies add(x.pi x.pi) # Uses new value (4)
```

### 3. Reference Rebinding

You can bind imported values to local references:

```rain
# Original document
#pi 3
#two-pies add(pi pi)
---
# Importing document with reference rebinding
#pi 4
@x 0x.. pi pi  # Bind to local pi
```

Expands to:
```rain
#pi 4
#x.pi pi                   # Links to local pi
#x.two-pies add(x.pi x.pi) # Uses local pi (4)
```

### 4. Explicit Path Rebinding

Use `.` to explicitly specify namespace paths:

```rain
# Original document
#pi 3
#two-pies add(pi pi)
---
# Importing document with explicit path
#pi 4
@x 0x.. .pi pi  # Explicitly bind to current namespace
```

Expands to:
```rain
#pi 4
#x.pi pi                   # Explicitly linked to local pi
#x.two-pies add(x.pi x.pi) # Uses local pi (4)
```

## Key Points

- Rebinding happens during import with `@namespace hash name value`
- You can bind to literal values or reference other fragments
- Use `.` for explicit namespace paths
- Rebinding affects all uses of the rebound name in imported code
- Original code remains unchanged - rebinding only affects the importing context

This powerful feature allows you to:
- Customize imported code for your specific needs
- Handle naming conflicts gracefully
- Create flexible, reusable code components

Here's the edited and restructured section on importing words in dotrain:

# Working with Words in dotrain

## Understanding Words

Words are the core operations available in dotrain, similar to functions in other languages. They can come from:
- Internal interpreters (built-in operations)
- External contracts (custom operations)

## Basic Word Import

The simplest way to import words is directly:

```rain
# Import interpreter words
@ 0x123...

# Use imported word 'add'
#result add(1 2)
```

This makes all words from the interpreter at `0x123...` available in your code.

## Managing Word Collisions

### The Problem

When importing words from multiple sources, naming conflicts can occur:

```rain
# Import first interpreter
@ 0x123...
#calc-a add(1 2)
---
# Import second interpreter
@ 0x456...

# PROBLEM: Which 'add' are we using?
#calc-b add(3 4)  # Ambiguous!
```

### The Solution: Namespaced Imports

Use dedicated namespaces to avoid conflicts:

```rain
# Import interpreters with namespaces
@math-v1 0x123...
@math-v2 0x456...

# Elide first interpreter's words
@imported-code 0x789...
  math-v1 !

# Now unambiguously uses math-v2's add
#result add(3 4)
```

## External Words

External words (from other contracts) should always use explicit namespacing:

```rain
# Import external operations
@chainlink 0x789...

# Use external word with clear namespace
#price chainlink.getPrice("ETH-USD")
```

Best practices:
- Always namespace external words
- Keep external operations in dedicated imports
- Be explicit about which operations come from where

## Key Concepts

### Word Resolution

1. Words without namespaces come from the current interpreter
2. Namespaced words like `extern.word` come from their specified source
3. Elided words become unavailable and must be explicitly rebound

### Import Best Practices

- Use clear, descriptive namespaces
- Keep interpreter words separate from external words
- Document word sources in comments
- Resolve ambiguities through elision and explicit namespacing

Example showing all concepts:

```rain
# Import internal words
@math 0x123...

# Import external price feed
@oracle 0x456...

# Define calculation using both
#token-price
  base-price: oracle.getPrice("TOKEN-USD"),
  fee: math.mul(base-price 0.003),  # 0.3% fee
  final: math.add(base-price fee);
```

This structure keeps code clear and maintainable while avoiding naming conflicts.


