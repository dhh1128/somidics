# Somidics Terminology & Notation Reference v0.4

## Core Terms

### Somidion
**Definition:** A mark on or feature of a person's body that has a specific location, is persistent rather than ephemeral, is characteristic rather than incidental, is highly unique, and can be recognized with ordinary human senses based on a simple description.

**Etymology:** From Greek *soma* (body) + *idio* (personal, unique)

**Pronunciation:** /soʊˈmɪdiən/ (so-MID-ee-ən)

**Part of speech:** Noun, countable

**Plural:** somidions

**Usage examples:**
- "She has a somidion on her left wrist" (the actual mark)
- "The typical person has dozens of somidions on their body"
- "A birthmark can be a somidion"
- "Choose a somidion you'd be willing to show a stranger"

**What qualifies as a somidion:**
✓ Mole, birthmark, freckle, discoloration
✓ Scar (permanent)
✓ Tattoo, piercing, implant
✓ Missing part (fingertip, etc.)
✓ Dimple, raised mole, unusual anatomical feature

**What does NOT qualify:**
✗ Hairstyle (easily changed)
✗ Temporary marks (bruise, recent wound)
✗ Biometrics requiring equipment (fingerprint, iris pattern)
✗ Features everyone has (two eyes, nose)
✗ Clothing choices

### Anti-Somidion (NEW in v0.4)
**Definition:** A claimed absence of marks of specific type(s) in specific zone(s) on a person's body.

**Etymology:** From Greek *anti* (against, opposite) + *soma* (body) + *idio* (personal, unique)

**Pronunciation:** /ˌæntisoʊˈmɪdiən/ (AN-tee-so-MID-ee-ən)

**Part of speech:** Noun, countable

**Plural:** anti-somidions

**Usage examples:**
- "Her anti-somidion is 'no tattoos on hands or face'"
- "Anti-somidions provide additional discrimination"
- "The credential includes anti-somidions for stronger identification"

**What qualifies as an anti-somidion:**
✓ No natural marks in zone(s)
✓ No scars in zone(s)
✓ No tattoos in zone(s)
✓ No piercings in zone(s)
✓ Combinations of zones and types

**What does NOT qualify:**
✗ Absence of hairstyle (not meaningful)
✗ Absence of temporary marks (changes)
✗ Absence of universal features

### Somid
**Definition:** The 13-bit encoded representation of a somidion's characteristics (zone + type + size + texture + special), before CRC is computed.

**Etymology:** Shortened from somidion, technical term

**Pronunciation:** /ˈsoʊmɪd/ (SO-mid)

**Part of speech:** Noun, countable

**Plural:** somids

**Value range:** 0-8191 (2^13)

**Usage examples:**
- "The somid encodes zone, type, size, texture, and special bit"
- "Valid somids range from 0 to 8191"
- "Each somid corresponds to a specific combination of attributes"
- "The somid is passed to the CRC-5 function"

**Technical usage only:**
- Appears in specifications, not user-facing text
- Users never see or work with somids directly
- Intermediate representation between somidion and somidic

**Bit structure (v0.3+):**
```
Bits 0-5: Zone (6 bits, 48 defined)
Bits 6-7: Type (2 bits)
Bits 8-9: Size (2 bits)
Bits 10-11: Texture (2 bits, context-dependent)
Bit 12: Multiplicity/Special (1 bit, context-dependent)
```

### Anti-Somid (NEW in v0.4)
**Definition:** The 8-bit encoded representation of an anti-somidion's characteristics (zone flags + type flags).

**Etymology:** From *anti* + *somid*

**Pronunciation:** /ˈæntisoʊmɪd/ (AN-tee-SO-mid)

**Part of speech:** Noun, countable

**Plural:** anti-somids

**Value range:** 0x11-0xFF (excluding bytes with nibble = 0)

**Usage examples:**
- "The anti-somid uses flag encoding for zones and types"
- "Anti-somids are represented as 2 hex digits"
- "This anti-somid combines multiple zones and types"

**Technical usage only:**
- Appears in specifications
- Users see hex representation, not the term "anti-somid"

**Bit structure:**
```
Bits 0-3: Zone flags (4 bits)
  Bit 0: Hands+fingers+wrists
  Bit 1: Face
  Bit 2: Ears
  Bit 3: Neck

Bits 4-7: Type flags (4 bits)
  Bit 4: Natural marks
  Bit 5: Scars
  Bit 6: Tattoos
  Bit 7: Piercings/implants
```

