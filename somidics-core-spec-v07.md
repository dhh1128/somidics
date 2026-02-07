<!--
MARKUP STYLE GUIDE FOR AI EDITORS:

This document follows strict logical markup conventions. Please preserve these:

1. BOLD TEXT: Use bold ONLY when introducing and defining a new term inline.
   Example: "A somidion is a physical distinguishing mark on the body."
   
2. ITALIC TEXT: Use *italic* for:
   - Subsequent references to previously defined terms
   - Emphasis of key concepts
   - Variable names or placeholders in prose
   
3. HEADERS: Use markdown headers (##, ###, ####) to create document structure.
   DO NOT use bold as a substitute for headers.
   
4. LOGICAL MARKUP: Always use appropriate semantic markup:
   - `code` for: code snippets, variable names, hex values, function names, file paths
   - bold for: first definition of terms only
   - *italic* for: emphasis and term references
   - > blockquotes for: important notes or warnings (sparingly)
   
5. AVOID: 
   - Excessive bolding for emphasis or "shouting"
   - Using bold where a header would be more appropriate
   - Visual markup where logical markup is better
   
6. PRESERVE: This document's clean outline structure for navigation and TOC generation

When making edits, maintain these conventions. The document should feel calm and 
professional, not visually noisy. Let the structure speak through headers, not bold text.
-->

# Somidics Core Specification v0.7

## Overview

Somidics is a human-verifiable, equipment-free biometric identification system based on naturally occurring or intentional marks on the human body.

Version: 0.7 (Feb 2026)
Previous versions: 0.6, 0.5, 0.4, 0.3, 0.2, 0.1

## 1. Introduction

### 1.1 What is somidics?

Somidics is a human-verifiable, equipment-free identification approach based on distinctive marks on the human body. Unlike traditional biometrics (fingerprints, iris scans, facial recognition), somidics requires no specialized hardware&mdash;just ordinary human observation and judgment.

The fundamental unit is a **somidion** (/so&#650;&#712;m&#618;di&#601;n/): a mark on a person's body that has a specific location, is persistent over time, is characteristic rather than incidental, and can be recognized with ordinary senses based on a simple description. Examples include moles, scars, birthmarks, tattoos, or missing tissue. A somidic system encodes these marks into compact identifiers that can be verified by showing the mark to another person.

Somidics fills a specific niche: contexts where equipment-free verification is more important than precision, where human judgment is acceptable, and where "good enough" identification (~1-in-3,000 to 1-in-60-million discrimination depending on configuration) serves the need. Target applications include refugee identification in camps without infrastructure, rural healthcare patient matching, fraud prevention for credit cards, and backup identification when biometric systems fail.

While this specification focuses on human identification, the somidic framework can be extended to identify animals (pets, livestock, wildlife) as well, though animal-specific encoding is outside the scope of the current version.

### 1.2 Quick Example

Consider Alice, who enrolls in a somidic-protected credit card system:

1. Enrollment: Alice has a small raised mole on her left wrist and no visible tattoos anywhere on her body. A brief interview captures these facts. The system encodes this as: `@+147293-4f`

2. The encoding: 
   - `@+147293` represents "single raised grain-sized natural mark on left wrist"
   - `-4f` represents "no tattoos anywhere in public zones"

3. Verification: When Alice uses her card, the terminal displays: "Show small raised mark on left wrist. Verify no visible tattoos." The cashier glances at Alice's wrist (sees the mole &#10003;), then quickly scans her visible skin&mdash;hands, face, neck, ears (no tattoos &#10003;)&mdash;and approves the transaction in ~45 seconds.

4. Security: A thief who steals Alice's card faces a problem. They need a grain-sized raised mark on their left wrist (estimated ~1-in-3,300 people have a matching mark in that specific zone with those specific attributes) AND no visible tattoos anywhere (only ~60% of adults). Combined: roughly 1-in-5,000 to 1-in-10,000 chance of a random person matching. Much harder than guessing a 4-digit PIN, and the thief can't fake the absence of tattoos if they have any.

This example shows somidics' core value: equipment-free verification with meaningful fraud prevention, at the cost of imperfect precision.

### 1.3 Key Design Principles

Equipment-free: No cameras, scanners, or specialized hardware. Works anywhere, anytime, with zero infrastructure cost.

Human-verifiable: Any person can check a match using ordinary vision (and occasionally touch). No training required beyond basic instructions.

Privacy-preserving through fuzziness: Size boundaries ("grain-sized" vs "fingernail-sized"), texture judgments ("raised" vs "flush"), and zone boundaries are intentionally fuzzy. This prevents somidics from becoming overly strong legal proof while still providing useful discrimination.

Stackable security: One somidion provides casual fraud prevention (estimated ~1-in-3,000). Adding negative assertions (contraquants) improves this significantly. Multiple somidions can reach 1-in-billions discrimination when needed.

Stable over decades: The encoding excludes unstable attributes like color (which fades) to ensure somidics remain valid for 20+ years without reissuance.

Verifier discretion: Optional components (contraquants) provide stronger identification, but verifiers choose whether to check them based on transaction value, time constraints, and fraud risk. The system degrades gracefully.

### 1.4 What Somidics Is Not

Somidics is not a replacement for strong biometrics in high-security contexts. It is not suitable for:
- Legal proof of identity in courtroom settings (too fuzzy)
- High-throughput scenarios like airport security lines (too slow&mdash;human verification takes 30-120 seconds)
- Situations requiring cryptographic-strength uniqueness guarantees

Somidics is not a database of body characteristics. It encodes only what the holder chooses to share, stored in their credential (card, digital wallet), not in a central database.

### 1.5 How to Read This Specification

For implementers: Read sections 2-5 (encoding, validation, decoding) in order. Section 4 (Validation and Conformance) is particularly critical&mdash;it defines exactly when to accept or reject inputs.

For integration architects: Skim section 1 (introduction), read section 6 (notation), and review Appendix A (examples). Section 7 (security properties) explains discrimination levels for different configurations.

For researchers: Section 1.3 (design principles) and the referenced design rationale document explain why specific encoding choices were made.

For casual readers: This introduction (section 1) and the example gallery (Appendix A) provide the core concepts without bit-level details.

## Core Concepts

### Information Flow

Quants (Somidion Encoding):
```
Physical mark → 13-bit encoding → +checksum → Decimal → In notation
Somidion     → Somid           → 18-bit  → **Quant**   → Somidic
(on body)      (0-8191)         (0-262143) (6 digits) (@+147293)
```

Contraquants:
```
Absence claim  → 8-bit encoding → Hex representation → In notation
**Contrasomidion** → Contrasomid    → Contraquant       → Somidic
(not on body)    (0-255)          (2 hex digits)      (@-41)
```

Combined Notation:
```
@+147293-41
 │ │     │
 │ │     └─ Contraquant: "No tattoos on hands+wrists"
 │ └─────── Quant: "Mole on left wrist"
 └───────── Somidic prefix (always present)
```

### Key Properties
- Equipment-free: No cameras, scanners, or specialized hardware required
- Human-verifiable: Ordinary person can check a match without training
- Memorable: Holder remembers "mole on left wrist" not "147293"
- Privacy-preserving: Fuzzy matching prevents overly strong identification
- Optional discrimination: Contraquants provide stronger ID at verifier's discretion
- Entropy target: ~12 bits per somidion, 5-20× boost with contraquants

## Quant Encoding (Somidions)

### Somid Structure (13 bits)

A **somid** is a 13-bit value encoding a somidion's characteristics:

- Bits 0-5: **Zone** (6 bits) — Body location (64 possible zones, 48 defined)
- Bits 6-7: **Type** (2 bits) — Category of mark
- Bits 8-9: **Size** (2 bits) — Measured by longest dimension
- Bits 10-11: **Texture** (2 bits) — Surface characteristic or anomaly subtype
Bit 12: Multiplicity/Special (1 bit) - Context-dependent meaning

### Zone Encoding (Bits 0-5)

#### Public Human Zones (48 defined zones)

##### Fingers (20 zones): 0-19
Each finger is subdivided into ring portion (lower half with knuckle) and upper portion (upper half toward tip):
- 0: Left thumb - ring portion
- 1: Left thumb - upper portion
- 2: Left index - ring portion
- 3: Left index - upper portion
- 4: Left middle - ring portion
- 5: Left middle - upper portion
- 6: Left ring - ring portion
- 7: Left ring - upper portion
- 8: Left pinky - ring portion
- 9: Left pinky - upper portion
- 10: Right thumb - ring portion
- 11: Right thumb - upper portion
- 12: Right index - ring portion
- 13: Right index - upper portion
- 14: Right middle - ring portion
- 15: Right middle - upper portion
- 16: Right ring - ring portion
- 17: Right ring - upper portion
- 18: Right pinky - ring portion
- 19: Right pinky - upper portion

##### Hands (8 zones): 20-27
Each palm/back is subdivided into thumb-side and pinky-side:
- 20: Left palm - thumb side
- 21: Left palm - pinky side
- 22: Left back - thumb side
- 23: Left back - pinky side
- 24: Right palm - thumb side
- 25: Right palm - pinky side
- 26: Right back - thumb side
- 27: Right back - pinky side

##### Arms (6 zones): 28-33
- 28: Left upper arm
- 29: Right upper arm
- 30: Left forearm
- 31: Right forearm
- 32: Left wrist
- 33: Right wrist

##### Face (10 zones): 34-43
- 34: Forehead
- 35: Left cheek
- 36: Right cheek
- 37: Nose
- 38: Lips/around mouth
- 39: Chin
- 40: Left periorbital (around eye/eyebrow)
- 41: Right periorbital
- 42: Left jaw
- 43: Right jaw

##### Ears & Neck (4 zones): 44-47
- 44: Left ear
- 45: Right ear
- 46: Neck front
- 47: Neck side/back

##### Reserved (16 zones): 48-63
- Reserved for private body parts or future expansion
- Not used in public somidics

### Type Encoding (Bits 6-7)

#### 00: Natural mark
- Mole, birthmark, freckle, discoloration, skin patch, dimple, vitiligo
- Present at birth or developed naturally over time

#### 01: Scar
- Linear, patch, or irregular scar
- From accident, surgery, or illness
- Permanent scarring only (not recent wounds)

#### 10: Missing/Anomalous
- Missing: Absent tissue (missing fingertip, digit, ear portion)
- Anomalous: Extra tissue, fused parts, deformed features
- See Texture encoding for subtype distinction

#### 11: Artificial/Intentional
- Tattoo, piercing, implant, scarification, branding
- Deliberately created body modification

### Size Encoding (Bits 8-9)

Size measured by longest dimension:

#### 00: Grain-sized
- Comparable to rice grain or wheat grain
- Approximately 3-7mm
- Small enough to require close inspection

#### 01: Fingernail-sized
- Comparable to pinky fingernail
- Approximately 8-15mm
- Clearly visible when looking at area

#### 10: Coin-sized
- Comparable to common coin diameter
- Approximately 15-25mm
- Noticeable from normal social distance

#### 11: Larger than coin
- Greater than 25mm in longest dimension
- Obvious, prominent mark

### Texture Encoding (Bits 10-11)

Context-dependent meaning based on Type:

#### For Type=00 (Natural) and Type=01 (Scar):

00: Flush/flat
- No relief, same level as surrounding skin

01: Raised
- Elevated above skin
- Examples: bump, elevated mole, keloid, wart

10: Depressed
- Below skin level
- Examples: dimple, pit, indented scar

11: INVALID
- This combination is not permitted

#### For Type=10 (Missing/Anomalous):

00: Missing (absent tissue)
01: Extra (additional tissue)
10: Fused (joined parts)
11: Deformed (misshapen)

#### For Type=11 (Artificial/Intentional):

00: Tattooed/inked
- Flush permanent ink/dye (tattoos, brands)

01: Implanted
- Raised subdermal implant (chip, bead)

10: Pierced
- Hole through skin (piercing, stud)

11: INVALID
- This combination is not permitted

#### Semantic alignment
- 00: Flush across all types
- 01: Raised/extra across all types
- 10: Depressed/fused/pierced across all types
- 11: Only valid for Type=10 (deformed)

### Multiplicity/Special Bit (Bit 12)

Context-dependent meaning based on Type and Texture

### Special Bit (Bit 12) - Complete Normative Mapping

The meaning of bit 12 depends on Type and Texture context:

| Type | Texture | Bit 12=0 | Bit 12=1 | Meaning Context |
|------|---------|----------|----------|-----------------|
| 00 (Natural) | 00 (Flush) | Single mark | Cluster of marks | Multiplicity |
| 00 (Natural) | 01 (Raised) | Single mark | Cluster of marks | Multiplicity |
| 00 (Natural) | 10 (Depressed) | Single mark | Cluster of marks | Multiplicity |
| 01 (Scar) | 00 (Flush) | Single scar | Multiple scars | Multiplicity |
| 01 (Scar) | 01 (Raised) | Single scar | Multiple scars | Multiplicity |
| 01 (Scar) | 10 (Depressed) | Single scar | Multiple scars | Multiplicity |
| 10 (Missing/Anom) | 00 (Missing) | Always 0 | N/A | Not used |
| 10 (Missing/Anom) | 01 (Extra) | Mild | Severe | Intensity |
| 10 (Missing/Anom) | 10 (Fused) | Mild | Severe | Intensity |
| 10 (Missing/Anom) | 11 (Deformed) | Mild | Severe | Intensity |
| 11 (Artificial) | 00 (Tattooed) | No writing/text | Has writing/text | Writing indicator |
| 11 (Artificial) | 01 (Implanted) | Single implant | Multiple implants | Multiplicity |
| 11 (Artificial) | 10 (Pierced) | Single piercing | Multiple piercings | Multiplicity |

Decoding algorithm:
1. First decode Type (bits 6-7) and Texture (bits 10-11)
2. Use this table to determine bit 12 meaning
3. For Missing (Type=10, Texture=00), bit 12 MUST be 0
4. For all other valid combinations, bit 12 provides additional discrimination

Examples:
- Type=00, Texture=01, Bit12=0 → "Single raised natural mark"
- Type=00, Texture=01, Bit12=1 → "Cluster of raised natural marks"
- Type=11, Texture=00, Bit12=1 → "Tattoo with writing"
- Type=10, Texture=10, Bit12=1 → "Severe fused tissue"

Implementation: Decoders MUST use this table to correctly interpret bit 12.
 - see v0.3 document for full details:
- For natural marks/scars: Multiplicity (single vs. cluster)
- For tattoos with ink: Writing indicator (contains text)
- For piercings: Multiplicity (single vs. multiple)
- For anomalous features: Intensity (mild vs. severe)
- For missing parts: Always 0 (no special meaning)

## Contraquants Encoding

### Contraquant Structure (8 bits = 2 hex digits)

An **contraquant** encodes a negative assertion: "No marks of type(s) X in zone(s) Y"

Bits 0-3: Zone flags (4 bits)
Bits 4-7: Type flags (4 bits)

Both zones and types use FLAG encoding (not enumeration), allowing combinations.

### contra-zone Flags (Bits 0-3)

Each bit represents a broad zone category:

Bit 0: Hands + Fingers + Wrists
- Covers quant zones: 0-19 (fingers), 20-27 (hands), 32-33 (wrists)
- Total: 30 quant zones collapsed into 1 contra-zone
- Key feature: Can verify even with long sleeves

Bit 1: Face
- Covers quant zones: 34-43 (all facial zones)
- Total: 10 quant zones

Bit 2: Ears
- Covers quant zones: 44-45 (both ears)
- Total: 2 quant zones

Bit 3: Neck
- Covers quant zones: 46-47 (neck front and side/back)
- Total: 2 quant zones

Note: Arms (quant zones 28-31) are intentionally NOT covered by any contra-zone for modesty reasons.

### Contraquant-Type Flags (Bits 4-7)

Each bit represents a mark type category:

Bit 4: Natural marks
- Moles, birthmarks, freckles, discolorations

Bit 5: Scars
- All types of scars

Bit 6: Tattoos
- All tattooed/inked marks

Bit 7: Piercings/Implants
- All piercings and implants

### Flag Encoding Examples

0x41 (binary: 01000001)
```
Zones: 0001 = Hands+fingers+wrists only
Types: 0100 = Tattoos only
Meaning: "No tattoos on hands+fingers+wrists"
```

0x43 (binary: 01000011)
```
Zones: 0011 = Hands+wrists + Face
Types: 0100 = Tattoos only
Meaning: "No tattoos on hands+wrists or face"
```

0xC1 (binary: 11000001)
```
Zones: 0001 = Hands+wrists only
Types: 1100 = Tattoos + Piercings
Meaning: "No tattoos or piercings on hands+wrists"
```

0xFF (binary: 11111111)
```
Zones: 1111 = All zones
Types: 1111 = All types
Meaning: "No marks of any kind anywhere in public zones"
```

### Common Contraquants Reference

```
11 = No natural marks on hands+fingers+wrists
12 = No natural marks on face
1f = No natural marks anywhere

21 = No scars on hands+fingers+wrists
2f = No scars anywhere

41 = No tattoos on hands+fingers+wrists
42 = No tattoos on face
43 = No tattoos on hands+wrists or face
4f = No tattoos anywhere

81 = No piercings on hands+fingers+wrists
82 = No piercings on face
8f = No piercings anywhere

c1 = No tattoos or piercings on hands+wrists
c2 = No tattoos or piercings on face
cf = No tattoos or piercings anywhere

ff = No marks of any kind anywhere
```

### Maximal Compaction Principle

Contraquants are always maximally compacted to minimize count.

Since both zones AND types are flags (bitmasks), you combine:
- Zone bits for marks with same type constraints
- Type bits for marks with same zone constraints

Example:
```
Input:  -41-42-81-82
  41 = No tattoos on hands+wrists
  42 = No tattoos on face
  81 = No piercings on hands+wrists
  82 = No piercings on face

Compaction:
  Step 1: Combine 0x41+0x42 → 0x43 (same type, OR zones)
  Step 2: Combine 0x81+0x82 → 0x83 (same type, OR zones)
  Step 3: Combine 0x43+0x83 → 0xC3 (same zones, OR types)

Output: -c3 (No tattoos or piercings on hands+wrists or face)
```

Maximum contraquants in canonical form: 4

This only occurs with 4 different zone patterns for the 4 type dimensions - very rare in practice.

Typical usage:
- Most somidics: 0-1 contraquants
- Common: 1 contraquant (e.g., `4f` or `cf`)
- Rare: 2-3 contraquants
- Very rare: 4 contraquants

## Checksum Specification (Quants Only)

### Algorithm

A 5-bit checksum is computed for each 13-bit somid using polynomial division.

Parameters:
- Polynomial: 0x05 (x^5 + x^2 + 1)
- Width: 5 bits
- Initial value: 0x1F (all 1s)
- Bit order: MSB-first (bit 12 â†’ bit 0)

Error detection capability:
- Detects all single-bit errors
- Detects all 2-bit errors
- Rejects ~97% of random errors (31/32 invalid patterns)

### Reference Implementation (Normative)

This Python implementation is normative. All conformant implementations MUST produce identical results.

```python
def compute_checksum(somid_13bit):
    """
    Compute 5-bit checksum for a 13-bit somid.
    
    Args:
        somid_13bit: Integer value 0-8191 (13-bit somid)
        
    Returns:
        Integer value 0-31 (5-bit checksum)
    """
    poly = 0x05  # x^5 + x^2 + 1
    crc = 0x1F   # Initialize to all 1s
    
    # Process each bit from MSB (bit 12) to LSB (bit 0)
    for i in range(13):
        bit = (somid_13bit >> (12 - i)) & 1
        msb = (crc >> 4) & 1
        crc = ((crc << 1) | bit) & 0x1F
        if msb:
            crc ^= poly
    
    return crc
```

### Canonical Test Vectors (Normative)

Implementations MUST produce these exact results:

| Somid (decimal) | Somid (binary)    | Checksum | Combined 18-bit        | Quant (decimal) |
|-----------------|-------------------|----------|------------------------|-----------------|
| 0               | 0b0000000000000   | 22       | 0b000000000000010110   | 000022          |
| 1               | 0b0000000000001   | 23       | 0b000000000000110111   | 000055          |
| 32              | 0b0000000100000   | 19       | 0b000000010000010011   | 001043          |
| 100             | 0b0000001100100   | 29       | 0b000000110010011101   | 003229          |
| 1000            | 0b0001111101000   | 18       | 0b000111110100010010   | 032018          |
| 8191            | 0b1111111111111   | 29       | 0b111111111111111101   | 262141          |

Construction: Combined 18-bit = (somid << 5) | checksum. The Quant is the decimal representation of this 18-bit value (Plane 0 offset = 0).

Note: Contraquants do NOT use checksum validation (only 256 possible values, both nibbles must be non-zero).

## Plane Architecture

The 6-digit decimal space (000000-999999) is divided into planes:

### Plane Structure
**Plane** size: 262,144 values (2^18)
- Each plane contains 8,192 unique somids (2^13)
- Each somid has 32 possible CRC values (2^5)
- Only 1 CRC value per somid is valid

### Plane Allocation

Plane 0: Human Somidics (000000-262143)
- All human somidions (public zones only in this version)
- Public zones: bits 0-5 = 0-47
- Reserved zones: bits 0-5 = 48-63 (reserved for future versions; MUST NOT appear in a well-formed somidic in v0.6)

Plane 1: Animal Somidics (262144-524287)
- Reserved for future animal specification
- Different attribute encoding appropriate for animals
- May include species, gender, breed as attributes

Plane 2: Reserved (524288-786431)
- Future expansion

Plane 3: Special Values (786432-999999)
- Partial plane (213,568 values)
- Test and special-purpose somidics

### Plane 3: Special Values (786432-999999)

Test/magic values with special semantics:

#### 999999: "Always Match" Test Value

```
@+999999
```

Validation: CRC check SKIPPED; MUST be rejected by default (see Validation Rules), but MAY be accepted in an explicit testing mode
Semantics: Matches any verification attempt  
Use: System testing, capability verification
Decoding: Returns special sentinel structure (all fields null/test values)
Production: MUST NOT be used in real somidics

#### 999998: "Never Match" Test Value

```
@+999998
```

Validation: CRC check SKIPPED; MUST be rejected by default (see Validation Rules), but MAY be accepted in an explicit testing mode
Semantics: Fails all verification attempts
Use: Negative testing, fraud detection testing
Decoding: Returns special sentinel structure (all fields null/test values)
Production: MUST NOT be used in real somidics

#### Other Plane 3 Values (786432-999997)

Status: Reserved for future use
Validation: Currently treated as invalid

Implementation requirements:
- Parsers MUST recognize 999999 and 999998 as special values
- Validators MUST skip CRC checking for these values
- Validators MUST still enforce well-formedness (syntax)
- Verifiers MUST implement the always-match/never-match semantics
- Production systems SHOULD reject these values in user somidics
- Test systems MAY use these values for verification testing


Note: Contraquants use a separate encoding space (hex notation), not decimal plane space.

## Complete Notation Specification (v0.6 UPDATE)

### Notation Grammar

```
somidic := '@' component+

component := '+' quant
           | '-' contra

quant := \d{6}      # Always 6 digits, leading zeros required
contra := [0-9a-fA-F]{2}    # Always 2 hex digits, both nibbles non-zero
```

Key principles:
- All somidics start with `@`
- Each quant preceded by `+`
- Each contra preceded by `-`
- At least one component (quant or contra) required

### Notation Examples

Single quant:
```
@+147293
```

Multiple quants:
```
@+147293+582047
@+147293+582047+923841
```

Single quant with contraquant:
```
@+147293-41
```

Multiple quants with one contraquant:
```
@+147293+582047-cf
```

Multiple quants with multiple contraquants:
```
@+147293+582047-41-cf
```

Pure contraquants (no quants):
```
@-cf
@-41-cf
```

### Semantic Interpretation

The notation represents set operations:

```
@+P₁+P₂+...+Pₙ-A₁-A₂-...-Aₘ

Matches body states in:
  (P₁ ∪ P₂ ∪ ... ∪ Pₙ) \ (A₁ ∪ A₂ ∪ ... ∪ Aₘ)

Where:
  Pᵢ = states matching quant i
  Aⱼ = states matching contraquant j
  + = union (∪)
  - = set difference (\)
```

Special case: Pure contraquants
```
@-A₁-A₂-...-Aₘ

Matches body states in:
  U \ (A₁ ∪ A₂ ∪ ... ∪ Aₘ)

Where U = universal set (all possible body states)
```

Examples:
```
@+147293        = Bodies matching somidic 147293
@+147293-41     = Bodies matching 147293 AND lacking hand tattoos
@-41            = All bodies lacking hand tattoos
```

### Canonical Ordering Rules

For quants:
1. Sort numerically ascending
2. Remove duplicates
3. Each preceded by `+`

For contraquants:
1. Maximally compact (combine zones and types)
2. Sort numerically ascending (as 8-bit values)
3. Remove duplicates (automatic after compaction)
4. Each preceded by `-`

Overall format:
- Quants first (if any), then contraquants (if any)
- All components preceded by their operator (`+` or `-`)

#
## Grammar Rules (Normative)

### Well-formed Somidic Syntax

A string is well-formed if and only if it matches ONE of these patterns:

Pattern 1: quants only
```regex
^@(\+\d{6})+$
```

Pattern 2: quants and contraquants
```regex
^@(\+\d{6})+(-[0-9a-f]{2})+$
```

Pattern 3: Contraquants only
```regex
^@(-[0-9a-f]{2})+$
```

### Syntax Rules

1. Prefix: MUST begin with `@`
2. Components: At least one component required
3. Quants: Format `+\d{6}` (plus sign, exactly 6 decimal digits, zero-padded)
4. Contraquants: Format `-[0-9a-fA-F]{2}` (minus sign, exactly 2 hex digits)
5. Ordering: All quants MUST precede all contraquants (no interleaving)
6. Whitespace: NO whitespace permitted anywhere
7. Trailing: NO trailing operators permitted

### Invalid Examples

```
147293          # Missing @ prefix
@147293         # Missing + operator
@-41+147293     # Wrong order (contraquants before quants)
@ +147293       # Whitespace not permitted
@+147293-       # Trailing operator
@+147293-4      # Contraquant must be 2 hex digits
@+12345         # Quant must be exactly 6 digits
```

## Validation Rules

### Conformance Requirements

A conformant implementation MUST:
1. Validate - Correctly accept/reject somidic strings per the grammar and validation rules
2. Decode - Parse valid somidics to the normative structured format (see Decoding Specification)
3. Canonicalize - Convert valid somidics to canonical form (sorted, compacted)

Implementations MAY additionally provide:
- Natural language rendering (see Rendering Guidance - non-normative)
- Enrollment wizards or user interfaces
- Additional validation beyond the minimum requirements

Structure validation:
- Must start with `@`
- Must have at least one component
- Each component must be valid (`+\d{6}` or `-[0-9a-f]{2}`)
- No whitespace within somidic
- No trailing operators

Quants:
- Must be 6-digit decimal with valid CRC-5
- Zone MUST be 0-47 (public zones)
- Zone values 48-63 are RESERVED and MUST be rejected as invalid in this version
- Leading zeros required (e.g., `000123` not `123`)
- Plane 3 special values (e.g., `@+999999`, `@+999998`) MUST be rejected by default; a validator MAY expose an explicit option (e.g., `allow_test_plane=True`) to accept them for testing

Contraquants:
- Must be pairs of hex digits (even character count after `-`)
- Each byte must have both nibbles non-zero
  - Invalid: 00, 01-0f, 10-f0 (meaningless assertions)
  - Valid: 11-ff (excluding x0 and 0x patterns)
- Must be in maximally compacted canonical form

**Canonical form:**
- Quants in ascending order
- Contraquants maximally compacted and in ascending order
- Quants before contraquants
- No duplicates
- Hex digits in lowercase

### Invalid Examples

```
@                       # No components
147293                  # Missing @ prefix
@+147293-               # Trailing operator
@+-                     # Operators without values
@+ 147293               # Whitespace
@+582047+147293         # Wrong order (not ascending)
@+147293-CF             # Uppercase hex (valid but not canonical - should be -cf)
@+147293-41-42          # Not maximally compacted (should be -43)
```

## Reading Aloud Protocol (v0.6 NEW SECTION)

When communicating somidic somidics verbally (e.g., over phone, during enrollment), use the following protocol:

### Basic Rules

Symbol pronunciation:
- `@` prefix: Silent (implied by context)
- `+` operator: Say "PLUS"
- `-` operator: Say "MINUS"

Digit pronunciation:
- Read in groups of three: "one four seven, two nine three"
- Always say "zero" (never "oh" or "o")
- Hex letters: Say letter name ("a", "b", "c", "d", "e", "f")

### Examples

Single quant:
```
@+147293
→ "PLUS one four seven, two nine three"
```

Multiple quants:
```
@+147293+582047
→ "PLUS one four seven, two nine three, PLUS five eight two, zero four seven"
```

quant with contraquant:
```
@+147293-41
→ "PLUS one four seven, two nine three, MINUS four one"
```

Multiple quants with contraquant:
```
@+147293+582047-cf
→ "PLUS one four seven, two nine three, PLUS five eight two, zero four seven, MINUS c f"
```

Pure contraquant:
```
@-cf
→ "MINUS c f"
```

Multiple contraquant:
```
@+147293-41-cf
→ "PLUS one four seven, two nine three, MINUS four one, MINUS c f"
```

### Verification Protocol

When receiving verbally:
1. Enter digits/letters as heard
2. System validates:
   - CRC-5 for quants
   - Format validation for contraquants
3. If invalid after 2-3 attempts, request repeat
4. If valid, accept and proceed

Best practices:
- Speak clearly and at moderate pace
- Pause between digit groups and operators
- Repeat if any confusion
- Confirm received value by reading back

## Decoding Somidics (v0.6 NEW SECTION - NORMATIVE)

Decoding converts a somidic somidic string into structured attribute data. This section defines the normative output format that all implementations MUST produce.

### Parsing Somidics

Step 1: Extract components
```python
def parse_somidic(somidic: str) -> tuple:
    """
    Parse somidic into quant and contraquant components.
    Returns: (list of quants, list of contraquants)
    """
    if not somidic.startswith('@'):
        raise ValueError("Somidic must start with '@'")
    
    # Remove @ prefix
    s = somidic[1:]
    
    # Split on + and -, keeping delimiters
    import re
    parts = re.split(r'(\+|-)', s)
    
    quants = []
    contras = []
    
    for i in range(0, len(parts), 2):
        operator = parts[i]
        value = parts[i+1] if i+1 < len(parts) else None
        
        if value is None:
            raise ValueError(f"Operator '{operator}' without value")
        
        if operator == '+':
            quants.append(value)
        elif operator == '-':
            contras.append(value)
    
    return quants, contras
```

### Decoding Quants (Normative)

Step 2: Decode each quant to structured attributes

All implementations MUST produce this structure:

```json
{
  "zone": <integer 0-63>,
  "zone_category": "finger" | "hand" | "arm" | "face" | "ear_neck" | "reserved",
  "zone_name": <string>,
  "type": <integer 0-3>,
  "type_name": "natural" | "scar" | "missing_anomalous" | "artificial",
  "size": <integer 0-3>,
  "size_name": "grain" | "fingernail" | "coin" | "larger",
  "texture": <integer 0-3>,
  "texture_name": <string>,
  "special": <integer 0-1 or null>,
  "special_meaning": <string or null>
}
```

Zone name mapping (normative):
```
0: "left_thumb_ring"
1: "left_thumb_upper"
2: "left_index_ring"
... (full mapping)
32: "left_wrist"
... etc
```

Type name mapping (normative):
```
0: "natural"
1: "scar"
2: "missing_anomalous"
3: "artificial"
```

Size name mapping (normative):
```
0: "grain"
1: "fingernail"
2: "coin"
3: "larger"
```

Texture name mapping (context-dependent, normative):

Texture values MUST be interpreted using a type-dependent mapping consistent with the Type/Texture encoding tables above.

#### For Type=00 (Natural) and Type=01 (Scar)
```
0b00: "flush"
0b01: "raised"
0b10: "depressed"
0b11: INVALID (MUST NOT appear in a well-formed quant)
```

#### For Type=10 (Missing/Anomalous)
```
0b00: "missing"
0b01: "extra"
0b10: "fused"
0b11: "deformed"
```

#### For Type=11 (Artificial/Intentional)
```
0b00: "tattooed"
0b01: "implanted"
0b10: "pierced"
0b11: INVALID (MUST NOT appear in a well-formed quant)
```

Special bit interpretation (context-dependent, normative):
- Natural marks/scars: "single" or "multiple"
- Tattoos (Type=3, Texture=2): "no_writing" or "with_writing"
- Piercings (Type=3, Texture=3): "single" or "multiple"
- Anomalous (Type=2, Texture≠0): "mild" or "severe"
- Missing (Type=2, Texture=0): null

### Decoding Contraquants (Normative)

Step 3: Decode each contraquant to structured attributes

All implementations MUST produce this structure:

```json
{
  "zone_flags": <integer 0-15>,
  "type_flags": <integer 0-15>,
  "zones": [<array of zone category strings>],
  "types": [<array of type category strings>]
}
```

Zone category strings (normative):
```
Bit 0 set: "hands_fingers_wrists"
Bit 1 set: "face"
Bit 2 set: "ears"
Bit 3 set: "neck"
```

Type category strings (normative):
```
Bit 4 set: "natural"
Bit 5 set: "scars"
Bit 6 set: "tattoos"
Bit 7 set: "piercings"
```

Example:
```python
def decode_contraquant(contra_hex: str) -> dict:
    """
    Decode 2-hex-digit contraquant to structured attributes.
    This function's output format is normative.
    """
    contra_value = int(contra_hex, 16)
    
    zone_flags = contra_value & 0x0F
    type_flags = (contra_value >> 4) & 0x0F
    
    zones = []
    if zone_flags & 0x01: zones.append('hands_fingers_wrists')
    if zone_flags & 0x02: zones.append('face')
    if zone_flags & 0x04: zones.append('ears')
    if zone_flags & 0x08: zones.append('neck')
    
    types = []
    if type_flags & 0x01: types.append('natural')
    if type_flags & 0x02: types.append('scars')
    if type_flags & 0x04: types.append('tattoos')
    if type_flags & 0x08: types.append('piercings')
    
    return {
        'zone_flags': zone_flags,
        'type_flags': type_flags,
        'zones': zones,
        'types': types
    }
```

### Complete Decoding Example

Input: `@+147293-41`

Output:
```json
{
  "quants": [
    {
      "zone": 32,
      "zone_category": "arm",
      "zone_name": "left_wrist",
      "type": 0,
      "type_name": "natural",
      "size": 2,
      "size_name": "coin",
      "texture": 1,
      "texture_name": "raised",
      "special": 0,
      "special_meaning": "single"
    }
  ],
  "contraquants": [
    {
      "zone_flags": 1,
      "type_flags": 4,
      "zones": ["hands_fingers_wrists"],
      "types": ["tattoos"]
    }
  ]
}
```

## Describing/Rendering Somidics (v0.6 NEW SECTION - NON-NORMATIVE)

This section provides guidance for converting structured attributes into human-readable natural language descriptions. The specific templates and phrasing are not normative; implementers may adapt for their target language and context.

### English Rendering Templates (Informative)

#### quant Somidics

Standard verbosity template:
```
"{special_meaning} {texture_name} {size_name} {type_name} on {zone_name}"
```

Examples:
```
"Single raised coin-sized natural mark on left wrist"
"Multiple flush fingernail-sized scars on right cheek"
"Single tattooed coin-sized mark with writing on left forearm"
```

Compact verbosity template:
```
"{type_name} on {zone_name}"
```

Examples:
```
"Natural mark on left wrist"
"Scar on right cheek"
"Tattoo on left forearm"
```

Full verbosity template:
```
Include all attributes explicitly with maximum detail.
```

#### Contraquants

Standard template:
```
"No {types} on {zones}"
```

Examples:
```
"No tattoos on hands, fingers, or wrists"
"No tattoos or piercings on face"
"No marks of any kind anywhere in public zones"
```

Compact template:
```
"No {type} ({zone})"
```

Examples:
```
"No hand tattoos"
"No facial piercings"
```

### Multi-Language Considerations (Informative)

Different languages have different syntax patterns. Implementers should adapt templates accordingly:

Spanish example:
- English: "Single raised coin-sized natural mark on left wrist"
- Spanish: "Marca natural única elevada del tamaño de una moneda en la muñeca izquierda"
- Note: Adjectives typically follow nouns in Spanish

Japanese example:
- English: "Natural mark on left wrist"
- Japanese: "左手首に自然な印"
- Note: Location typically comes before the mark description

Arabic example:
- Note: Right-to-left text, different adjective placement

### Context-Specific Rendering (Informative)

Enrollment context:
- Use full or standard verbosity
- Help user confirm correct mark chosen
- Example: "Single raised coin-sized natural mark on left wrist"

Verification display:
- Use standard verbosity for verifier
- Include both encoded form and description
- Example: 
  ```
  @+147293-41
  "Single raised natural mark on left wrist"
  "No tattoos on hands, fingers, or wrists"
  ```

Compact display (e.g., card):
- Use compact verbosity to save space
- Example: "Wrist mole / No hand tattoos"

Screen reader / accessibility:
- Use full verbosity for clarity
- Spell out abbreviations
- Example: "Single raised coin-sized natural mark on left wrist. No tattoos on hands, fingers, or wrists."

### Implementation Guidance

Implementations SHOULD:
1. Decode somidics to normative structured format (required)
2. Provide rendering functions for target language (recommended)
3. Support multiple verbosity levels (recommended)
4. Allow context-specific customization (optional)

Implementations MAY:
- Add additional rendering formats
- Support custom templates
- Provide translation capabilities
- Include cultural adaptations

## Override Principle

### Quants Specify Exceptions to Contraquant Exclusions

Key rule: Quants and contraquants work together. Quants identify specific marks (inclusions), contraquants identify absences (exclusions), and quants create exceptions to contraquant exclusions.

A somidic like `@+147293-41` means:
1. "I HAVE this specific mark" (quant 147293)
2. "I DO NOT have other marks of this type in this zone" (contraquant 41)

Example:
```
@+147293-41
  Quant (147293): "Tattoo with writing on left wrist" (zone 32)
  Contraquant (41): "No tattoos on hands+fingers+wrists"
  
Zone 32 (left wrist) is IN contra-zone 0 (hands+fingers+wrists).

Interpretation: "I have THIS specific tattoo on my left wrist,
                 but no OTHER tattoos on hands, fingers, or wrists"
```

Verification process:
1. Verify the specific quant (tattoo on left wrist) ✓
2. Scan the rest of hands+fingers+wrists for OTHER tattoos ✓
3. Accept if both conditions met

Implementation:
```python
def verify_contraquant(person, contra_value, quant_zones):
    """
    contra_value: 8-bit contraquant
    quant_zones: list of zones claimed in quants
    """
    contra_zones = expand_contra_zone_flags_to_quant_zones(contra_value & 0x0F)
    contra_types = extract_type_flags(contra_value >> 4)
    
    # Remove zones already claimed in quants (exceptions)
    zones_to_check = [z for z in contra_zones if z not in quant_zones]
    
    # Verify no marks of contra_types exist in zones_to_check
    return not has_marks(person, zones_to_check, contra_types)
```

## Verifier Discretion Principle

### Critical Design Principle

Contraquants are ALWAYS optional from the verifier's perspective.

The verifier decides whether checking contraquants is worth the extra time based on:
- Transaction value
- Fraud risk
- Time constraints
- Context

Low-security context:
```
Somidic: @+147293-cf
Verifier checks: Just quant (30 seconds)
Ignores: Contraquant
```

High-security context:
```
Somidic: @+147293-cf
Verifier checks: quant AND contraquant (120 seconds)
```

This means:
- Somidic holder encodes once
- Verifier pays cost only when justified
- System degrades gracefully
- Context-appropriate security

## Security Properties

### Entropy Analysis (v0.6)

Single quant:
- Effective entropy: ~11.7 bits
- Discrimination: ~1-in-3,300

With one contraquant:
- Contra entropy: ~2-3 bits (depends on population)
- Combined: ~14-15 bits
- Discrimination: ~1-in-15,000 to 1-in-30,000

Multiple somidics:
- Two quants: ~23 bits = 1-in-8-million
- Two quants + contra: ~26 bits = 1-in-60-million
- Three quants + contra: ~38 bits = 1-in-250-billion

### Attack Resistance

Credit card theft with contraquant:
- Thief needs mark in correct zone (1/3,300)
- AND needs absence of marks in contra-zone (1/5 - 1/3 depending on type)
- Combined: 1/15,000 to 1/100,000

Much stronger than PIN (1/10,000) which can be observed.

### Limitations
Not suitable for:
- Legal proof of identity (too fuzzy)
- High-security authentication alone
- High-throughput scenarios

Best for:
- Casual to medium fraud prevention
- Equipment-free contexts
- Privacy-preserving identification
- Backup/fallback authentication

## Implementation Notes

### Regex Patterns

```regex
# Complete somidic pattern
^@(\+\d{6}|-[0-9a-f]{2})+$

# Extract all somidics from text
@(\+\d{6}|-[0-9a-f]{2})+

# Extract quants
(?<=\+)\d{6}

# Extract contraquants
(?<=-)[0-9a-f]{2}
```

### Contraquant Validation

```python
def validate_contraquant_byte(value):
    """
    Validate a single 8-bit contraquant
    """
    zone_nibble = value & 0x0F
    type_nibble = value & 0xF0
    
    if zone_nibble == 0:
        raise ValueError("Zone nibble must be non-zero")
    if type_nibble == 0:
        raise ValueError("Type nibble must be non-zero")
    
    return True
```

### Maximal Compaction Algorithm (Normative)

A contraquant byte encodes a set of *absence claims* of the form:
> ─Å“No marks of type(s) T in zone(s) Z.─Â

Where:
- `zone_flags` Z is the low nibble (bits 0-3), interpreted as a set of up to 4 zone-groups.
- `type_flags` T is the high nibble (bits 4-7), interpreted as a set of up to 4 type categories.

A contraquant with `(Z, T)` semantically expands to the Cartesian product `Z &times; T` (up to 16 atomic claims).

Canonical form requirement: Given any multiset of contraquants, implementations MUST reduce them to an equivalent set with the minimum number of contraquants, and MUST select a unique result.

#### Step 1: Normalize and expand
1. Parse each contraquant byte into `(Z, T)`.
2. Reject any byte with `Z=0` or `T=0`.
3. Expand each `(Z, T)` into the set of atomic claims  
   `S ⊆ {1,2,4,8} &times; {0x10,0x20,0x40,0x80}` by enumerating each set bit in `Z` and each set bit in `T`.
4. Let `S` be the union of atomic claims from all contraquants.

#### Step 2: Minimum rectangle cover
A single contraquant corresponds to a rectangle `Z&times;T` in the 4×4 atomic-claim grid.

Canonical compaction is defined as a minimum-cardinality rectangle cover of `S`.

Implementations MUST find a set of rectangles `R = {(Z_i, T_i)}` such that:
- `⋃ (Z_i&times;T_i) = S`, and
- `|R|` is minimal.

Upper bound: `|R| &le; 4`.

#### Step 3: Deterministic tie-breaking
If multiple minimum covers exist, implementations MUST choose the one whose sorted byte list  
`B = sorted([Z_i | T_i])` is lexicographically smallest.

#### Reference search procedure (informative but deterministic)
Given the small 4×4 domain, a straightforward search is practical and yields deterministic results:
1. Enumerate all candidate rectangles `(Z,T)` where `Z∈{1..15}` and `T∈{0x10..0xF0 step 0x10}`.
2. For `k` in `1..4`, enumerate combinations of `k` candidates in increasing byte order, and select the first combination whose union equals `S`.
3. Emit those `k` bytes, sorted ascending.

#### Non-minimal compaction pitfall (informative)

Naive strategies that merge type categories only when their zone masks are identical are NOT equivalent
to the minimum rectangle cover requirement.

Counterexample:

Atomic claims:
- A absent in zones {1,2}
- B absent in zones {1,2}
- A absent in zones {3,4}
- C absent in zones {3,4}

Per-type zone masks:
- A: {1,2,3,4}
- B: {1,2}
- C: {3,4}

Row-merging yields 3 contraquants:
- A×{1,2,3,4}, B×{1,2}, C×{3,4}

Minimum rectangle cover (cardinality 2):
- (A|B)×{1,2}
- (A|C)×{3,4}

Canonical compaction MUST allow a type category to appear in multiple contraquants when doing so
reduces total count.

## Decoding Specification (Normative)

### Quant Decoding Structure

Given a valid quant (6-digit decimal number), decode to this structured format:

```json
{
  "zone": <int 0-47>,
  "zone_name": <string>,
  "type": <int 0-3>,
  "type_name": <string>,
  "size": <int 0-3>,
  "size_name": <string>,
  "texture": <int 0-3>,
  "texture_name": <string>,
  "special": <int 0-1 or null>,
  "special_meaning": <string or null>
}
```

Field specifications:

type_name: MUST be one of:
- `"natural"` (type=00)
- `"scar"` (type=01)
- `"missing_anomalous"` (type=10)
- `"artificial"` (type=11)

size_name: MUST be one of:
- `"grain"` (size=00)
- `"fingernail"` (size=01)
- `"coin"` (size=02)
- `"larger"` (size=03)

texture_name: Context-dependent on type (see Texture Encoding section)

special: Context-dependent on type and texture (see Multiplicity/Special section)

### Contraquant Decoding Structure

Given a valid contraquant (2 hex digits), decode to this structured format:

```json
{
  "zone_flags": <int 0-15>,
  "type_flags": <int 0-15>,
  "zones": <array of strings>,
  "types": <array of strings>
}
```

zones array: MUST contain zero or more of:
- `"hands_fingers_wrists"` (bit 0 set)
- `"face"` (bit 1 set)
- `"ears"` (bit 2 set)
- `"neck"` (bit 3 set)

types array: MUST contain zero or more of:
- `"natural"` (bit 4 set)
- `"scars"` (bit 5 set)
- `"tattoos"` (bit 6 set)
- `"piercings"` (bit 7 set)

### Example Decoding

Input quant: `000022` (somid=0, checksum=22)
```json
{
  "zone": 0,
  "zone_name": "left_thumb_ring",
  "type": 0,
  "type_name": "natural",
  "size": 0,
  "size_name": "grain",
  "texture": 0,
  "texture_name": "flush",
  "special": 0,
  "special_meaning": "single"
}
```

Input contraquant: `41`
```json
{
  "zone_flags": 1,
  "type_flags": 4,
  "zones": ["hands_fingers_wrists"],
  "types": ["tattoos"]
}
```

### Zone Names (Normative)

All implementations MUST use these exact zone names:

| Zone | Name | Description |
|------|------|-------------|
| 0 | left_thumb_ring | Left thumb - ring portion |
| 1 | left_thumb_upper | Left thumb - upper portion |
| 2 | left_index_ring | Left index - ring portion |
| 3 | left_index_upper | Left index - upper portion |
| 4 | left_middle_ring | Left middle - ring portion |
| 5 | left_middle_upper | Left middle - upper portion |
| 6 | left_ring_ring | Left ring finger - ring portion |
| 7 | left_ring_upper | Left ring finger - upper portion |
| 8 | left_pinky_ring | Left pinky - ring portion |
| 9 | left_pinky_upper | Left pinky - upper portion |
| 10 | right_thumb_ring | Right thumb - ring portion |
| 11 | right_thumb_upper | Right thumb - upper portion |
| 12 | right_index_ring | Right index - ring portion |
| 13 | right_index_upper | Right index - upper portion |
| 14 | right_middle_ring | Right middle - ring portion |
| 15 | right_middle_upper | Right middle - upper portion |
| 16 | right_ring_ring | Right ring finger - ring portion |
| 17 | right_ring_upper | Right ring finger - upper portion |
| 18 | right_pinky_ring | Right pinky - ring portion |
| 19 | right_pinky_upper | Right pinky - upper portion |
| 20 | left_palm_thumb | Left palm - thumb side |
| 21 | left_palm_pinky | Left palm - pinky side |
| 22 | left_back_thumb | Left hand back - thumb side |
| 23 | left_back_pinky | Left hand back - pinky side |
| 24 | right_palm_thumb | Right palm - thumb side |
| 25 | right_palm_pinky | Right palm - pinky side |
| 26 | right_back_thumb | Right hand back - thumb side |
| 27 | right_back_pinky | Right hand back - pinky side |
| 28 | left_upper_arm | Left upper arm |
| 29 | right_upper_arm | Right upper arm |
| 30 | left_forearm | Left forearm |
| 31 | right_forearm | Right forearm |
| 32 | left_wrist | Left wrist |
| 33 | right_wrist | Right wrist |
| 34 | forehead | Forehead |
| 35 | left_cheek | Left cheek |
| 36 | right_cheek | Right cheek |
| 37 | nose | Nose |
| 38 | mouth | Lips/around mouth |
| 39 | chin | Chin |
| 40 | left_periorbital | Left eye/eyebrow area |
| 41 | right_periorbital | Right eye/eyebrow area |
| 42 | left_jaw | Left jaw |
| 43 | right_jaw | Right jaw |
| 44 | left_ear | Left ear |
| 45 | right_ear | Right ear |
| 46 | neck_front | Neck front |
| 47 | neck_side_back | Neck side/back |



# Validation and Conformance (Normative)

This section defines how implementations must validate somidic strings, detect errors, and determine canonical form compliance.

## Conformance Levels

A conformant implementation MUST implement all three validation phases. Implementations MAY choose different strictness behaviors:

### Minimal Conformance
- Accepts any syntactically valid somidic
- Performs syntax and semantic validation (Phases 1-2)
- MAY accept non-canonical forms
- Use case: Lenient parsers, migration tools

### Standard Conformance (RECOMMENDED)
- Accepts only canonical somidics
- Performs all three validation phases
- Rejects non-canonical forms with specific error
- Use case: Production systems, verification tools

### Strict Conformance
- Performs all validation
- Rejects test plane values (999999, 999998) by default
- Enforces case sensitivity (lowercase hex only)
- Use case: High-security systems

## Validation Phases

Validation proceeds in three distinct phases. Each phase assumes the previous phase passed.

Phase 1: Syntax Validation
- Checks grammar rules only
- Uses regex matching
- No semantic knowledge required
- Fast rejection of malformed input

Phase 2: Semantic Validation
- Checks CRC-5 for quants
- Validates zone ranges
- Validates type-texture combinations
- Validates contraquant nibbles
- Requires decoding bits

Phase 3: Canonicalization
- Checks sorting
- Checks compaction (contraquants)
- Checks deduplication
- Requires comparing against canonical form

## Phase 1: Syntax Validation

Purpose: Determine if input matches somidic grammar

Validation steps:

### 1.1 Prefix Check
```
RULE: Somidic MUST start with '@'
ERROR: SomidicSyntaxError("Somidic must start with '@'")
```

### 1.2 Component Structure
```
RULE: Somidic MUST match ONE of these patterns:
  Pattern A: ^@(\+\d{6})+$                    (quants only)
  Pattern B: ^@(\+\d{6})+(-[0-9a-fA-F]{2})+$ (quants + contraquants)
  Pattern C: ^@(-[0-9a-fA-F]{2})+$            (contraquants only)

ERROR: SomidicSyntaxError("Invalid somidic format")
```

### 1.3 Component Format
For each component:

Quant format:
```
RULE: Exactly 6 decimal digits after '+'
VALID:   @+147293
INVALID: @+12345  (too few digits)
INVALID: @+1472930 (too many digits)
ERROR: SomidicSyntaxError("Quant must be exactly 6 digits")
```

Contraquant format:
```
RULE: Exactly 2 hex digits after '-'
RULE: Hex digits must be [0-9a-fA-F] (case-insensitive at this phase)
VALID:   @-41
VALID:   @-CF (accepted here, may fail canonicalization)
INVALID: @-4 (too few digits)
INVALID: @-GH (invalid hex)
ERROR: SomidicSyntaxError("Contraquant must be exactly 2 hex digits")
```

### 1.4 Whitespace Check
```
RULE: NO whitespace anywhere in somidic
INVALID: @+ 147293
INVALID: @ +147293
INVALID: @+147293 -41
ERROR: SomidicSyntaxError("Whitespace not permitted")
```

### 1.5 Trailing Operator Check
```
RULE: NO trailing operators
INVALID: @+147293-
INVALID: @+147293+
ERROR: SomidicSyntaxError("Trailing operator not permitted")
```

### 1.6 Component Ordering
```
RULE: All quants MUST precede all contraquants
VALID:   @+147293-41
INVALID: @-41+147293
ERROR: SomidicSyntaxError("Quants must precede contraquants")
```

Phase 1 Output: List of quant strings, list of contraquant strings

## Phase 2: Semantic Validation

Purpose: Verify that components encode valid somidion/contrasomidion data

For each quant:

### 2.1 Quant CRC-5 Validation
```
1. Extract quant value Q (6-digit decimal → 18-bit integer)
2. Compute: somid_13 = Q >> 5
3. Compute: checksum = Q & 0x1F
4. Compute: expected_checksum = CRC5(somid_13)
5. IF checksum ≠ expected_checksum:
     ERROR: SomidicChecksumError("CRC-5 validation failed")
```

Special handling for Plane 3 test values:
```
IF Q = 999999 OR Q = 999998:
  IF strict_mode OR NOT test_mode:
    ERROR: SomidicPlaneError("Test plane values not permitted")
  ELSE:
    SKIP CRC validation (test values have no valid CRC)
    CONTINUE to Phase 3
```

### 2.2 Zone Range Validation
```
1. Extract zone = somid_13 & 0x3F (bits 0-5)
2. IF zone > 47:
     ERROR: SomidicZoneError("Zone 48-63 are reserved")
```

### 2.3 Type-Texture Combination Validation
```
1. Extract type = (somid_13 >> 6) & 0x03 (bits 6-7)
2. Extract texture = (somid_13 >> 10) & 0x03 (bits 10-11)
3. Check valid combinations:
   
   Type 00 (Natural) or Type 01 (Scar):
     IF texture = 0b11:
       ERROR: SomidicTextureError("Texture 11 invalid for natural/scar")
   
   Type 11 (Artificial):
     IF texture = 0b11:
       ERROR: SomidicTextureError("Texture 11 invalid for artificial")
   
   Type 10 (Missing/Anomalous):
     ALL textures valid (no check needed)
```

### 2.4 Special Bit Validation (Type=10, Texture=00)
```
IF type = 0b10 AND texture = 0b00: # Missing
  special_bit = (somid_13 >> 12) & 0x01
  IF special_bit ≠ 0:
    ERROR: SomidicSpecialBitError("Special bit must be 0 for missing parts")
```

For each contraquant:

### 2.5 Contraquant Nibble Validation
```
1. Parse hex string to byte value C (2 hex digits → 8-bit integer)
2. Extract zone_nibble = C & 0x0F
3. Extract type_nibble = C & 0xF0
4. IF zone_nibble = 0:
     ERROR: SomidicContraquantError("Zone nibble must be non-zero")
5. IF type_nibble = 0:
     ERROR: SomidicContraquantError("Type nibble must be non-zero")
```

Phase 2 Output: Validated list of somids (13-bit), validated list of contraquant bytes (8-bit)

## Phase 3: Canonicalization Validation

Purpose: Verify somidic is in canonical form

### 3.1 Quant Canonicalization

#### 3.1.1 Sorting Check
```
1. Extract quant values Q_1, Q_2, ..., Q_n
2. FOR i = 1 to n-1:
     IF Q_i >= Q_{i+1}:
       ERROR: SomidicNonCanonicalError("Quants must be in ascending order")
```

#### 3.1.2 Duplication Check
```
1. FOR i = 1 to n-1:
     IF Q_i = Q_{i+1}:
       ERROR: SomidicNonCanonicalError("Duplicate quants not permitted")
```

### 3.2 Contraquant Canonicalization

#### 3.2.1 Maximal Compaction Check
```
1. Expand contraquants to atomic claims:
   FOR each contraquant C_i:
     zone_flags = C_i & 0x0F
     type_flags = C_i >> 4
     Add (zone_flags, type_flags) to claim set S

2. Compute minimal rectangle cover R_min (see Maximal Compaction Algorithm)

3. IF |contraquants| > |R_min|:
     ERROR: SomidicNonCanonicalError("Contraquants not maximally compacted")

4. IF contraquants ≠ R_min (after sorting):
     ERROR: SomidicNonCanonicalError("Contraquants not in canonical form")
```

Note: The Maximal Compaction Algorithm is defined in its own section (see "Maximal Compaction Algorithm").

#### 3.2.2 Contraquant Sorting Check
```
1. Extract contraquant byte values C_1, C_2, ..., C_m
2. FOR i = 1 to m-1:
     IF C_i >= C_{i+1}:
       ERROR: SomidicNonCanonicalError("Contraquants must be in ascending order")
```

#### 3.2.3 Case Normalization Check (Strict mode only)
```
IF strict_mode:
  FOR each contraquant hex string:
    IF contains uppercase letters:
      ERROR: SomidicNonCanonicalError("Hex digits must be lowercase")
```

Phase 3 Output: Confirmation that somidic is canonical

## Complete Validation Algorithm (Normative Pseudocode)

```python
def validate_somidic(somidic_string, mode='standard'):
    """
    Validate a somidic string according to conformance level.
    
    Args:
        somidic_string: String to validate
        mode: 'minimal', 'standard', or 'strict'
    
    Returns:
        (quants, contraquants) if valid
        
    Raises:
        SomidicError: Specific subclass for each error type
    """
    
    # PHASE 1: SYNTAX VALIDATION
    if not somidic_string.startswith('@'):
        raise SomidicSyntaxError("Somidic must start with '@'")
    
    # Remove @ prefix
    s = somidic_string[1:]
    
    # Check for whitespace
    if ' ' in s or '\t' in s or '\n' in s:
        raise SomidicSyntaxError("Whitespace not permitted")
    
    # Split into components
    components = split_components(s)  # Using operators as delimiters
    
    if len(components) == 0:
        raise SomidicSyntaxError("At least one component required")
    
    quants = []
    contraquants = []
    seen_contra = False
    
    for op, value in components:
        if op == '+':
            if seen_contra:
                raise SomidicSyntaxError("Quants must precede contraquants")
            if not is_6_digit_decimal(value):
                raise SomidicSyntaxError("Quant must be exactly 6 digits")
            quants.append(value)
        elif op == '-':
            seen_contra = True
            if not is_2_hex_digits(value):
                raise SomidicSyntaxError("Contraquant must be exactly 2 hex digits")
            contraquants.append(value)
        else:
            raise SomidicSyntaxError(f"Invalid operator '{op}'")
    
    # PHASE 2: SEMANTIC VALIDATION
    validated_quants = []
    for quant_str in quants:
        Q = int(quant_str)
        
        # Handle test plane
        if Q in [999999, 999998]:
            if mode == 'strict' or not TEST_MODE:
                raise SomidicPlaneError("Test plane values not permitted")
            validated_quants.append(Q)
            continue
        
        # CRC validation
        somid = Q >> 5
        checksum = Q & 0x1F
        expected = compute_crc5(somid)
        if checksum != expected:
            raise SomidicChecksumError(f"CRC-5 validation failed for {quant_str}")
        
        # Zone validation
        zone = somid & 0x3F
        if zone > 47:
            raise SomidicZoneError(f"Zone {zone} is reserved")
        
        # Type-texture validation
        type_val = (somid >> 6) & 0x03
        texture = (somid >> 10) & 0x03
        
        if type_val in [0b00, 0b01] and texture == 0b11:
            raise SomidicTextureError("Texture 11 invalid for natural/scar")
        if type_val == 0b11 and texture == 0b11:
            raise SomidicTextureError("Texture 11 invalid for artificial")
        
        # Special bit validation for missing
        if type_val == 0b10 and texture == 0b00:
            special = (somid >> 12) & 0x01
            if special != 0:
                raise SomidicSpecialBitError("Special bit must be 0 for missing")
        
        validated_quants.append(Q)
    
    validated_contraquants = []
    for contra_str in contraquants:
        C = int(contra_str, 16)
        zone_nibble = C & 0x0F
        type_nibble = C & 0xF0
        
        if zone_nibble == 0:
            raise SomidicContraquantError("Zone nibble must be non-zero")
        if type_nibble == 0:
            raise SomidicContraquantError("Type nibble must be non-zero")
        
        validated_contraquants.append((contra_str, C))
    
    # PHASE 3: CANONICALIZATION (skip if minimal mode)
    if mode in ['standard', 'strict']:
        # Check quant sorting
        for i in range(len(validated_quants) - 1):
            if validated_quants[i] >= validated_quants[i+1]:
                if validated_quants[i] == validated_quants[i+1]:
                    raise SomidicNonCanonicalError("Duplicate quants")
                else:
                    raise SomidicNonCanonicalError("Quants not sorted")
        
        # Check contraquant compaction and sorting
        if len(validated_contraquants) > 0:
            contra_bytes = [c[1] for c in validated_contraquants]
            
            # Check maximal compaction
            canonical = maximal_compaction(contra_bytes)
            if len(canonical) != len(contra_bytes) or canonical != sorted(contra_bytes):
                raise SomidicNonCanonicalError("Contraquants not maximally compacted")
            
            # Check sorting
            for i in range(len(contra_bytes) - 1):
                if contra_bytes[i] >= contra_bytes[i+1]:
                    raise SomidicNonCanonicalError("Contraquants not sorted")
        
        # Check case (strict mode only)
        if mode == 'strict':
            for contra_str, _ in validated_contraquants:
                if contra_str != contra_str.lower():
                    raise SomidicNonCanonicalError("Hex must be lowercase")
    
    return validated_quants, [c[1] for c in validated_contraquants]
```

## Exception Hierarchy

All somidic validation errors inherit from a base exception class:

```python
class SomidicError(ValueError):
    """Base exception for all somidic validation errors."""
    pass
```

### Syntax Errors (Phase 1)

```python
class SomidicSyntaxError(SomidicError):
    """Raised when somidic violates grammar rules.
    
    Causes:
    - Missing '@' prefix
    - Invalid component format (not 6 decimal or 2 hex digits)
    - Whitespace present
    - Trailing operators
    - Invalid operator characters
    - Wrong component ordering (contraquants before quants)
    
    User-facing message:
      "This somidic code is incorrectly formatted. Please check it carefully."
    """
    pass
```

### Semantic Errors (Phase 2)

```python
class SomidicChecksumError(SomidicError):
    """Raised when CRC-5 validation fails.
    
    Causes:
    - Quant checksum doesn't match computed CRC-5
    - Indicates typo or corruption
    
    User-facing message:
      "This code appears incorrect. Please check the number carefully."
    """
    pass

class SomidicZoneError(SomidicError):
    """Raised when zone value is invalid.
    
    Causes:
    - Zone 48-63 (reserved zones in public somidic)
    - Zone > 63 (impossible value)
    
    User-facing message:
      "This code references an invalid body zone. Please contact the issuer."
    """
    pass

class SomidicTextureError(SomidicError):
    """Raised when type-texture combination is invalid.
    
    Causes:
    - Type 00/01 (natural/scar) with Texture 11
    - Type 11 (artificial) with Texture 11
    
    User-facing message:
      "This code has an invalid attribute combination. Please contact the issuer."
    """
    pass

class SomidicSpecialBitError(SomidicError):
    """Raised when special bit has invalid value.
    
    Causes:
    - Special bit = 1 for missing parts (Type=10, Texture=00)
    
    User-facing message:
      "This code has an invalid attribute. Please contact the issuer."
    """
    pass

class SomidicContraquantError(SomidicError):
    """Raised when contraquant byte is invalid.
    
    Causes:
    - Zone nibble = 0 (no zones specified)
    - Type nibble = 0 (no types specified)
    
    User-facing message:
      "This code has an invalid exclusion. Please contact the issuer."
    """
    pass

class SomidicPlaneError(SomidicError):
    """Raised when plane value is invalid or disallowed.
    
    Causes:
    - Test plane values (999999, 999998) in production mode
    - Reserved plane values
    
    User-facing message:
      "This code is for testing only and cannot be used."
    """
    pass
```

### Canonical Form Errors (Phase 3)

```python
class SomidicNonCanonicalError(SomidicError):
    """Raised when somidic is valid but not canonical.
    
    Causes:
    - Quants not in ascending order
    - Duplicate quants present
    - Contraquants not maximally compacted
    - Contraquants not in ascending order
    - Hex digits uppercase (strict mode)
    
    Note: This error means the somidic is semantically valid but
    presented in non-standard form.
    
    User-facing message:
      "This code is valid but not in standard form. It should be rewritten as: [canonical form]"
    """
    pass
```

## Validation Decision Tree

```
                    ┌─────────────┐
                    │ Input string│
                    └──────┬──────┘
                           │
                           ▼
                  ┌────────────────┐
                  │ Starts with @? │
                  └────┬───────┬───┘
                       │ No    │ Yes
                       ▼       ▼
                    ERROR    Continue
                           │
                           ▼
                  ┌────────────────┐
                  │ Has whitespace?│
                  └────┬───────┬───┘
                       │ Yes   │ No
                       ▼       ▼
                    ERROR    Continue
                           │
                           ▼
                  ┌────────────────┐
                  │ Valid pattern? │
                  │ (quants/antis) │
                  └────┬───────┬───┘
                       │ No    │ Yes
                       ▼       ▼
                    ERROR    Parse components
                           │
                           ▼
                  ┌────────────────┐
                  │  For each quant│
                  │  Check CRC-5   │
                  └────┬───────┬───┘
                       │ Fail  │ Pass
                       ▼       ▼
                    ERROR    Continue
                           │
                           ▼
                  ┌────────────────┐
                  │  Check zone    │
                  │  (0-47 only)   │
                  └────┬───────┬───┘
                       │ Fail  │ Pass
                       ▼       ▼
                    ERROR    Continue
                           │
                           ▼
                  ┌────────────────┐
                  │Check type-text │
                  │  combinations  │
                  └────┬───────┬───┘
                       │ Fail  │ Pass
                       ▼       ▼
                    ERROR    Continue
                           │
                           ▼
                  ┌────────────────┐
                  │For each contra │
                  │Check nibbles≠0 │
                  └────┬───────┬───┘
                       │ Fail  │ Pass
                       ▼       ▼
                    ERROR    Continue
                           │
                           ▼
                  ┌────────────────┐
                  │If standard mode│
                  │  Check sorted  │
                  └────┬───────┬───┘
                       │ Fail  │ Pass
                       ▼       ▼
                    ERROR    Continue
                           │
                           ▼
                  ┌────────────────┐
                  │If standard mode│
                  │Check compacted │
                  └────┬───────┬───┘
                       │ Fail  │ Pass
                       ▼       ▼
                    ERROR    VALID!
```

## Implementation Notes

### Validation Performance

Expected validation time:
- Phase 1 (Syntax): ~1-2 microseconds (regex matching)
- Phase 2 (Semantic): ~10-50 microseconds per component (CRC computation)
- Phase 3 (Canonical): ~5-20 microseconds (sorting check, compaction check)

Total: ~20-100 microseconds for typical somidic

### Early Exit Optimization

Implementations SHOULD exit on first error (fail-fast) rather than collecting all errors. This provides faster feedback and simpler error reporting.

### Error Message Guidelines

For end users:
- Keep messages simple and actionable
- Don't expose internal validation details
- Suggest next steps ("check the number", "contact issuer")

For developers:
- Include specific error type
- Include position/component where error occurred
- Include actual vs. expected values when helpful

### Test Mode Handling

The special values 999999 and 999998 require careful handling:

```python
# Default: reject test values
validator = SomidicValidator(test_mode=False)  # raises SomidicPlaneError

# Testing: accept test values
validator = SomidicValidator(test_mode=True)   # skips CRC for 999999/999998
```

Production systems SHOULD default to `test_mode=False`.


## Version History

v0.7 (Current) - January 2026
- Consolidated validation into single comprehensive section
- Added three-phase validation model (Syntax/Semantic/Canonicalization)
- Defined conformance levels (Minimal/Standard/Strict)
- Added complete validation algorithm pseudocode
- Added three new exception types (SomidicSpecialBitError, SomidicContraquantError, SomidicPlaneError)
- Added validation decision tree diagram
- Improved error message guidance for implementers

v0.6 - January 2026
- Updated notation: `@` prefix required, `+` for quants, `-` for contraquants
- Separated contraquants (each preceded by `-`)
- Added read-aloud protocol section
- Added formal decoding specification (normative)
- Added rendering/describing guidance (non-normative)
- Improved semantic clarity with set operation interpretation

v0.5 - January 2026
- Added contraquants (8-bit flag encoding)
- Hyphen notation for negative assertions
- Maximal compaction principle
- Override principle for quants
- Verifier discretion principle

v0.3 - January 2026
- Context-dependent bit 12 encoding
- Missing/Anomalous texture repurposing
- Tattoo writing indicator
- Anomalous intensifier

v0.2 - January 2026
- 13-bit somid (increased from 10-bit)
- 48 public zones (increased from 31)
- 5-bit CRC (reduced from 8-bit)
- Multiplicity attribute added

v0.1 (Initial) - January 2026
- 10-bit somid structure
- 32 public human zones
- CRC-8 validation

## Migration from v0.5

Breaking changes: Notation format changed

Old notation (v0.5):
```
147293              # Single quant
@147293:582047      # Multiple quants
147293-41           # With contraquant
-cf                 # Pure contraquant
```

New notation (v0.6):
```
@+147293            # Single quant
@+147293+582047     # Multiple quants
@+147293-41         # With contraquant
@-cf                # Pure contraquant
```

Migration approach:
- Update parsers to recognize new format
- Provide conversion tool for old somidics
- Support both formats during transition period
- Document breaking change clearly

Benefits of new notation:
- Unambiguous in text (always starts with `@`)
- Symmetric operators (`+` and `-`)
- Clearer set operation semantics
- Trivial read-aloud rules
- No ambiguity with negative numbers
