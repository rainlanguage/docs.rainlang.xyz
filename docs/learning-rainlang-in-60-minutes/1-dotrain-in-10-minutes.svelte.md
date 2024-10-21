# Learn DotRain in 10 minutes

DotRain (.rain) is a portable, extensible and composable format for describing Rainlang fragments. .rain files serve as a wrapper/container/medium for Rainlang to be shared and audited simply in a permissionless and adversarial environment such as a public blockchain.

## Key Objectives

1. **Facilitating Audits**:
   - `.rain` files enable **"overlay style" audits**, which allow metadata composition to be shared and audited at a social layer.
   - This helps create a layered **audit trail**, critical for building incremental **trust models**.
   - Participants can gain or lose trust based on the **on-chain artifacts** they produce, which justify their reputation.

2. **Supporting Iterative Development**:
   - The `.rain` file format supports iterative development, allowing for changes like **"this is exactly that with only these changes"**.

3. **1:1 with CBOR-Seq Metadata**:
   - A `.rain` file is 1:1 with a **CBOR sequence** of metadata, as defined in the Rainlang metadata spec.
   - Given a compatible CBOR-seq, an equivalent `.rain` file can be recovered, ensuring **metadata integrity** and minimizing formatting concerns.

## Properties

1. **Immutable Data Imports**:
   - `.rain` files must support **unambiguous, immutable imports** to ensure auditability.
   - Mutable or ambiguous imports lead to unreliable behavior, such as **"works on my machine"** issues, which tooling cannot mitigate easily.

2. **Tooling Support**:
   - `.rain` files have **extension points** for tools to provide comprehensive **code analysis**.
   - This ensures readers can detect malicious behavior from both bytecode and metadata authors.

3. **Location-Independent Imports**:
   - Imports are **location-independent**, ensuring content integrity via hashing, similar to blocks in a blockchain or data in IPFS.
   - This allows content to be handled **peer-to-peer (p2p)**, decoupled from client, storage, or transmission specifics.

4. **Namespace Support**:
   - `.rain` files support **namespacing** to resolve ambiguities when different entities share the same human-readable name.

5. **Minimal Document Structure**:
   - The `.rain` document structure should be **minimal** to ensure **stability, extensibility**, and simplicity in tooling implementations.
   - The format should avoid hardcoding meanings into syntax that might change or become deprecated in the future.


## Key Takeaways

`.rain` files are a robust, audit-friendly format for describing Rainlang fragments, designed with transparency, auditability, and iterative development in mind. By supporting immutable imports, tooling integration, location-independent data, and a minimal structure, `.rain` files ensure long-term extensibility and future-proofing in decentralized environments.
