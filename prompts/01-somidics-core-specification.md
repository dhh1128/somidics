# Somidics Core Specification v0.3

## Overview

Somidics is a human-verifiable, equipment-free biometric identification system based on naturally occurring or intentional marks on the human body.

**Version:** 0.3 (January 2026)
**Previous version:** 0.2 (see 06-somidics-v0.2-update.md for v0.1 → v0.2 changes)

## Core Concepts

### Information Flow
```
Physical mark → 13-bit encoding → +CRC-5 → Decimal representation
Somidion     → Somid           → 18-bit  → Somidic
(on body)      (0-8191)         (0-262143) (6 digits)
```

### Key Properties
- **Equipment-free**: No cameras, scanners, or specialized hardware required
- **Human-verifiable**: Ordinary person can check a match without training
- **Memorable**: Holder remembers "mole on left wrist" not "147293"
- **Privacy-preserving**: Fuzzy matching prevents overly strong identification
- **Entropy target**: ~12 bits per somidion (~1-in-4000 discrimination)

## Encoding Specification

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

**Measurement guidelines:**
- For linear marks (scars): measure length
- For round marks (moles): measure diameter
- For irregular marks: approximate longest axis
- Human judgment acceptable - "obviously closer to one category than others"

### Texture Encoding (Bits 10-11)

**Context-dependent meaning based on Type:**

#### For Type=00 (Natural), Type=01 (Scar), Type=11 (Artificial with Texture≠10):

**00: Flush/flat with skin**
- No relief, same level as surrounding skin
- Most freckles, flat moles, healed scars, some tattoos

**01: Raised or depressed**
- Natural 3D feature
- Raised: bump, elevated mole, keloid, wart
- Depressed: dimple, pit, indented scar
- Noticeable by touch or close visual inspection

**10: Tattooed/inked**
- Permanent ink marking in/under skin
- Any color (black, colored, faded)
- Surface is typically flush but visibly marked
- Note: When Type=11 (Artificial), this triggers special bit 12 meaning

**11: Pierced/implanted**
- Hole through skin (with or without jewelry)
- Subdermal implant creating visible bump
- Includes traditional piercings and body modification implants

#### For Type=10 (Missing/Anomalous):

**Texture bits are repurposed to indicate subtype:**

**00: Missing (absent tissue)**
- Missing fingertip, digit, ear portion
- Congenital or acquired absence
- Convention: Use most proximal zone (if whole finger missing, use ring portion zone)

**01: Extra (additional tissue)**
- Webbed fingers (syndactyly with extra tissue)
- Extra digit (polydactyly)
- Cauliflower ear (extra cartilage/tissue)
- Any congenital or acquired extra tissue

**10: Fused (joined parts)**
- Fused fingers (syndactyly without extra tissue)
- Fused toes
- Connected anatomical parts that should be separate

**11: Deformed (misshapen)**
- Bent/crooked finger or toe
- Unusually shaped ear
- Distorted anatomical feature
- Maintains normal count but abnormal shape

### Multiplicity/Special Bit (Bit 12)

**Context-dependent meaning based on Type and Texture:**

#### Type=00 (Natural) or Type=01 (Scar):
**Multiplicity encoding:**
- 0: Single mark
- 1: Cluster of marks (multiple marks grouped together in same zone)

**Examples:**
- "Single mole on left cheek"
- "Cluster of freckles on right forearm"
- "Single scar on left wrist"
- "Cluster of scars on right knee"

#### Type=10 (Missing/Anomalous):

**For Texture=00 (Missing):**
- Bit 12 = 0 (unused, set by convention)
- Missing is binary - either present or absent

**For Texture=01/10/11 (Anomalous subtypes):**
**Intensifier encoding:**
- 0: Mild/subtle (requires close inspection to notice)
- 1: Severe/prominent (obvious from normal social distance)

**Examples:**
- "Mild extra tissue on left hand pinky side" (slight webbing)
- "Severe extra tissue on left hand pinky side" (significant webbing)
- "Mild deformed on right pinky ring portion" (slightly bent)
- "Severe deformed on left ear" (cauliflower ear)

#### Type=11 (Artificial):

**For Texture=10 (Tattooed):**
**Writing indicator:**
- 0: No writing (pure imagery, symbols, patterns)
- 1: Contains writing (words, letters, numbers, text)

**Examples:**
- "Tattoo without writing on left forearm, coin-sized"
- "Tattoo with writing on right shoulder, larger"

