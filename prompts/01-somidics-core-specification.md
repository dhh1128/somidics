# Somidics Core Specification v0.4

## Overview

Somidics is a human-verifiable, equipment-free biometric identification system based on naturally occurring or intentional marks on the human body. Version 0.4 adds **anti-somidics** - negative assertions about marks NOT present on the body.

**Version:** 0.4 (January 2026)
**Previous versions:** 0.3, 0.2, 0.1 (see update documents for changes)

## Core Concepts

### Information Flow

**Positive Somidics:**
```
Physical mark → 13-bit encoding → +CRC-5 → Decimal representation
Somidion     → Somid           → 18-bit  → Somidic
(on body)      (0-8191)         (0-262143) (6 digits)
```

**Anti-Somidics (NEW in v0.4):**
```
Absence claim → 8-bit encoding → Hex representation
Anti-somidion → Zone+Type flags → Anti-somidic
(not on body)   (0-255)          (2 hex digits)
```

**Combined Notation:**
```
147293-41
  │     │
  │     └─ Anti-somidic: "No tattoos on hands+wrists"
  └─────── Positive somidic: "Mole on left wrist"
```

### Key Properties
- **Equipment-free**: No cameras, scanners, or specialized hardware required
- **Human-verifiable**: Ordinary person can check a match without training
- **Memorable**: Holder remembers "mole on left wrist" not "147293"
- **Privacy-preserving**: Fuzzy matching prevents overly strong identification
- **Optional discrimination**: Anti-somidics provide stronger ID at verifier's discretion
- **Entropy target**: ~12 bits per somidion, 5-20× boost with anti-somidics

## Positive Somidics Encoding

### Somid Structure (13 bits)

A somid is a 13-bit value encoding a somidion's characteristics:

**Bits 0-5: Zone (6 bits)** - Body location (64 possible zones, 48 defined)
**Bits 6-7: Type (2 bits)** - Category of mark
**Bits 8-9: Size (2 bits)** - Measured by longest dimension
**Bits 10-11: Texture (2 bits)** - Surface characteristic or anomaly subtype
**Bit 12: Multiplicity/Special (1 bit)** - Context-dependent meaning

### Zone Encoding (Bits 0-5)

#### Public Human Zones (48 defined zones)

**Fingers (20 zones): 0-19**
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

**Hands (8 zones): 20-27**
Each palm/back is subdivided into thumb-side and pinky-side:
- 20: Left palm - thumb side
- 21: Left palm - pinky side
- 22: Left back - thumb side
- 23: Left back - pinky side
- 24: Right palm - thumb side
- 25: Right palm - pinky side
- 26: Right back - thumb side
- 27: Right back - pinky side

**Arms (6 zones): 28-33**
- 28: Left upper arm
- 29: Right upper arm
- 30: Left forearm
- 31: Right forearm
- 32: Left wrist
- 33: Right wrist

**Face (10 zones): 34-43**
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

**Ears & Neck (4 zones): 44-47**
- 44: Left ear
- 45: Right ear
- 46: Neck front
- 47: Neck side/back

**Reserved (16 zones): 48-63**
- Reserved for private body parts or future expansion
- Not used in public somidics

### Type Encoding (Bits 6-7)

**00: Natural mark**
- Mole, birthmark, freckle, discoloration, skin patch, dimple
- Present at birth or developed naturally over time

**01: Scar**
- Linear, patch, or irregular scar
- From accident, surgery, or illness
- Permanent scarring only (not recent wounds)

**10: Missing/Anomalous**
- Missing: Absent tissue (missing fingertip, digit, ear portion)
- Anomalous: Extra tissue, fused parts, deformed features
- See Texture encoding for subtype distinction

**11: Artificial/Intentional**
- Tattoo, piercing, implant, scarification, branding
- Deliberately created body modification

### Size Encoding (Bits 8-9)

Size measured by **longest dimension**:

**00: Grain-sized**
- Comparable to rice grain or wheat grain
- Approximately 3-7mm
- Small enough to require close inspection

**01: Fingernail-sized**
- Comparable to pinky fingernail
- Approximately 8-15mm
- Clearly visible when looking at area

