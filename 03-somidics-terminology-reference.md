# Somidics Core Specification v0.6

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

### Anti-Somidion
**Definition:** A claimed absence of marks of specific type(s) in specific zone(s) on a person's body.

**Etymology:** From Greek *anti* (against, opposite) + *soma* (body) + *idio* (personal, unique)

**Pronunciation:** /ˌæntisoʊˈmɪdiən/ (AN-tee-so-MID-ee-ən)

**Part of speech:** Noun, countable

**Plural:** contrasomidions

**Usage examples:**
- "Her contrasomidion is 'no tattoos on hands or face'"
- "Contrasomidions provide additional discrimination"
- "The somidic includes contrasomidions for stronger identification"

**What qualifies as an contrasomidion:**
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

**Bit structure:**
```
Bits 0-5: Zone (6 bits, 48 defined)
Bits 6-7: Type (2 bits)
Bits 8-9: Size (2 bits)
Bits 10-11: Texture (2 bits, context-dependent)
Bit 12: Multiplicity/Special (1 bit, context-dependent)
```

### Anti-Somid
**Definition:** The 8-bit encoded representation of an contrasomidion's characteristics (zone flags + type flags).

**Etymology:** From *anti* + *somid*

**Pronunciation:** /ˈæntisoʊmɪd/ (AN-tee-SO-mid)

**Part of speech:** Noun, countable

**Plural:** contrasomids

**Value range:** 0x11-0xFF (excluding bytes with nibble = 0)

**Usage examples:**
- "The contrasomid uses flag encoding for zones and types"
- "Contrasomids are represented as 2 hex digits"
- "This contrasomid combines multiple zones and types"

**Technical usage only:**
- Appears in specifications
- Users see hex representation, not the term "contrasomid"

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


### Quant
**Definition:** The 6-digit decimal number representing a somid with CRC-5, before notation is applied.

**Etymology:** From "quantum" - a discrete unit

**Pronunciation:** /kwɒnt/ (kwont)

**Part of speech:** Noun, countable

**Plural:** quants

**Value range:** 000000-262143 (18-bit value as 6-digit decimal, zero-padded)

**Usage examples:**
- "The quant 147293 encodes a mole on the left wrist"
- "Convert the somid to a quant by adding CRC-5"
- "The quant becomes a somidic when properly notated: @+147293"

**Relationship to somidic:**
- Quant = just the number (147293)
- Somidic = properly notated form (@+147293)
- A somidic may contain multiple quants and/or contraquants

**Technical usage:**
- Appears in specifications and implementations
- Users see the somidic, not the quant
- The quant is the intermediate form between somid and somidic

**Structure:**
- 18-bit value: (somid << 5) | CRC-5
- Plus plane offset (0 for humans)
- Converted to decimal, zero-padded to 6 digits

### Somidic
**Definition (noun):** The complete properly-notated expression representing one or more somidions and/or contrasomidions, always beginning with `@` and using `+` and `-` operators.

**Definition (adjective):** Relating to somidion-based identification and authentication.

**Etymology:** From somidion, paralleling "biometric/biometrics"

**Pronunciation:** /soʊˈmɪdɪk/ (so-MID-ik)

**Part of speech:** 
- Noun (countable): "a somidic," "two somidics"
- Adjective: "somidic authentication"

**As noun - Value range:** 000000-999999 (6 decimal digits, zero-padded)

**Usage examples (noun):**
- "Enter your somidic: @+147293"
- "This somidic contains somidics @+147293+582047"
- "Each somidic includes a CRC-5 checksum"
- "The somidic 147293 encodes a mark on the left wrist"

**Usage examples (adjective):**
- "somidic authentication system"
- "somidic verification process"
- "somidic-protected somidic"
- "somidic enrollment procedure"

**Structure:**
- 18-bit value: (somid << 5) | CRC-5
- Plus plane offset (0 for humans)
- Converted to decimal, zero-padded to 6 digits