### Somidic
**Definition (noun):** The 6-digit decimal number that represents a somidion, including CRC-5 checksum for validation.

**Definition (adjective):** Relating to somidion-based identification and authentication.

**Etymology:** From somidion, paralleling "biometric/biometrics"

**Pronunciation:** /soʊˈmɪdɪk/ (so-MID-ik)

**Part of speech:** 
- Noun (countable): "a somidic," "two somidics"
- Adjective: "somidic authentication"

**As noun - Value range:** 000000-999999 (6 decimal digits, zero-padded)

**Usage examples (noun):**
- "Enter your somidic: 147293"
- "This credential contains somidics @147293:582047"
- "Each somidic includes a CRC-5 checksum"
- "The somidic 147293 encodes a mark on the left forearm"

**Usage examples (adjective):**
- "somidic authentication system"
- "somidic verification process"
- "somidic-protected credential"
- "somidic enrollment procedure"

**Structure:**
- 18-bit value: (somid << 5) | CRC-5
- Plus plane offset (0 for humans)
- Converted to decimal, zero-padded to 6 digits

### Anti-Somidic (NEW in v0.4)
**Definition (noun):** The 2-hex-digit representation of an anti-somidion, encoding zone and type flags.

**Definition (adjective):** Relating to negative assertions about body marks.

**Etymology:** From *anti* + *somidic*

**Pronunciation:** /ˌæntisoʊˈmɪdɪk/ (AN-tee-so-MID-ik)

**Part of speech:**
- Noun (countable): "an anti-somidic," "two anti-somidics"
- Adjective: "anti-somidic assertion"

**As noun - Value range:** 0x11-0xFF (2 hex digits, both nibbles non-zero)

**Usage examples (noun):**
- "The anti-somidic 41 means 'no tattoos on hands+wrists'"
- "This credential includes anti-somidics -cf"
- "Anti-somidics provide optional discrimination enhancement"

**Usage examples (adjective):**
- "anti-somidic verification requires scanning zones"
- "anti-somidic protection enhances security"

**Structure:**
- 8-bit value: zone_flags | (type_flags << 4)
- Represented as 2 hex digits
- No CRC (only 256 possible values, both nibbles must be non-zero)

### Somidics
**Definition:** The field of study or practice of somidion-based identification; or multiple somidic values used together.

**Part of speech:** 
- Noun (mass): The field/practice
- Noun (plural): Multiple somidic values

**Usage examples (field of study):**
- "Somidics provides equipment-free verification"
- "Research in somidics is still emerging"
- "The potential of somidics for humanitarian use"

**Usage examples (plural):**
- "Enter your somidics: @147293:582047-cf" (v0.4: includes anti-somidics)
- "How many somidics does this credential require?"
- "Store the somidics in an array"

## Notation Conventions

### Single Positive Somidic

**Format:** 6-digit decimal number, zero-padded

**Examples:**
- `147293`
- `000016` (smallest valid human somidic)
- `262143` (largest in Plane 0)
- `999999` (special: always match)

**In text:**
- "somidic 147293"
- "Enter somidic: 147293"
- "Your somidic is 147293"

**Regex pattern:** `\b\d{6}\b`

### Single Anti-Somidic (NEW in v0.4)

**Format:** 2 hex digits (lowercase)

**Examples:**
- `41` - No tattoos on hands+fingers+wrists
- `42` - No tattoos on face
- `cf` - No tattoos or piercings anywhere
- `ff` - No marks of any kind anywhere

**In text:**
- "anti-somidic 41"
- "The anti-somidic cf excludes tattoos and piercings"

**Regex pattern:** `[0-9a-f]{2}` (excluding x0 and 0x patterns)

### Positive Somidic with Anti-Somidics (NEW in v0.4)

**Format:** 6-digit decimal, hyphen, 2+ hex digits

**Examples:**
- `147293-41` (single positive, single anti)
- `147293-cf` (single positive, single anti)
- `147293-4143` (single positive, two antis - compacts to -43)

**In text:**
- "somidic 147293-41"
- "Enter: 147293-cf"

**Regex pattern:** `\d{6}-([0-9a-f]{2})+`

### Multiple Positive Somidics (Sets)