**10: Coin-sized**
- Comparable to common coin diameter
- Approximately 15-25mm
- Noticeable from normal social distance

**11: Larger than coin**
- Greater than 25mm in longest dimension
- Obvious, prominent mark

### Texture Encoding (Bits 10-11)

**Context-dependent meaning based on Type:**

#### For Type=00 (Natural), Type=01 (Scar), Type=11 (Artificial with Texture≠10):

**00: Flush/flat with skin**
- No relief, same level as surrounding skin

**01: Raised or depressed**
- Natural 3D feature
- Raised: bump, elevated mole, keloid, wart
- Depressed: dimple, pit, indented scar

**10: Tattooed/inked**
- Permanent ink marking in/under skin

**11: Pierced/implanted**
- Hole through skin or subdermal implant

#### For Type=10 (Missing/Anomalous):

**00: Missing (absent tissue)**
**01: Extra (additional tissue)**
**10: Fused (joined parts)**
**11: Deformed (misshapen)**

### Multiplicity/Special Bit (Bit 12)

**Context-dependent meaning based on Type and Texture** - see full specification in v0.3 document for details.

## Anti-Somidics Encoding (NEW in v0.4)

### Anti-Somidic Structure (8 bits = 2 hex digits)

An anti-somidic encodes a negative assertion: "No marks of type(s) X in zone(s) Y"

**Bits 0-3: Zone flags (4 bits)**
**Bits 4-7: Type flags (4 bits)**

Both zones and types use **FLAG encoding** (not enumeration), allowing combinations.

### Anti-Zone Flags (Bits 0-3)

Each bit represents a broad zone category:

**Bit 0: Hands + Fingers + Wrists**
- Covers positive zones: 0-19 (fingers), 20-27 (hands), 32-33 (wrists)
- Total: 30 positive zones collapsed into 1 anti-zone
- **Key feature:** Can verify even with long sleeves

**Bit 1: Face**
- Covers positive zones: 34-43 (all facial zones)
- Total: 10 positive zones

**Bit 2: Ears**
- Covers positive zones: 44-45 (both ears)
- Total: 2 positive zones

**Bit 3: Neck**
- Covers positive zones: 46-47 (neck front and side/back)
- Total: 2 positive zones

**Note:** Arms (positive zones 28-31) are intentionally NOT covered by any anti-zone for modesty reasons.

### Anti-Type Flags (Bits 4-7)

Each bit represents a mark type category:

**Bit 4: Natural marks**
- Moles, birthmarks, freckles, discolorations

**Bit 5: Scars**
- All types of scars

**Bit 6: Tattoos**
- All tattooed/inked marks

**Bit 7: Piercings/Implants**
- All piercings and implants

### Flag Encoding Examples

**0x41 (binary: 01000001)**
```
Zones: 0001 = Hands+fingers+wrists only
Types: 0100 = Tattoos only
Meaning: "No tattoos on hands+fingers+wrists"
```

**0x43 (binary: 01000011)**
```
Zones: 0011 = Hands+wrists + Face
Types: 0100 = Tattoos only
Meaning: "No tattoos on hands+wrists or face"
```

**0xC1 (binary: 11000001)**
```
Zones: 0001 = Hands+wrists only
Types: 1100 = Tattoos + Piercings
Meaning: "No tattoos or piercings on hands+wrists"
```

**0xFF (binary: 11111111)**
```
Zones: 1111 = All zones
Types: 1111 = All types
Meaning: "No marks of any kind anywhere in public zones"
```

### Common Anti-Somidics Reference

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

**Anti-somidics are always maximally compacted** to minimize count.

Since both zones AND types are flags (bitmasks), you combine:
- Zone bits for marks with same type constraints
- Type bits for marks with same zone constraints