### Anti-Somidic
**Definition (noun):** The 2-hex-digit representation of an contrasomidion, encoding zone and type flags.

**Definition (adjective):** Relating to negative assertions about body marks.

**Etymology:** From *anti* + *somidic*

**Pronunciation:** /ˌæntisoʊˈmɪdɪk/ (AN-tee-so-MID-ik)

**Part of speech:**
- Noun (countable): "an contraquant," "two contraquants"
- Adjective: "contraquant assertion"

**As noun - Value range:** 0x11-0xFF (2 hex digits, both nibbles non-zero)

**Usage examples (noun):**
- "The contraquant 41 means 'no tattoos on hands+wrists'"
- "This somidic includes contraquants @+147293-41"
- "Contraquants provide optional discrimination enhancement"

**Usage examples (adjective):**
- "contraquant verification requires scanning zones"
- "contraquant protection enhances security"

**Structure:**
- 8-bit value: zone_flags | (type_flags << 4)
- Represented as 2 hex digits (lowercase)
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
- "Enter your somidics: @+147293+582047-cf"
- "How many somidics does this somidic require?"
- "Store the somidics in an array"

## Notation Conventions (v0.6 UPDATE)

### Complete Somidic Format

**All somidics start with `@`:**
- This prefix identifies the string as a somidic somidic
- Required for all somidics (single, multiple, or pure anti)

**Positive somidics:**
- Each preceded by `+`
- Format: 6 decimal digits (leading zeros required)

**Contraquants:**
- Each preceded by `-`
- Format: 2 hex digits (lowercase, both nibbles non-zero)

### Single Positive Somidic

**Format:** `@+` followed by 6-digit decimal

**Examples:**
- `@+147293`
- `@+000016` (smallest valid human somidic)
- `@+262143` (largest in Plane 0)

**In text:**
- "somidic @+147293"
- "Enter somidic: @+147293"
- "Your somidic is @+147293"

**Regex pattern:** `@\+\d{6}`

### Single Anti-Somidic

**Format:** `@-` followed by 2 hex digits (lowercase)

**Examples:**
- `@-41` - No tattoos on hands+fingers+wrists
- `@-42` - No tattoos on face
- `@-cf` - No tattoos or piercings anywhere
- `@-ff` - No marks of any kind anywhere

**In text:**
- "contraquant @-41"
- "somidic @-cf excludes tattoos and piercings"

**Regex pattern:** `@-[0-9a-f]{2}`

### Positive Somidic with Anti-Somidics

**Format:** `@+` positives, then `-` antis

**Examples:**
- `@+147293-41` (single positive, single anti)
- `@+147293-cf` (single positive, single anti)
- `@+147293-41-cf` (single positive, two antis)

**In text:**
- "somidic @+147293-41"
- "Enter: @+147293-cf"

**Regex pattern:** `@\+\d{6}(-[0-9a-f]{2})+`

### Multiple Positive Somidics

**Format:** `@+` followed by positives, each preceded by `+`

**Examples:**
- `@+147293+582047` (two somidics)
- `@+147293+582047+923841` (three somidics)

**Canonical ordering:** Always numerically ascending

**Invalid (not canonical):**
- `@+582047+147293` (wrong order)
- `147293+582047` (missing @ prefix)

**In text:**
- "somidics @+147293+582047"
- "Enter your somidics: @+147293+582047+923841"

**Regex pattern:** `@\+\d{6}(\+\d{6})+`

### Multiple Positives with Anti-Somidics

**Format:** `@+` positives, then `-` antis

**Examples:**
- `@+147293+582047-cf` (two positives, one anti)
- `@+147293+582047-41-cf` (two positives, two antis)

**Canonical ordering:**
- Positives: sorted numerically ascending
- Contraquants: maximally compacted, then sorted numerically

**Regex pattern:** `@\+\d{6}(\+\d{6})+(-[0-9a-f]{2})+`

### Pure Anti-Somidics

**Format:** `@-` followed by antis, each preceded by `-`