**Format:** @ prefix, colon-separated, canonical sorted order

**Structure:** `@somidic1:somidic2:somidic3`

**Canonical ordering rule:** Always sort numerically ascending

**Examples:**
- `@147293:582047` (two somidics)
- `@147293:582047:923841` (three somidics)

**Invalid (not canonical):**
- `@582047:147293` (wrong order)
- `147293:582047` (missing @ prefix)
- `@147293.582047` (wrong separator)

**In text:**
- "somidics @147293:582047"
- "Enter your somidics: @147293:582047:923841"

**Regex pattern:** `@\d{6}(:\d{6})+`

### Multiple Positives with Anti-Somidics (NEW in v0.4)

**Format:** @ prefix, colon-separated positives, hyphen, anti-somidics

**Examples:**
- `@147293:582047-cf` (two positives, one anti)
- `@147293:582047:923841-4f8f` (three positives, two antis - compacts to -cf)

**Canonical ordering:**
- Positives: sorted numerically ascending
- Anti-somidics: maximally compacted, then sorted numerically

**Regex pattern:** `@\d{6}(:\d{6})+-([0-9a-f]{2})+`

### Pure Anti-Somidics (NEW in v0.4)

**Format:** Hyphen, 2+ hex digits

**Examples:**
- `-cf` (no tattoos or piercings anywhere)
- `-ff` (no marks of any kind anywhere)
- `-4f` (no tattoos anywhere)

**Use case:** Professional credentials (surgeon, lawyer, etc.)

**Regex pattern:** `-([0-9a-f]{2})+`

### Complete Somidics Set Grammar (v0.4)

```
somidic_set := positive_part [anti_part]
             | anti_part

positive_part := '@'? decimal6 (':' decimal6)*
anti_part := '-' hex_byte+

decimal6 := [0-9]{6}
hex_byte := [0-9a-f]{2}
```

**Complete regex:** `^(@?\d{6}(:\d{6})*)?(-(([0-9a-f]{2})+))?$`

## Terminology Hierarchy

### v0.3 Hierarchy (Positive Only)
```
Somidion (physical mark)
    ↓ encode as
Somid (13-bit value)
    ↓ add CRC-5
18-bit combined value
    ↓ add plane offset, convert to decimal
Somidic (6-digit number)
    ↓ multiple somidics
Somidics set (@ notation)
```

### v0.4 Hierarchy (With Anti-Somidics)
```
Somidion (physical mark)          Anti-somidion (absence claim)
    ↓ encode                          ↓ encode
Somid (13-bit)                    Anti-somid (8-bit)
    ↓ add CRC-5                       ↓ (no CRC)
18-bit combined                   8-bit value
    ↓ decimal                         ↓ hex
Somidic (6 digits)                Anti-somidic (2 hex digits)
    ↓                                 ↓
    └─────────────┬──────────────────┘
                  ↓
         Complete somidics set
         (positives-antis notation)
```

## Terms by Context

### User-Facing Language
**Use these terms:**
- "somidion" - the mark on your body
- "anti-somidion" - what marks you DON'T have
- "somidic" - the 6-digit number
- "anti-somidic" - the 2-hex-digit exclusion
- "Enter your somidic"
- "Show your somidion"

**Avoid these terms:**
- "somid" / "anti-somid" (too technical)
- "descriptor" (too technical)
- "CRC" (implementation detail)
- "bits" / "flags" (confusing)

### Technical/Developer Documentation
**Use these terms:**
- "somid" - the 13-bit value
- "anti-somid" - the 8-bit value
- "CRC-5" - the checksum algorithm
- "plane" - the 262k-value namespace
- "canonical form" - for sorted sets
- "flag encoding" - for anti-somid zones/types
- "maximal compaction" - for anti-somidics

### Academic Writing
**Use these terms:**
- "somidion" / "anti-somidion" - the physical features/absences
- "somidic" (noun/adj) - paralleling biometric
- "somidics" - the field of study
- "somidion-based authentication"
- "equipment-free biometric primitive"
- "negative assertions" - for anti-somidics
- "verifier discretion" - for optional checking

## Pronunciation Guide