**Example:**
```
Input:  -41428182
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

**Maximum anti-somidics in canonical form: 4**

This only occurs with 4 different zone patterns for the 4 type dimensions - very rare in practice.

**Typical usage:**
- Most credentials: 0-1 anti-somidics
- Common: 1 anti-somidic (e.g., `4f` or `cf`)
- Rare: 2-3 anti-somidics
- Very rare: 4 anti-somidics

## CRC-5 Specification (Positive Somidics Only)

### Algorithm
CRC-5-USB with polynomial 0x05 (x^5 + x^2 + 1)

**Parameters:**
- Polynomial: 0x05
- Width: 5 bits
- Initial value: 0x1F
- Final XOR: 0x1F
- Input reflection: true
- Output reflection: true

**Note:** Anti-somidics do NOT use CRC validation (only 256 possible values, both nibbles must be non-zero).

## Plane Architecture

The 6-digit decimal space (000000-999999) is divided into planes:

### Plane Structure
**Plane size:** 262,144 values (2^18)
- Each plane contains 8,192 unique somids (2^13)
- Each somid has 32 possible CRC values (2^5)
- Only 1 CRC value per somid is valid

### Plane Allocation

**Plane 0: Human Somidics (000000-262143)**
- All human somidions (public and reserved zones)
- Public zones: bits 0-5 = 0-47
- Reserved zones: bits 0-5 = 48-63

**Plane 1: Animal Somidics (262144-524287)**
- Reserved for future animal specification
- Different attribute encoding appropriate for animals
- May include species, gender, breed as attributes

**Plane 2: Reserved (524288-786431)**
- Future expansion

**Plane 3: Special Values (786432-999999)**
- Partial plane (213,568 values)
- Test and special-purpose somidics
- **999999: Always match** (test value)
- **999998: Never match** (test value)

**Note:** Anti-somidics use a separate encoding space (hex notation), not decimal plane space.

## Complete Notation Specification

### Notation Grammar

```
somidic_set := positive_part [anti_part]
             | anti_part

positive_part := '@'? decimal6 (':' decimal6)*
anti_part := '-' hex_byte+

decimal6 := [0-9]{6}
hex_byte := [0-9a-f]{2}
```

### Examples

**Positive only (v0.3 compatible):**
```
147293              Single positive somidic
@147293:582047      Two positive somidics
```

**Positive with anti (v0.4):**
```
147293-41           Single positive, single anti
147293-4143         Single positive, two antis (compacts to -43)
@147293:582047-cf   Two positives, one anti
```

**Anti only (v0.4):**
```
-cf                 Pure anti-somidic (no positives)
-ff                 No marks of any kind anywhere
```

### Canonical Ordering Rules

**For positive somidics:**
1. Sort numerically ascending
2. Remove duplicates
3. Prefix with @ if multiple

**For anti-somidics:**
1. Maximally compact (combine zones and types)
2. Sort numerically ascending (as 8-bit values)
3. Remove duplicates (automatic after compaction)

**Overall format:**
- Positives (if any), then hyphen, then antis (if any)
- Hyphen required only if antis present

### Validation Rules

**Positive somidics:**
- Must be 6-digit decimal with valid CRC-5
- Zone must be 0-47 (public zones)

**Anti-somidics:**
- Must be pairs of hex digits (even character count)
- Each byte must have both nibbles non-zero
  - Invalid: 00, 01-0f, 10-f0 (meaningless assertions)
  - Valid: 11-ff (excluding x0 and 0x patterns)
- Must be in maximally compacted canonical form

## Encoding Process

### From Somidion to Positive Somidic

(Same as v0.3 - see full specification for details)

### From Anti-Somidion to Anti-Somidic

**Step 1: Identify exclusion zones**
- Which broad zones to exclude? (hands+wrists, face, ears, neck)
- Set corresponding zone bits

**Step 2: Identify exclusion types**
- Which mark types to exclude? (natural, scars, tattoos, piercings)
- Set corresponding type bits

**Step 3: Combine into 8-bit value**
```python
anti_value = zone_flags | (type_flags << 4)
anti_hex = format(anti_value, '02x')
```

**Step 4: Compact with other anti-somidics**
```python
# If multiple anti-somidics, maximally compact
all_antis = [anti1, anti2, ...]
compacted = maximally_compact(all_antis)
anti_string = ''.join(format(a, '02x') for a in compacted)
```

**Step 5: Combine with positive somidics**
```python
if positives and antis:
    result = positive_string + '-' + anti_string
elif antis only:
    result = '-' + anti_string
else:
    result = positive_string