**Examples:**
- `@-cf` (no tattoos or piercings anywhere)
- `@-ff` (no marks of any kind anywhere)
- `@-41-cf` (multiple antis - before compaction)

**Use case:** Professional somidics (surgeon, lawyer, etc.)

**Regex pattern:** `@(-[0-9a-f]{2})+`

### Complete Somidic Grammar (v0.6)

```
somidic := '@' component+

component := '+' positive
           | '-' anti

positive := \d{6}      # Always 6 digits, leading zeros required
anti := [0-9a-f]{2}    # Always 2 hex digits, lowercase, both non-zero
```

**Complete regex:** `^@(\+\d{6}|-[0-9a-f]{2})+$`

## Reading Aloud Conventions (v0.6 NEW SECTION)

### Basic Protocol

**Symbol pronunciation:**
- `@` → (silent - implied by context)
- `+` → "PLUS"
- `-` → "MINUS"

**Digit pronunciation:**
- Group in threes: "one four seven, two nine three"
- Always "zero" (never "oh" or "o")
- Hex letters: "a", "b", "c", "d", "e", "f"

### Examples

```
@+147293
→ "PLUS one four seven, two nine three"

@+147293+582047
→ "PLUS one four seven, two nine three, PLUS five eight two, zero four seven"

@+147293-41
→ "PLUS one four seven, two nine three, MINUS four one"

@-cf
→ "MINUS c f"

@+147293-41-cf
→ "PLUS one four seven, two nine three, MINUS four one, MINUS c f"
```

## Terminology Hierarchy

### v0.6 Hierarchy (With Updated Notation)

```
Somidion (physical mark)          Contrasomidion (absence claim)
    ↓ encode                          ↓ encode
Somid (13-bit)                    Contrasomid (8-bit)
    ↓ add CRC-5                       ↓ (no CRC)
18-bit combined                   8-bit value
    ↓ decimal                         ↓ hex
Somidic (6 digits)                Contraquant (2 hex digits)
    ↓                                 ↓
    └─────────────┬───────────────────┘
                  ↓
         Complete somidic
         (@+ notation with - for antis)
```

## Terms by Context

### User-Facing Language
**Use these terms:**
- "somidion" - the mark on your body
- "contrasomidion" - what marks you DON'T have
- "somidic" - the number in the somidic
- "contraquant" - the exclusion in the somidic
- "Enter your somidic: @+147293"
- "Show your somidion"

**Avoid these terms:**
- "somid" / "contrasomid" (too technical)
- "descriptor" (too technical)
- "CRC" (implementation detail)
- "bits" / "flags" (confusing)

### Technical/Developer Documentation
**Use these terms:**
- "somid" - the 13-bit value
- "contrasomid" - the 8-bit value
- "CRC-5" - the checksum algorithm
- "plane" - the 262k-value namespace
- "canonical form" - for sorted somidics
- "flag encoding" - for contrasomid zones/types
- "maximal compaction" - for contraquants

### Academic Writing
**Use these terms:**
- "somidion" / "contrasomidion" - the physical features/absences
- "somidic" (noun/adj) - paralleling biometric
- "somidics" - the field of study
- "somidion-based authentication"
- "equipment-free biometric primitive"
- "negative assertions" - for contraquants
- "verifier discretion" - for optional checking

## Pronunciation Guide

| Term | IPA | Simple |
|------|-----|--------|
| somidion | /soʊˈmɪdiən/ | so-MID-ee-ən |
| contrasomidion | /ˌæntisoʊˈmɪdiən/ | AN-tee-so-MID-ee-ən |
| somid | /ˈsoʊmɪd/ | SO-mid |
| contrasomid | /ˈæntisoʊmɪd/ | AN-tee-SO-mid |
| somidic | /soʊˈmɪdɪk/ | so-MID-ik |
| contraquant | /ˌæntisoʊˈmɪdɪk/ | AN-tee-so-MID-ik |
| somidics | /soʊˈmɪdɪks/ | so-MID-iks |

