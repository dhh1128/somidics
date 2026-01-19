# Somidics Terminology & Notation Reference

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

### Somid
**Definition:** The 10-bit encoded representation of a somidion's characteristics (zone + type + size + texture), before CRC is computed.

**Etymology:** Shortened from somidion, technical term

**Pronunciation:** /ˈsoʊmɪd/ (SO-mid)

**Part of speech:** Noun, countable

**Plural:** somids

**Value range:** 0-1023 (2^10)

**Usage examples:**
- "The somid encodes zone, type, size, and texture"
- "Valid somids range from 0 to 1023"
- "Each somid corresponds to a specific combination of attributes"
- "The somid is passed to the CRC-8 function"

**Technical usage only:**
- Appears in specifications, not user-facing text
- Users never see or work with somids directly
- Intermediate representation between somidion and somidic

**Bit structure:**
```
Bits 0-4: Zone (5 bits)
Bits 5-6: Type (2 bits)
Bits 7-8: Size (2 bits)
Bits 9-10: Texture (2 bits)
```

### Somidic
**Definition (noun):** The 6-digit decimal number that represents a somidion, including CRC-8 checksum for validation.

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
- "Each somidic includes a CRC-8 checksum"
- "The somidic 147293 encodes a mark on the left forearm"

**Usage examples (adjective):**
- "somidic authentication system"
- "somidic verification process"
- "somidic-protected credential"
- "somidic enrollment procedure"

**Structure:**
- 18-bit value: (8-bit CRC << 10) | 10-bit somid
- Plus plane offset (0 for humans)
- Converted to decimal, zero-padded to 6 digits

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
- "Enter your somidics: @147293:582047"
- "How many somidics does this credential require?"
- "Store the somidics in an array"

### Somidion Descriptor
**Definition:** Technical term for the 10-bit somid, emphasizing it describes the somidion's attributes. Synonym for somid.

**Usage:** Primarily in formal specifications when clarity is needed.

**Example:** "The somidion descriptor consists of 5 zone bits and 5 attribute bits"

## Notation Conventions

### Single Somidic

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

### Multiple Somidics (Sets)

**Format:** @ prefix, colon-separated, canonical sorted order

**Structure:** `@somidic1:somidic2:somidic3:...`

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
- "This credential requires a three-value somidic set"

**Regex pattern:** `@\d{6}(:\d{6})+`

**Comparison algorithm:**
```
1. Parse both sets
2. Sort each set numerically
3. Compare element by element
4. Sets are equal if all elements match
```

**Deduplication:**
Sets automatically deduplicate: `@147293:147293:582047` → `@147293:582047`

## Terminology Hierarchy

```
Somidion (physical mark)
    ↓ encode as
Somid (10-bit value)
    ↓ add CRC-8
18-bit combined value
    ↓ add plane offset, convert to decimal
Somidic (6-digit number)
    ↓ multiple somidics
Somidics set (@ notation)
```

## Terms by Context

### User-Facing Language
**Use these terms:**
- "somidion" - the mark on your body
- "somidic" - the 6-digit number
- "somidics" - when referring to multiple
- "Enter your somidic"
- "Show your somidion"

**Avoid these terms:**
- "somid" (too technical)
- "descriptor" (too technical)
- "CRC" (implementation detail)
- "bits" (confusing)

### Technical/Developer Documentation
**Use these terms:**
- "somid" - the 10-bit value
- "somidion descriptor" - when being explicit
- "CRC-8" - the checksum algorithm
- "plane" - the 262k-value namespace
- "canonical form" - for sorted sets

### Academic Writing
**Use these terms:**
- "somidion" - the physical feature
- "somidic" (noun/adj) - paralleling biometric
- "somidics" - the field of study
- "somidion-based authentication"
- "equipment-free biometric primitive"

## Pronunciation Guide

| Term | IPA | Simple |
|------|-----|--------|
| somidion | /soʊˈmɪdiən/ | so-MID-ee-ən |
| somid | /ˈsoʊmɪd/ | SO-mid |
| somidic | /soʊˈmɪdɪk/ | so-MID-ik |
| somidics | /soʊˈmɪdɪks/ | so-MID-iks |

**Stress pattern:** Emphasis on middle syllable for somidion/somidic/somidics, first syllable for somid

## Common Phrasings

### Questions
- "What is your somidic?"
- "Do you have a somidion you'd be willing to share?"
- "How many somidics does this require?"
- "Can you show me your somidion?"

### Instructions
- "Enter your somidic: 147293"
- "Choose a somidion on your hands or face"
- "This credential contains somidics @147293:582047"
- "Scan the QR code to see the required somidics"