```

## Decoding Process

### From Somidic Set to Description

**Step 1: Parse notation**
```python
if '-' in somidic_string:
    positive_part, anti_part = somidic_string.split('-', 1)
else:
    positive_part = somidic_string
    anti_part = None
```

**Step 2: Decode positive somidics**
```python
if positive_part:
    positives = [decode_positive(p) for p in positive_part.lstrip('@').split(':')]
```

**Step 3: Decode anti-somidics**
```python
if anti_part:
    # Parse pairs of hex digits
    antis = []
    for i in range(0, len(anti_part), 2):
        anti_value = int(anti_part[i:i+2], 16)
        antis.append(decode_anti(anti_value))
```

**Step 4: Generate description**
```python
description = {
    'positives': [
        "Single raised mole on left wrist, fingernail-sized",
        "Scar on left cheek, coin-sized"
    ],
    'antis': [
        "No tattoos or piercings anywhere in public zones"
    ]
}
```

### Anti-Somidic Decoding

```python
def decode_anti_somidic(anti_value):
    """
    anti_value: 8-bit integer (0x11 through 0xFF, excluding x0 and 0x)
    returns: human-readable description
    """
    zone_flags = anti_value & 0x0F
    type_flags = (anti_value >> 4) & 0x0F
    
    # Decode zone flags
    zones = []
    if zone_flags & 0x01: zones.append('hands+fingers+wrists')
    if zone_flags & 0x02: zones.append('face')
    if zone_flags & 0x04: zones.append('ears')
    if zone_flags & 0x08: zones.append('neck')
    
    # Decode type flags
    types = []
    if type_flags & 0x01: types.append('natural marks')
    if type_flags & 0x02: types.append('scars')
    if type_flags & 0x04: types.append('tattoos')
    if type_flags & 0x08: types.append('piercings')
    
    zone_desc = ' or '.join(zones)
    type_desc = ' or '.join(types)
    
    return f"No {type_desc} on {zone_desc}"
```

## Override Principle

### Positive Somidics Are Exceptions

**Key rule:** Positive somidics always override anti-somidics in overlapping zones.

**Example:**
```
147293-41
  Positive: "Tattoo with writing on left wrist" (zone 32)
  Anti: "No tattoos on hands+fingers+wrists"
  
Zone 32 (left wrist) is IN anti-zone 0 (hands+fingers+wrists).

Interpretation: "I have THIS specific tattoo on my left wrist,
                 but no OTHER tattoos on hands, fingers, or wrists"
```

**Verification process:**
1. Verify the specific positive somidic (tattoo on left wrist) ✓
2. Scan the rest of hands+fingers+wrists for OTHER tattoos ✓
3. Accept if both conditions met

**Implementation:**
```python
def verify_anti_somidic(person, anti_value, positive_zones):
    """
    anti_value: 8-bit anti-somidic
    positive_zones: list of zones claimed in positive somidics
    """
    anti_zones = expand_anti_zone_flags_to_positive_zones(anti_value & 0x0F)
    anti_types = extract_type_flags(anti_value >> 4)
    
    # Remove zones already claimed in positives (exceptions)
    zones_to_check = [z for z in anti_zones if z not in positive_zones]
    
    # Verify no marks of anti_types exist in zones_to_check
    return not has_marks(person, zones_to_check, anti_types)