**Stress pattern:** Emphasis on middle syllable for somidion/somidic/somidics, first syllable for somid

## Common Phrasings

### Questions
- "What is your somidic?"
- "Do you have a somidion you'd be willing to share?"
- "What contraquants does this somidic include?"
- "Can you show me your somidion?"

### Instructions
- "Enter your somidic: @+147293-41"
- "Choose a somidion on your hands or face"
- "This somidic is @+147293+582047-cf"
- "Scan the QR code to see the required somidic"

### Descriptions
- "Small raised mole on left forearm"
- "Tattoo on right hand back, coin-sized"
- "No tattoos on hands, fingers, or wrists"
- "No tattoos or piercings anywhere in public zones"

### Technical
- "Compute CRC-5 over the somid"
- "The somidic passes validation"
- "Encode the contrasomidion as flag bits"
- "Maximally compact the contraquants"

## Anti-Somidic Notation Details

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

### Canonical Form Rules (v0.6)

**Positive somidics:**
1. Sort numerically ascending
2. Remove duplicates
3. Each preceded by `+`

**Contraquants:**
1. Maximally compact (combine zones and types)
2. Sort numerically ascending (as 8-bit values)
3. Remove duplicates (automatic after compaction)
4. Each preceded by `-`

**Complete somidic:**
- Always starts with `@`
- Positives first (if any), then antis (if any)
- All components explicitly marked with operator

## Translation Guidance

### Terms That Should NOT Be Translated
- **Somidion** - Keep as-is (like "biometric")
- **Contrasomidion** - Keep as-is
- **Somid** / **Contrasomid** - Keep as-is (technical terms)
- **Somidic** / **Contraquant** - Keep as-is
- **Somidics** - Keep as-is

**Rationale:** These are technical terms with specific meanings. Translating them creates confusion and inconsistency across languages.

### Terms That SHOULD Be Translated
- "mark on the body" → translate naturally
- "absence of marks" → translate naturally
- "6-digit number" / "2 hex digits" → translate naturally
- "verification" → translate naturally
- Body part names (hand, finger, face) → translate naturally
- "PLUS" / "MINUS" (when reading aloud) → translate naturally

### Recommended Translation Pattern
```
English: "Enter your somidic: a 6-digit number with an @ symbol and plus signs,
          plus optional contraquants (MINUS and 2 hex digits) for what you DON'T have"
Spanish: "Ingrese su credencial: un número de 6 dígitos con símbolo @ y signos más,
          más contraquants opcionales (MENOS y 2 dígitos hexadecimales) 
          para lo que NO tiene"
```

**Key principle:** Keep somidic/contraquant as loanwords, translate descriptive text.

## Semantic Interpretation (v0.6 NEW)

### Set Operations Reading

**The notation represents mathematical set operations:**

```
@+P₁+P₂-A₁-A₂

Reads as:
  "Union of P₁ and P₂, minus union of A₁ and A₂"
  
Mathematically:
  (P₁ ∪ P₂) \ (A₁ ∪ A₂)

Where:
  + = union (∪)
  - = set difference (\)
```

**Examples:**
```
@+147293
  = Set of bodies matching somidic 147293

@+147293-41
  = Set of bodies matching 147293, excluding those with hand tattoos
  = P₁₄₇₂₉₃ \ A₄₁

@-41
  = Universal set, excluding those with hand tattoos
  = U \ A₄₁
```

## Version History

**v0.6** - January 2026
- Updated all notation examples to use `@+` prefix
- Separated contraquants (each with `-` prefix)
- Added reading aloud protocol section
- Added semantic interpretation section
- Updated regex patterns for new notation
- All terminology unchanged (just notation format)

**v0.5** - January 2026
- Added contrasomidion, contrasomid, contraquant terminology
- Hyphen notation for negative assertions
- Maximal compaction principle
- Updated all examples to include contraquants where relevant

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