| Term | IPA | Simple |
|------|-----|--------|
| somidion | /soʊˈmɪdiən/ | so-MID-ee-ən |
| anti-somidion | /ˌæntisoʊˈmɪdiən/ | AN-tee-so-MID-ee-ən |
| somid | /ˈsoʊmɪd/ | SO-mid |
| anti-somid | /ˈæntisoʊmɪd/ | AN-tee-SO-mid |
| somidic | /soʊˈmɪdɪk/ | so-MID-ik |
| anti-somidic | /ˌæntisoʊˈmɪdɪk/ | AN-tee-so-MID-ik |
| somidics | /soʊˈmɪdɪks/ | so-MID-iks |

**Stress pattern:** Emphasis on middle syllable for somidion/somidic/somidics, first syllable for somid

## Common Phrasings

### Questions
- "What is your somidic?"
- "Do you have a somidion you'd be willing to share?"
- "What anti-somidics does this credential include?" (v0.4)
- "Can you show me your somidion?"

### Instructions
- "Enter your somidic: 147293-41" (v0.4)
- "Choose a somidion on your hands or face"
- "This credential contains somidics @147293:582047-cf" (v0.4)
- "Scan the QR code to see the required somidics"

### Descriptions
- "Small raised mole on left forearm"
- "Tattoo on right hand back, coin-sized"
- "No tattoos on hands, fingers, or wrists" (v0.4 anti-somidic)
- "No tattoos or piercings anywhere in public zones" (v0.4 anti-somidic)

### Technical
- "Compute CRC-5 over the somid"
- "The somidic passes validation"
- "Encode the anti-somidion as flag bits" (v0.4)
- "Maximally compact the anti-somidics" (v0.4)

## Anti-Somidic Notation Details (v0.4)

### Common Anti-Somidics

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
8f = No piercings anywhere

c1 = No tattoos or piercings on hands+wrists
cf = No tattoos or piercings anywhere

ff = No marks of any kind anywhere
```

### Decoding Anti-Somidics

**Read hex digits as binary flags:**

```
0x43 (binary: 01000011)
  First hex digit (4 = 0100):
    Bit 6 set = Tattoos
  Second hex digit (3 = 0011):
    Bit 0 set = Hands+wrists
    Bit 1 set = Face
  
Meaning: "No tattoos on hands+wrists or face"
```

### Canonical Form Rules (v0.4)

**Positive somidics:**
1. Sort numerically ascending
2. Remove duplicates
3. Prefix with @ if multiple

**Anti-somidics:**
1. Maximally compact (combine zones and types)
2. Sort numerically ascending (as 8-bit values)
3. Remove duplicates (automatic after compaction)

**Complete set:**
- Positives (if any), hyphen, antis (if any)
- Or just antis (pure anti-somidic credential)

## Translation Guidance

### Terms That Should NOT Be Translated
- **Somidion** - Keep as-is (like "biometric")
- **Anti-somidion** - Keep as-is
- **Somid** / **Anti-somid** - Keep as-is (technical terms)
- **Somidic** / **Anti-somidic** - Keep as-is
- **Somidics** - Keep as-is

**Rationale:** These are technical terms with specific meanings. Translating them creates confusion and inconsistency across languages.

### Terms That SHOULD Be Translated
- "mark on the body" → translate naturally
- "absence of marks" → translate naturally (v0.4)
- "6-digit number" / "2 hex digits" → translate naturally
- "verification" → translate naturally
- Body part names (hand, finger, face) → translate naturally

### Recommended Translation Pattern
```
English: "Enter your somidic: a 6-digit number representing a mark, 
          plus optional anti-somidics (2 hex digits each) for what you DON'T have"
Spanish: "Ingrese su somidic: un número de 6 dígitos que representa una marca,
          más anti-somidics opcionales (2 dígitos hexadecimales cada uno) 
          para lo que NO tiene"
```

**Key principle:** Keep somidic/anti-somidic as loanwords, translate descriptive text.

## Version History

**v0.4** - January 2026
- Added anti-somidion, anti-somid, anti-somidic terminology
- Hyphen notation for negative assertions
- Maximal compaction principle
- Updated all examples to include anti-somidics where relevant

**v0.3** - January 2026
- Updated somid to 13 bits (from 10)
- Context-dependent encoding terminology
- No terminology changes, just updated bit structures

**v0.2** - January 2026
- Updated somid to 13 bits
- Updated zone count to 48
- Added multiplicity terminology

**v0.1** - January 2026
- Initial terminology established