```

## Verifier Discretion Principle

### Critical Design Principle

**Anti-somidics are ALWAYS optional from the verifier's perspective.**

The verifier decides whether checking anti-somidics is worth the extra time based on:
- Transaction value
- Fraud risk
- Time constraints
- Context

**Low-security context:**
```
Credential: 147293-cf
Verifier checks: Just positive somidic (30 seconds)
Ignores: Anti-somidic
```

**High-security context:**
```
Credential: 147293-cf
Verifier checks: Positive AND anti (120 seconds)
```

**This means:**
- Credential holder encodes once
- Verifier pays cost only when justified
- System degrades gracefully
- Context-appropriate security

## Multiple Somidics (Somidics Sets)

### Notation
Multiple somidics use @ prefix and colon separators for positives:

**Format:** `@somidic1:somidic2:somidic3-anti1anti2`

**Example:** `@147293:582047:923841-cf`

### Purpose
- Increased discrimination with multiple positives
- Optional additional discrimination with antis
- Flexibility for different security contexts

## Security Properties

### Entropy Analysis (v0.4)

**Single positive somidic:**
- Effective entropy: ~11.7 bits
- Discrimination: ~1-in-3,300

**With one anti-somidic:**
- Anti entropy: ~2-3 bits (depends on population)
- Combined: ~14-15 bits
- Discrimination: ~1-in-15,000 to 1-in-30,000

**Multiple somidics:**
- Two positives: ~23 bits = 1-in-8-million
- Two positives + anti: ~26 bits = 1-in-60-million
- Three positives + anti: ~38 bits = 1-in-250-billion

### Attack Resistance

**Credit card theft with anti-somidic:**
- Thief needs mark in correct zone (1/3,300)
- AND needs absence of marks in anti-zone (1/5 - 1/3 depending on type)
- Combined: 1/15,000 to 1/100,000

Much stronger than PIN (1/10,000) which can be observed.

### Limitations
**Not suitable for:**
- Legal proof of identity (too fuzzy)
- High-security authentication alone
- High-throughput scenarios

**Best for:**
- Casual to medium fraud prevention
- Equipment-free contexts
- Privacy-preserving identification
- Backup/fallback authentication

## Implementation Notes

### Anti-Somidic Validation

```python
def validate_anti_somidic_byte(value):
    """
    Validate a single 8-bit anti-somidic
    """
    zone_nibble = value & 0x0F
    type_nibble = value & 0xF0
    
    if zone_nibble == 0:
        raise ValueError("Zone nibble must be non-zero")
    if type_nibble == 0:
        raise ValueError("Type nibble must be non-zero")
    
    return True
```

### Maximal Compaction Algorithm

```python
def maximally_compact_anti_somidics(antis):
    """
    Compact anti-somidics to minimum count
    """
    changed = True
    anti_tuples = [(a & 0x0F, a & 0xF0) for a in antis]
    
    while changed:
        changed = False
        new_tuples = []
        used = set()
        
        for i, (z1, t1) in enumerate(anti_tuples):
            if i in used:
                continue
            
            combined_z = z1
            combined_t = t1
            
            for j, (z2, t2) in enumerate(anti_tuples[i+1:], i+1):
                if j in used:
                    continue
                
                # Same zones? Combine types
                if z1 == z2:
                    combined_t |= t2
                    used.add(j)
                    changed = True
                
                # Same types? Combine zones
                elif t1 == t2:
                    combined_z |= z2
                    used.add(j)
                    changed = True
            
            new_tuples.append((combined_z, combined_t))
            used.add(i)
        
        anti_tuples = new_tuples
    
    result = [z | t for z, t in anti_tuples]
    return sorted(result)
```

## Version History

**v0.4 (Current)** - January 2026
- Added anti-somidics (8-bit flag encoding)
- Hyphen notation for negative assertions
- Maximal compaction principle
- Override principle for positives
- Verifier discretion principle
- All positive somidic features unchanged from v0.3

**v0.3** - January 2026
- Context-dependent bit 12 encoding
- Missing/Anomalous texture repurposing
- Tattoo writing indicator
- Anomalous intensifier
- See 06-somidics-v0.3-update.md for details

**v0.2** - January 2026
- 13-bit somid (increased from 10-bit)
- 48 public zones (increased from 31)
- 5-bit CRC (reduced from 8-bit)
- Multiplicity attribute added
- See 06-somidics-v0.2-update.md for details

**v0.1 (Initial)** - January 2026
- 10-bit somid structure
- 32 public human zones
- CRC-8 validation

## Migration from v0.3

**Breaking changes:** None - anti-somidics are purely additive

**Backward compatibility:**
- v0.3 credentials remain valid (no hyphen = no antis)
- v0.4 parsers can read v0.3 credentials
- v0.3 parsers need update to support hyphen notation

**Recommended approach:**
- New credentials can use v0.4 format with anti-somidics
- Existing v0.3 credentials continue to work
- Optional: Upgrade high-value credentials to add anti-somidics