### Descriptions
- "Small raised mole on left forearm"
- "Tattoo on right hand back, coin-sized"
- "Scar on left cheek, fingernail-sized, flush"
- "Missing part on right index finger, small extent"

### Technical
- "Compute CRC-8 over the somid"
- "The somidic passes validation"
- "Encode the somidion as a somid"
- "Plane 0 contains human somidics"

## Related Terms

### Authentication Context
- **PIN** - Personal Identification Number (what you know)
- **Biometric** - Fingerprint, iris scan (what you are, with equipment)
- **Somidic** - Body mark identifier (what you are, without equipment)
- **Password** - Secret string (what you know)
- **Token** - Physical device (what you have)

### Body Marking Terms
- **Mole** - Natural pigmented spot
- **Birthmark** - Mark present at birth
- **Scar** - Mark from healed wound
- **Tattoo** - Permanent ink marking
- **Piercing** - Hole through skin
- **Implant** - Object under skin
- **Freckle** - Small pigmented spot
- **Dimple** - Natural depression in skin

### Identification Systems
- **Credential** - Document asserting identity
- **Biometric** - Physiological measurement
- **Identifier** - Unique reference
- **Verification** - Confirming identity claim
- **Authentication** - Proving identity

## Translation Guidance

### Terms That Should NOT Be Translated
- **Somidion** - Keep as-is (like "biometric")
- **Somid** - Keep as-is (technical term)
- **Somidic** - Keep as-is
- **Somidics** - Keep as-is

**Rationale:** These are technical terms with specific meanings. Translating them creates confusion and inconsistency across languages.

### Terms That SHOULD Be Translated
- "mark on the body" → translate naturally
- "6-digit number" → translate naturally  
- "verification" → translate naturally
- "authentication" → translate naturally
- Body part names (hand, finger, face) → translate naturally

### Recommended Translation Pattern
English: "Enter your somidic: a 6-digit number representing a mark on your body"
Spanish: "Ingrese su somidic: un número de 6 dígitos que representa una marca en su cuerpo"
French: "Entrez votre somidic : un nombre à 6 chiffres représentant une marque sur votre corps"

**Key principle:** Keep somidic/somidion as loanwords, translate descriptive text.

## Abbreviations

**Generally avoid abbreviating** - terms are already short

**If abbreviation is necessary:**
- Somidic → SC or S (in tables/diagrams only)
- Somidion → SI (in technical diagrams only)
- Never abbreviate in user-facing text

## Grammar Notes

### Countability
- **Somidion:** Countable - "one somidion, two somidions"
- **Somid:** Countable - "one somid, two somids"
- **Somidic:** Countable - "one somidic, two somidics"
- **Somidics:** Mass or plural depending on context

### Article Usage
- "a somidion" (countable, indefinite)
- "the somidion on your wrist" (countable, definite)
- "a somidic" (countable, indefinite)
- "somidics as an authentication method" (mass, no article)

### Verb Agreement
- "The somidic is valid" (singular)
- "Somidics are useful" (plural or mass)
- "Your somidion is on your left hand" (singular)
- "Enter your somidics" (plural)

### Possessive Forms
- "my somidion" / "your somidion"
- "my somidic" / "your somidic"
- "the person's somidions" (plural possessive)
- "the credential's somidics" (plural possessive)

## Examples in Context

### Enrollment Dialog
```
System: "Welcome to somidic enrollment."
System: "A somidion is a unique mark on your body, like a mole or scar."
System: "Think of a somidion you'd be comfortable showing someone."
User: "I have a mole on my left wrist."
System: "Great! Let's encode that as your somidic."
[Interview process]
System: "Your somidic is 147293. You don't need to memorize this number."
```

### Verification Dialog
```
System: "This credential requires somidic verification."
System: "Please show: Small raised mark on left forearm"
Verifier: [Looks at person's left forearm]
Verifier: "I see a small raised mole there."
Verifier: [Accepts verification]
```

### Technical Documentation
```
The somid (10-bit descriptor) is computed by encoding the 
somidion's zone (5 bits), type (2 bits), size (2 bits), 
and texture (2 bits). A CRC-8 checksum is then computed 
over the somid and prepended, creating an 18-bit value. 
This value is converted to decimal and formatted as a 
6-digit somidic.
```

### Academic Paper
```
We propose somidics, a novel authentication primitive based 
on naturally occurring body marks called somidions. Unlike 
traditional biometrics, somidic authentication requires no 
specialized equipment and can be performed by untrained 
verifiers using only visual inspection.
```

## Version History

**v0.1** - Initial terminology (January 2026)
- Established somidion/somid/somidic/somidics
- Defined @ notation for sets
- Canonical ordering rules
