# Front Matter Scenarios
[Scenarios](https://github.com/rainlanguage/specs/blob/main/ob-yaml.md#front-matter-scenarios) in dotrain files provide a hierarchical configuration structure that generates specific rainlang instances through binding composition. This system allows developers to define multiple execution contexts for the same rainlang, enabling production deployment, testing, and simulation from a single configuration.

## Scenario Structure

### Basic Anatomy
```yaml
scenarios:
  <root-scenario>:
    network: <network-reference>
    deployer: <deployer-reference>
    orderbook: <orderbook-reference>
    bindings: <binding-definitions>
    scenarios: <nested-scenarios>
```

### Hierarchy Levels

1. **Root Scenario**
   - Defines base network context
   - Specifies core infrastructure references
   - Contains global bindings

2. **Child Scenarios**
   - Inherit parent configurations
   - Override or extend parent bindings
   - Can contain further nested scenarios

3. **Leaf Scenarios**
   - Final execution contexts
   - Specific use cases (prod, test, simulation)
   - Complete binding resolution

## Binding System

### Types of Bindings

1. **Infrastructure Bindings**
   ```yaml
   network: base
   deployer: base
   orderbook: base
   ```
   - Define operational context
   - Must reference valid configurations
   - Inherited by child scenarios

2. **Contract Bindings**
   ```yaml
   bindings:
     orderbook-subparser: "0x662dFd6d..."
   ```
   - Reference on-chain contracts
   - Used for external interactions
   - Usually global to scenario tree

3. **Parameter Bindings**
   ```yaml
   bindings:
     limit-ratio: 0.0004
     output-amount: 100
   ```
   - Define strategy parameters
   - Can be overridden in child scenarios
   - Type-safe according to rainlang spec

## Execution Contexts

### Production Context
```yaml
prod:
  bindings:
    get-output-amount: '''get-output-amount-prod'
```
- Live deployment configuration
- Real network interactions
- Production-ready parameters

### Simulation Context
```yaml
plot:
  runs: 100
  bindings:
    get-output-amount: '''get-output-amount-plot'
```
- Strategy visualization
- Multiple simulation runs
- Performance analysis

### Backtest Context
```yaml
backtest:
  runs: 1
  blocks:
    range: [21530860..21531860]
    interval: 100
  bindings:
    get-output-amount: '''get-output-amount-prod'
```
- Historical performance testing
- Block range specification
- Interval-based analysis


## Testing & Development

### Local Testing
```yaml
test:
  runs: 10
  bindings:
    network: localhost
    deployer: test
```
- Use local network
- Quick iteration
- Rapid feedback

### Simulation Setup
```yaml
simulate:
  runs: 1000
  interval: 10
  bindings:
    price-feed: mock
```
- Multiple scenarios
- Parameter sweeps
- Performance analysis