**Edge cases:**
- Mixed (image + text): Use 1 (contains writing)
- Single letter/character: Use 1 (it's writing)
- Foreign script: Use 1 (it's writing)
- Ambiguous symbols: Use 0 (imagery)

**For Texture=11 (Pierced):**
**Multiplicity encoding:**
- 0: Single piercing
- 1: Multiple piercings (cluster)

**Examples:**
- "Single piercing in right ear"
- "Multiple piercings in left ear"

**For Texture=00 or 01 (Other artificial):**
**Multiplicity encoding:**
- 0: Single mark
- 1: Cluster of marks

**Examples:**
- "Single implant on left forearm, raised"
- "Cluster of scarification marks on right upper arm, raised"

## CRC-5 Specification

### Algorithm
CRC-5-USB with polynomial 0x05 (x^5 + x^2 + 1)

**Parameters:**
- Polynomial: 0x05
- Width: 5 bits
- Initial value: 0x1F
- Final XOR: 0x1F
- Input reflection: true
- Output reflection: true

**Computation:**
```
Input: 13-bit somid (bits 0-12)
Output: 5-bit CRC (bits 0-4)

Process somid bits with reflection
```

### Purpose
- Error detection: Catches transcription errors
- Invalid value rejection: ~96.9% of random 6-digit numbers are invalid (31/32)
- Security: Makes brute-force search for matching somidics harder

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
- Possibly non-terrestrial life forms
- Or extended human/animal encoding schemes

**Plane 3: Special Values (786432-999999)**
- Partial plane (213,568 values)
- Test and special-purpose somidics
- **999999: Always match** (test value)
- **999998: Never match** (test value)
- 999997-786432: Reserved for future special values

## Encoding Process

### From Somidion to Somidic

**Step 1: Identify somidion attributes**
- Zone: Which body part? (Consider finger portions, hand sides)
- Type: Natural, scar, missing/anomalous, artificial?
- Size: Grain, fingernail, coin, larger?
- Texture: Depends on type (see context-dependent encoding)
- Multiplicity/Special: Depends on type and texture (see context-dependent encoding)

**Step 2: Encode as somid (13 bits)**
```python
bits[0:5]   = zone_value        (0-63, only 0-47 defined)
bits[6:7]   = type_value        (0-3)
bits[8:9]   = size_value        (0-3)
bits[10:11] = texture_value     (0-3, meaning depends on type)
bits[12]    = special_value     (0-1, meaning depends on type/texture)

somid = (zone << 7) | (type << 5) | (size << 3) | (texture << 1) | special
```

**Step 3: Compute CRC-5**
```python
crc = compute_crc5_usb(somid)  # 5 bits
```

**Step 4: Combine into 18-bit value**
```python
combined = (somid << 5) | crc
```

**Step 5: Add plane offset and format**
```python
plane_offset = 0  # For human somidics
decimal_value = plane_offset + combined
somidic = format("%06d", decimal_value)  # Zero-pad to 6 digits
```

## Decoding Process

### From Somidic to Somidion Description

**Step 1: Parse and validate**
```python
value = parse_decimal(somidic_string)

if value >= 786432:
    return handle_special_value(value)

plane = value // 262144
offset = value % 262144
```

**Step 2: Extract components**
```python
crc_received = offset & 0x1F          # Last 5 bits
somid = (offset >> 5) & 0x1FFF        # First 13 bits
```

**Step 3: Validate CRC**
```python
crc_computed = compute_crc5_usb(somid)

if crc_computed != crc_received:
    return "INVALID_CHECKSUM"
```

**Step 4: Decode somid**
```python
zone = (somid >> 7) & 0x3F      # 6 bits
type = (somid >> 5) & 0x03      # 2 bits
size = (somid >> 3) & 0x03      # 2 bits
texture = (somid >> 1) & 0x03   # 2 bits
special = somid & 0x01          # 1 bit
```

**Step 5: Convert to human-readable description**
```python
zone_name = ZONE_MAP[zone]
size_name = SIZE_MAP[size]

# Context-dependent interpretation
if type == NATURAL or type == SCAR:
    texture_name = TEXTURE_MAP[texture]
    multiplicity = "cluster of" if special else "single"
    return f"{multiplicity} {texture_name} {TYPE_MAP[type]} on {zone_name}, {size_name}"

elif type == MISSING_ANOMALOUS:
    if texture == MISSING:
        return f"Missing on {zone_name}"
    else:
        anomaly_type = ["extra tissue", "fused", "deformed"][texture - 1]
        severity = "severe" if special else "mild"
        return f"{severity.capitalize()} {anomaly_type} on {zone_name}"

elif type == ARTIFICIAL:
    if texture == TATTOOED:
        writing = "with writing" if special else "without writing"
        return f"Tattoo {writing} on {zone_name}, {size_name}"
    elif texture == PIERCED:
        multiplicity = "multiple piercings" if special else "single piercing"
        return f"{multiplicity} in {zone_name}"
    else:
        texture_name = TEXTURE_MAP[texture]
        multiplicity = "cluster of" if special else "single"
        return f"{multiplicity} {texture_name} artificial mark on {zone_name}, {size_name}"
```

**Example outputs:**
- "Single raised natural mark on left forearm, coin-sized"
- "Tattoo with writing on right shoulder, larger"
- "Missing on right index ring portion"
- "Severe deformed on left ear"
- "Multiple piercings in right ear"

## Multiple Somidics (Somidics Sets)

### Notation
Multiple somidics use @ prefix and colon separators:

**Format:** `@somidic1:somidic2:somidic3`

**Example:** `@147293:582047:923841`

### Canonical Ordering
Somidics in a set must always be sorted numerically ascending:

**Incorrect:** `@582047:147293`
**Correct:** `@147293:582047`

### Purpose
- Increased discrimination: Two somidics = ~1-in-16-million
- Credential deduplication: Sets always compared in canonical form
- Flexibility: Different credentials can require different numbers

### Regex Patterns
**Single somidic:** `\b\d{6}\b`
**Multiple somidics:** `@\d{6}(:\d{6})+`

## Security Properties

### Entropy Analysis (v0.3)

**Single somidic (theoretical maximum):**
- Zone: log₂(48) = 5.58 bits
- Type: 2 bits
- Size: 2 bits
- Texture: 2 bits (context-dependent)
- Special: 1 bit (context-dependent)
- **Total if independent: 12.58 bits**

**Actual entropy (accounting for dependencies):**
- Zone distribution non-uniformity: -0.30 bits
- Type↔Texture correlation: -0.20 bits
- Size distribution skew: -0.08 bits
- Other correlations: -0.10 bits
- Unused zone space: -0.42 bits
- **Estimated actual: ~11.5 bits base**

**Gains from v0.3 enhancements:**
- Tattoo writing indicator: +0.12 bits average
- Anomalous intensifier: +0.06 bits average
- **v0.3 effective entropy: ~11.7 bits**

**Effective discrimination: 2^11.7 ≈ 3,300 possibilities**

Note: This is more conservative than v0.2's estimate due to refined correlation analysis, but v0.3 enhancements partially recover the loss.

**Multiple somidics:**
- Two independent somidics: ~23 bits = 1-in-8-million
- Three independent somidics: ~35 bits = 1-in-34-billion

### Attack Resistance

**Credit card theft scenario:**
- Thief needs mark in same zone
- Same type
- Similar size
- Similar texture/subtype
- Similar special attribute
- **Combined: ~1/3,300 chance with single somidic**

**Impersonation attempts:**
- Temporary tattoo: Requires knowledge of victim's somidic, harder with writing indicator
- Makeup: Limited to surface marks, requires knowledge
- Finding someone with match: ~1/3,300 people
- Much stronger than PIN alone (which can be observed/guessed)

### Limitations
**Not suitable for:**
- Legal proof of identity (too fuzzy)
- High-security authentication alone (use proper biometrics)
- High-throughput scenarios (human verification is slow)

**Best for:**
- Casual fraud prevention (credit cards, low-value transactions)
- Low-resource identification (refugee camps, rural areas)
- Backup/fallback identification
- Child identification (when equipment unavailable)
- Equipment-free verification scenarios

## Implementation Notes

### Somid Validation
Before computing CRC, validate somid components:
- Zone must be 0-63 (only 0-47 defined for public use)
- Type must be 0-3
- Size must be 0-3
- Texture must be 0-3
- Special must be 0-1
- If Type=10 (Missing/Anomalous) and Texture=00 (Missing), Special should be 0

### CRC-5 Library Selection
Use CRC-5-USB implementations:
- Python: `crcmod` library with poly=0x05, or manual implementation
- JavaScript: Custom implementation (few libraries support CRC-5-USB)
- Java: Custom implementation
- C/C++: Custom implementation (polynomial 0x05)

**Reference implementation needed:** CRC-5-USB is less common than CRC-8, so reference implementations should be provided.

### Error Messages
**User-facing:**
- "Invalid somidic - please check the number"
- "This somidic has an invalid checksum"
- "Somidic not recognized"

**Developer/debug:**
- "CRC mismatch: expected XX, got YY"
- "Invalid zone value: 64 (must be 0-63)"
- "Somidic outside valid plane range"
- "Reserved zone used (48-63)"

## Version History

**v0.3 (Current)** - January 2026
- Context-dependent bit 12 encoding:
  - Tattoo writing indicator
  - Anomalous intensifier
  - Maintains multiplicity for natural, scar, and non-tattoo artificial marks
- Missing/Anomalous texture repurposing (4 subtypes: missing, extra, fused, deformed)
- Improved semantic clarity for anomalous features
- Refined entropy analysis with better correlation modeling
- All other aspects unchanged from v0.2 (13-bit somid, 48 zones, CRC-5)

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
- See 06-somidics-v0.2-update.md for v0.1 → v0.2 changes

## Migration from v0.2

**Breaking changes:**
- Context-dependent bit 12 encoding
- Missing/Anomalous texture repurposing
- Tattoo writing indicator

**For systems using v0.2:**
- Re-encoding required for:
  - Tattoos (to capture writing indicator)
  - Anomalous features (to capture intensifier)
  - Missing parts (texture must be set to 00)
- Natural marks and scars mostly unchanged (multiplicity already existed)
- Can detect v0.2 vs v0.3 by checking semantic consistency of decoded values

**Recommendation:** Re-issue credentials with v0.3 encoding when practical
