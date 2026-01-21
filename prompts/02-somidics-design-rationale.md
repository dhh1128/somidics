# Somidics Core Specification v0.6

## Core Design Philosophy

Somidics is designed to provide **"good enough" identification** in contexts where:
- Specialized equipment is unavailable
- Strong biometrics would be overkill or inappropriate
- Human judgment and interaction are acceptable
- Privacy concerns favor fuzzy matching over precision

**Key principle:** Optimize for practical utility in low-resource contexts, not theoretical perfection.

## Why Equipment-Free?

### Problem Context
Traditional biometrics require:
- Fingerprint scanners ($100-$10,000)
- Iris scanners ($1,000-$10,000)
- Facial recognition cameras + compute
- Reliable power and connectivity
- Trained operators

### Target Scenarios Where This Fails
1. **Refugee camps** - Limited infrastructure, high turnover
2. **Rural healthcare** - No biometric readers available
3. **Disaster response** - Equipment lost/damaged
4. **Developing world** - Cost prohibitive
5. **Child identification at birth** - No hospital equipment available
6. **Trafficking victim recovery** - Need immediate, equipment-free ID

### Somidics Solution
- Zero equipment cost
- Works anywhere, anytime
- No training required
- No power/connectivity needed
- Natural human capability (recognizing marks)

## Why Human-Verifiable (Not ML-Based)?

### Decision: Keep Humans in the Loop

**Rejected approach:** Computer vision + ML to match somidions
- Would require cameras (breaks equipment-free requirement)
- Would require training data (privacy issues)
- Would require compute (cost, power)
- Would introduce ML bias issues
- Would create false precision

**Chosen approach:** Human verifier makes judgment call
- Natural human skill (we already do this)
- No equipment needed
- Handles edge cases gracefully
- Built-in forgiveness for ambiguity
- Culturally adaptable

### Inter-Rater Reliability

**We accept that two humans might disagree:**
- Is a mark "grain-sized" or "fingernail-sized"? Ã¢â€ â€™ Fuzzy boundary
- Is it "raised" or "flush"? Ã¢â€ â€™ Sometimes subtle
- Is it on "left cheek" or "nose"? Ã¢â€ â€™ If near boundary

**This is a feature, not a bug:**
- Prevents somidics from being used as strong legal proof
- Preserves privacy (can't definitively exclude someone)
- Matches the use case (casual fraud prevention, not courtroom evidence)

## Zone Selection Rationale

### Evolution of Zone Design

**v0.1:** 32 zones (coarse granularity)
**v0.2:** 48 zones (finer granularity through subdivision)
**v0.3:** Same 48 zones (focus shifted to attribute enhancement)

### Why These Zones?

**Included zones must be:**
1. **Culturally acceptable to show** - Most cultures accept showing hands/face
2. **Stable over time** - Excludes hair, clothing choices
3. **Likely to have distinguishing marks** - People actually have marks there
4. **Easy to describe in common language** - Natural body part names

### Finger Subdivision (v0.2 Innovation)

**Why subdivide into ring/upper portions:**
- Adds precision without complexity (natural language: "tip of finger" vs "base of finger")
- Each portion distinct: Ring portion = knuckle area, Upper portion = toward fingertip
- Missing finger convention: If whole finger missing, use ring portion zone
- Entropy gain: logÃ¢â€šâ€š(20) vs logÃ¢â€šâ€š(10) = 4.32 vs 3.32 bits (+1 bit)

**Natural language examples:**
- "Scar on ring portion of left index finger"
- "Mole on upper portion of right thumb"

### Hand Subdivision (v0.2 Innovation)

**Why subdivide into thumb-side/pinky-side:**
- Hands are large zones with many marks
- Natural reference points (thumb side vs pinky side immediately clear)
- Entropy gain: logÃ¢â€šâ€š(8) vs logÃ¢â€šâ€š(4) = 3 vs 2 bits (+1 bit)

**Natural language examples:**
- "Birthmark on thumb side of left palm"
- "Scar on pinky side of right hand back"

### Face Expansion (v0.2 Innovation)

**Why add lips/mouth zone:**
- Common location for marks (moles near mouth, lip scars)
- Distinct enough to be its own zone
- Natural language: "mole near mouth"

**Why split jaw into left/right:**
- Jaw is large area, can have marks on either side
- Natural language: "scar on left jaw" vs "right jaw"

### What We Excluded and Why

**Upper legs/thighs:**
- Ã¢Å“â€” Modesty concerns (requires removing pants)
- Ã¢Å“â€” Practical concerns (less accessible)
- Ã¢Å“â€” Cultural unacceptability in many contexts

**Feet/toes:**
- Ã¢Å“â€” Requires shoe removal (impractical)
- Ã¢Å“â€” Hygiene concerns
- Ã¢Å“â€” Less common to have distinctive marks

**Torso (chest, back, abdomen):**
- Ã¢Å“â€” Requires clothing removal
- Ã¢Å“â€” Strong modesty concerns
- Ã¢Å“â€œ Moved to reserved zones (48-63) for forensic use only

**Scalp/hair:**
- Ã¢Å“â€” Hair coverage varies
- Ã¢Å“â€” Marks not consistently visible
- Ã¢Å“â€” Cultural variations in hair covering

**Teeth:**
- Ã¢Å“â€” Too unstable (dental work changes)
- Ã¢Å“â€” Requires opening mouth (intimate)
- Ã¢Å“â€” Not reliably visible

### Reserved Zones (48-63)

**Design choice:**
- 16 zones reserved rather than defined
- Allows future private zone map without breaking compatibility
- Medical examiner/forensic use cases
- Better to reserve now than regret later

## Attribute Selection Rationale

### Why These Attributes?

**Type, Size, Texture, Multiplicity/Special** were chosen because they are:
1. **Observable without equipment** - Just eyes and possibly touch
2. **Stable over time** - Don't change much across decades
3. **Natural human distinctions** - We already think this way
4. **Language-universal** - Every culture has words for these concepts

### Type: Why Natural/Scar/Missing-Anomalous/Artificial?

**Natural marks:**
- Most common (moles, birthmarks, freckles)
- Stable
- No stigma

**Scars:**
- Very common
- Permanent enough
- Distinctive

**Missing/Anomalous (v0.2 change):**
- Grouped together as "anatomical variations"
- Both represent deviation from normal anatomy
- Rationale: Different from deliberate modification
- v0.3 enhancement: Texture bits repurposed to distinguish subtypes

**Artificial/intentional:**
- Tattoos increasingly common worldwide
- Piercings universal across cultures
- Implants/body modification growing
- Intentional Ã¢â€ â€™ person chose to be identifiable

### Size: Why Four Categories?

**Design pressure:** Need human-comparable references

**Rejected approaches:**
- Ã¢Å“â€” Millimeter measurements - Requires ruler, too precise
- Ã¢Å“â€” "Small/medium/large" - Too vague, only 3 categories
- Ã¢Å“â€” Comparison to body parts (thumb joint) - Requires looking at person's thumb

**Chosen approach:** Universal reference objects
- Grain (rice/wheat) - Available globally
- Fingernail (pinky) - Everyone has one, good reference
- Coin - Nearly universal, familiar size
- Larger than coin - Obvious category

**Why "longest dimension":**
- Linear scars measured by length naturally
- Round marks measured by diameter naturally
- Avoids confusion about area vs. length
- Clear rule removes ambiguity

**Entropy:** 2 bits (4 categories) is good balance
- Enough discrimination
- Not so many that boundaries are unclear

### Texture: Evolution and Context-Dependency

#### v0.1/v0.2: Simple Texture Encoding

**00: Flush/flat** - Most common state
**01: Raised or depressed** - Natural 3D features
**10: Tattooed/inked** - Surface marking
**11: Pierced/implanted** - Physical penetration

This worked well for natural marks and scars, but left opportunities on the table.

#### v0.3: Context-Dependent Texture

**Key insight:** The meaning of texture bits can depend on Type without ambiguity during decoding.

**For Type=10 (Missing/Anomalous), texture bits repurposed:**
- **00: Missing** - Absent tissue
- **01: Extra** - Additional tissue (webbing, extra digit)
- **10: Fused** - Connected parts (syndactyly)
- **11: Deformed** - Misshapen (bent finger, cauliflower ear)

**Rationale:**
- Missing/anomalous features don't have "texture" in the normal sense
- These bits were essentially wasted (always set to 00 by convention in v0.2)
- Repurposing gives 4Ãƒâ€” discrimination for these rare but important cases
- Natural language remains clear: "Extra tissue on left hand" vs "Missing on right finger"
- Stability: These subtypes don't change over time

**Entropy gain:** ~0.06 bits average (rare but highly discriminating when present)

### Multiplicity/Special Bit: Context-Dependent Design (v0.3 Innovation)

**Key innovation:** Bit 12's meaning depends on Type and Texture context.

#### For Natural Marks and Scars:
**Multiplicity encoding (v0.2 addition, maintained in v0.3):**
- Single vs cluster distinction
- Common use case: "cluster of freckles" vs "single mole"
- Entropy: ~0.88 bits (70/30 split estimated)

#### For Tattoos (Type=11, Texture=10):
**Writing indicator (v0.3 innovation):**
- 0 = No writing (pure imagery)
- 1 = Contains writing (text, letters, words)

**Rationale:**
- Tattoos with text are common and distinctive
- "Tattoo with writing" is stable over time (unlike color)
- Natural language clear: "tattoo with writing on forearm"
- Multiplicity doesn't make sense for tattoos (each tattoo is a separate zone typically)
- Entropy gain: ~0.12 bits average (40/60 split estimated)

**Why not color?**
- Color fades significantly over 20+ years
- Violates stability principle
- Lighting-dependent
- Same reason we rejected color for natural marks

**Why writing instead of imagery type?**
- Binary distinction (has writing or doesn't)
- Stable (writing doesn't become non-writing)
- Easy to verify visually
- Culturally universal

#### For Piercings (Type=11, Texture=11):
**Multiplicity encoding (v0.3):**
- Single vs multiple piercings
- Common use case: "Three piercings in left ear"
- Makes semantic sense (people cluster piercings)

#### For Anomalous Features (Type=10, TextureÃ¢â€°Â 00):
**Intensifier encoding (v0.3 innovation):**
- 0 = Mild/subtle
- 1 = Severe/prominent

**Rationale:**
- Multiplicity doesn't make sense (each anomaly gets its own zone/somidion)
- Severity is meaningful and stable
- Examples: "Mild webbing" vs "Severe webbing"
- "Slight deformity" vs "Major deformity"
- Natural language clear
- Entropy gain: ~0.06 bits (primarily from anomalous cases)

#### For Missing Parts (Type=10, Texture=00):
**Bit 12 = 0 by convention:**
- Missing is binary (present or absent)
- No meaningful use for this bit
- Simple and clean

**Zone convention handles extent:**
- Ring portion = whole finger can be missing
- Upper portion = just tip missing
- Bit 12 not needed

### What We Deliberately Don't Encode

**Color/Contrast - Explicitly excluded**

**Why not include?**
- Ã¢Å“â€” Fades over time (scars, tattoos)
- Ã¢Å“â€” Changes with sun exposure
- Ã¢Å“â€” Subjective to skin tone
- Ã¢Å“â€” Lighting-dependent
- Ã¢Å“â€” Reduces stability

**This was a hard decision** - color is very identifying! But:
- Stability is more important than discrimination
- A somidic should be valid for decades
- "Dark mole" becomes "faded mole" Ã¢â€ â€™ breaks the encoding

**Origin (birth/accident/intentional) - Excluded**

**Why not?**
- Ã¢Å“â€” Not observable at verification time
- Ã¢Å“â€” Doesn't help verifier make determination
- Ã¢Å“â€” Adds complexity without utility

**Age of mark - Excluded**

**Why not?**
- Ã¢Å“â€” Not determinable by looking
- Ã¢Å“â€” Not helpful for matching
- Ã¢Å“â€” Would be guesswork

## CRC Design Evolution

### v0.1: CRC-8 (8 bits)
- Strong error detection (255/256 = 99.6% rejection)
- 10-bit somid + 8-bit CRC = 18 bits total
- Effective discrimination: ~1-in-500

### v0.2: CRC-5 (5 bits)
- Weaker but sufficient error detection (31/32 = 96.9% rejection)
- 13-bit somid + 5-bit CRC = 18 bits total
- Bits better used for discrimination than error detection
- Effective discrimination: ~1-in-3,870

### Rationale for CRC-5

**Why reduce from 8 to 5 bits?**
- Error detection still strong enough (96.9% rejection rate)
- Gained 3 bits for somid (10Ã¢â€ â€™13 bits)
- 13-bit somid with dependencies Ã¢â€°Ë† 11.9 bits real entropy
- Much better than 10-bit somid with dependencies Ã¢â€°Ë† 8-9 bits
- Trade 2.7% error detection for ~3Ãƒâ€” discrimination improvement

**Is 96.9% rejection enough?**
- Yes - still catches vast majority of typos
- Random 6-digit number: 96.9% chance of immediate rejection
- Prevents casual brute force attempts
- Good enough for use case (not cryptographic)

### v0.3: CRC-5 Maintained
- No change to CRC
- Focus on better using the 13-bit somid space
- Context-dependent encoding squeezes more entropy from existing bits

## Context-Dependent Encoding Philosophy (v0.3)

### The Key Insight

**Traditional approach:** Each bit position has one fixed meaning across all contexts.

**Context-dependent approach:** Bit meanings can vary based on other bits, as long as decoding is unambiguous.

**Example:**
- When Type=10 (Missing/Anomalous), texture bits mean "subtype" not "surface texture"
- Decoder knows to interpret differently because Type=10 is already decoded
- No ambiguity, but more expressive power

### Benefits

1. **Efficient bit usage:** Don't waste bits on meaningless combinations
2. **Semantic clarity:** Encoding matches conceptual model
3. **Extensibility:** Can add context-specific meanings later
4. **Natural language:** Decoded descriptions make sense

### Design Rules

**For context-dependent encoding to work:**
1. **Unambiguous decoding:** Must be able to determine context before interpreting dependent bits
2. **Semantic consistency:** Reinterpretation should make conceptual sense
3. **Natural language:** Decoded output should read naturally
4. **Stability:** Context-dependent meanings still must be stable over time

### Example: Missing vs Anomalous

**v0.2 approach:**
- Type=10 means "Missing or Anomalous"
- Texture always 00 (flush) by convention
- Loss: Can't distinguish missing from webbed from deformed

**v0.3 approach:**
- Type=10 means "Missing or Anomalous"
- Texture bits 00/01/10/11 mean Missing/Extra/Fused/Deformed
- For anomalous (textureÃ¢â€°Â 00), bit 12 means mild/severe
- For missing (texture=00), bit 12 unused (set to 0)
- Gain: 4 subtypes, intensity for 3 of them

## Fuzziness as a Feature

### Design Principle: Embrace Ambiguity

**Intentional fuzziness occurs at:**
1. **Size boundaries** - Is it "grain" or "fingernail"?
2. **Texture boundaries** - Is it "slightly raised" or "flush"?
3. **Zone boundaries** - Is it "left cheek" or "nose" if near middle?
4. **Severity boundaries** - Is webbing "mild" or "severe"?

**Why this is good:**

**Privacy benefit:**
- Can't use somidics as definitive legal proof
- Can challenge in court: "That's not quite my mark"
- Prevents overreach of the system

**Practical benefit:**
- Handles natural human variation in judgment
- No need for precise measurement tools
- Graceful degradation (close enough counts)

**Cultural benefit:**
- Different cultures may judge differently
- System adapts to local interpretation
- No "one true answer"

### Discrimination Target Evolution

**v0.1:** ~1-in-500 (10-bit somid, high correlations)
**v0.2:** ~1-in-3,870 (13-bit somid, multiplicity added)
**v0.3:** ~1-in-3,300 (refined correlation analysis, context-dependent enhancements)

**Why this range?**
- Don't need fingerprint-level precision (1 in billions)
- Just need "good enough to deter casual fraud"
- Use multiple somidics if need stronger ID
- Single somidic: casual fraud prevention
- Two somidics: strong identification
- Three+ somidics: very strong identification

## Stability vs. Precision Tradeoff

### Key Design Choice: Optimize for Stability

**What we sacrificed:**
- Color encoding (would add ~2 bits but fades)
- Fine texture details (smooth vs rough varies)
- Exact size measurements (would require ruler)

**What we gained:**
- Somidics valid for decades
- Resistant to fading, aging
- No re-issuance needed for natural changes

**Example:**
- Tattoo fades from dark to light over 20 years
- Old encoding: "dark tattoo" Ã¢â€ â€™ breaks when it fades
- Our encoding: "tattoo with writing on left forearm, coin-sized" Ã¢â€ â€™ still valid

### When Re-issuance IS Required

**Acceptable reasons:**
- Tattoo completely removed
- Mole surgically removed
- New prominent scar overshadows old somidion
- Missing body part (amputation)

**These are analogous to:**
- Name changes (require somidic re-issue)
- Photo updates in passport
- Address changes in driver's license

## Entropy Recovery Strategy (v0.3)

### The Problem

v0.2 introduced 13-bit somid but:
- Zone underutilization: 48/64 zones used (-0.42 bits)
- Type-Texture correlation: Natural can't be tattooed (-0.39 bits)
- Zone popularity skew: Hands over-represented (-0.30 bits)
- Size distribution: Fingernail-sized most common (-0.08 bits)
- Other correlations: (-0.10 bits)
- **Total loss: ~1.3 bits from theoretical 12.58**

### The Solution

**Don't fight the correlations - exploit them:**

1. **Tattoo writing bit:** Since Type=11+Texture=10 identifies tattoos, repurpose bit 12
   - Gain: ~0.12 bits average

2. **Anomalous intensifier:** Since Type=10+TextureÃ¢â€°Â 00 identifies anomalies, repurpose bit 12
   - Gain: ~0.06 bits average

3. **Missing texture repurposing:** Since Type=10 texture was wasted, create subtypes
   - Gain: ~0.06 bits average (from better discrimination of rare cases)

**Result:** Partial recovery of lost entropy through smarter encoding

## Success Criteria Review

### What We Optimized For

1. Ã¢Å“â€¦ **Equipment-free** - Zero cost, works anywhere
2. Ã¢Å“â€¦ **Human-verifiable** - No training needed
3. Ã¢Å“â€¦ **Memorable** - Remember the mark, not the number
4. Ã¢Å“â€¦ **Stackable** - Multiple somidics for higher security
5. Ã¢Å“â€¦ **Stable** - Decades of validity
6. Ã¢Å“â€¦ **Privacy-preserving** - Fuzzy enough to avoid overreach
7. Ã¢Å“â€¦ **Culturally adaptable** - Respects modesty norms
8. Ã¢Å“â€¦ **Practical** - Works in real-world scenarios

### What We Explicitly Did NOT Optimize For

1. Ã¢Å“â€” **Courtroom-proof identification** - Too strong for our use case
2. Ã¢Å“â€” **High-throughput** - Human verification is inherently slow
3. Ã¢Å“â€” **Video-call verification** - Requires good cameras, lighting
4. Ã¢Å“â€” **Perfect precision** - Would sacrifice stability
5. Ã¢Å“â€” **Uniqueness guarantee** - 1-in-3,300 is good enough
6. Ã¢Å“â€” **Universal coverage** - Some people may lack suitable marks

## Version Evolution Summary

### v0.1 Ã¢â€ â€™ v0.2: Discrimination Enhancement
- 10-bit Ã¢â€ â€™ 13-bit somid
- 32 Ã¢â€ â€™ 48 zones (finger/hand/face subdivision)
- CRC-8 Ã¢â€ â€™ CRC-5 (trade error detection for discrimination)
- Added multiplicity attribute
- Result: ~7.7Ãƒâ€” better discrimination

### v0.2 Ã¢â€ â€™ v0.3: Semantic Enhancement
- Same bit structure (13-bit somid, 5-bit CRC)
- Context-dependent bit 12 (tattoo writing, anomalous intensity)
- Missing/Anomalous texture repurposing (4 subtypes)
- Refined entropy analysis (more accurate correlation modeling)
- Result: Better real-world discrimination through smarter encoding

### Design Philosophy Evolution

**v0.1:** Prove the concept
**v0.2:** Maximize theoretical entropy
**v0.3:** Optimize practical entropy given real-world constraints
**v0.5:** Add optional negative assertions for discrimination enhancement

The progression shows increasing sophistication while maintaining core simplicity and stability principles.

## Anti-Somidics Design Rationale (v0.5 Addition)

### The Core Insight

**Positive somidics alone have limited discrimination** (~1-in-3,300 for a single somidic).

**Adding negative assertions dramatically improves discrimination:**
- "I have a mole on my wrist" = 1-in-3,300
- "I have a mole on my wrist AND no tattoos anywhere" = 1-in-15,000+

The key innovation: **Use absence of marks as identifying information**.

### Why Anti-Somidics?

#### Problem: Positive Somidics Can Be Easy to Match

**Scenario:** Credit card protected by somidic "tattoo on right forearm"

**Issue:** In a tattooed population, 30-40% of people might have a tattoo on their forearm.

**Discrimination is weak:** The thief has a decent chance of finding someone who matches.

#### Solution: Add Negative Constraint

**Enhanced somidic:** "Tattoo on right forearm + NO other tattoos on hands or face"

**Now the thief needs:**
1. Someone with tattoo on forearm (30-40% of population)
2. Who has NO tattoos on hands (70% of tattooed people)
3. Who has NO tattoos on face (90% of tattooed people)

**Combined probability:** 30% Ã— 70% Ã— 90% â‰ˆ 19% â†’ **Much harder!**

### Why Flag Encoding Instead of Enumeration?

#### Rejected Approach: Enumeration

**Could have used 2 bits for zones (4 values), 2 bits for types (4 values):**
```
Zone enumeration: 00=hands, 01=face, 02=ears, 03=neck
Type enumeration: 00=natural, 01=scars, 10=tattoos, 11=piercings
```

**Problem:** Can't say "no tattoos on hands OR face" - would need TWO contraquants.

**Result:** 8 hex digits instead of 2 to express simple concepts.

#### Chosen Approach: Flags (Bitmasks)

**Use 4 zone flags + 4 type flags:**
```
Zones: bit0=hands, bit1=face, bit2=ears, bit3=neck
Types: bit4=natural, bit5=scars, bit6=tattoos, bit7=piercings
```

**Benefit:** Can combine zones and types in ONE contraquant:
```
0x43 = No tattoos on (hands | face)
0xC1 = No (tattoos | piercings) on hands
0xFF = No marks of any kind anywhere
```

**This is much more compact and expressive.**

### Why Only 4 Anti-Zones (Not 48)?

#### Could Have Used All 48 Positive Zones

**Rejected approach:** Contraquants use same 48 zones as positive somidics

**Problems:**
1. **Verification tedious:** "Check each of 48 zones for absence" - impractical
2. **Overkill precision:** Don't need to distinguish "no tattoos on left index ring portion vs upper portion"
3. **Bit cost:** Would need 6 bits for zones, leaving only 2 for types

#### Chosen Approach: 4 Broad Zones

**Rationale:**
1. **Natural groupings:** People think in broad terms ("hands" not "left index upper portion")
2. **Efficient verification:** Scan entire hand in one pass
3. **Compact encoding:** 4 bits for zones, 4 bits for types
4. **Asymmetric by design:** Zone 0 verifiable even with long sleeves

**The 4 zones are strategically chosen:**

**Zone 0: Hands + Fingers + Wrists (30 positive zones)**
- **Killer feature:** Verifiable without disrobing (visible with long sleeves)
- **Cultural acceptability:** Universal
- **High coverage:** Most compressed contrazone

**Zone 1: Face (10 positive zones)**
- Always visible
- Highly discriminating (tattoos on face are uncommon in many populations)

**Zone 2: Ears (2 positive zones)**
- Piercings common here
- Usually visible

**Zone 3: Neck (2 positive zones)**
- Usually visible
- Tattoos here are distinctive

**Arms (zones 28-31) intentionally excluded:**
- Require more exposure (remove/roll up long sleeves)
- Modesty concerns
- Better to leave out for practical reasons

### Why Maximal Compaction?

#### Could Allow Redundant Anti-Somidics

**User might enter:**
```
@-41 (no tattoos on hands)
-42 (no tattoos on face)
```

**System could keep both:** `-41-42`

#### Chosen: Always Compact

**System automatically compacts to:** `-43`

**Rationale:**
1. **Shorter notation:** 2 hex digits instead of 4
2. **Faster verification:** One scan instead of two
3. **Clearer semantics:** "No tattoos on hands or face" vs. "No tattoos on hands AND no tattoos on face"
4. **Less storage:** Important for somidics
5. **Canonical form:** Prevents duplicate representations

**Maximum contraquants: 4**

Only achievable with 4 different zone patterns across the 4 type dimensions - practically absurd.

**In reality:**
- Most somidics: 0-1 contraquants
- Typical: Just one (like `4f` or `cf`)

### Why Verifier Discretion?

#### Critical Design Principle

**Contraquants are ALWAYS optional from verifier's perspective.**

#### Could Have Made Them Mandatory

**Rejected approach:** If somidic contains contraquants, verifier MUST check them

**Problems:**
1. Forces slow verification even in low-value contexts
2. Makes contraquants a burden instead of a benefit
3. Reduces flexibility

#### Chosen: Verifier Decides

**Verifier checks contraquants only when discrimination value justifies the time cost.**

**Benefits:**
1. **Graceful degradation:** Ignore antis = works like v0.3
2. **Context-appropriate:** High-value = check antis, low-value = skip them
3. **No downside:** Somidic holder pays no extra cost
4. **Market-driven:** Verifiers naturally optimize for their use case

**Example scenarios:**

**Grocery store (low-value):**
- Checks: Just first positive somidic
- Ignores: All contraquants
- Time: 30 seconds
- Acceptable fraud risk

**Jewelry store (high-value):**
- Checks: All positives + first contraquant
- Time: 90 seconds
- Much lower fraud risk

**Border control (very high-value):**
- Checks: All positives + all contraquants
- Time: 2-3 minutes
- Minimal fraud risk

### Why Hyphen Notation?

#### Semantic Meaning

**The hyphen represents "subtraction" or "exclusion":**
```
@+147293-41
  â”‚     â”‚
  â”‚     â””â”€ MINUS this characteristic (no tattoos)
  â””â”€â”€â”€â”€â”€â”€â”€ PLUS this characteristic (mole)
```

**Reads naturally:** "I have X minus Y"

#### Could Have Used Different Notation

**Rejected alternatives:**
```
@+147293+41    Confusing (+ implies adding)
@+147293/41    Looks like division
@+147293,41    Looks like list continuation
@+147293|41    Hard to type, unclear meaning
```

**Hyphen is best:**
- Visual clarity (clearly separates positives from antis)
- Semantic fit (subtraction/exclusion)
- Easy to type
- Won't confuse with positive somidics (no valid positive somidic contains hyphen)

### Why 8 Bits (Not 4, Not 16)?

#### 4 Bits Would Be Too Limiting

**With 4 bits total:**
- Could have 2 zone bits (enumeration) + 2 type bits (enumeration)
- **Problem:** Can't combine zones or types
- Would need multiple contraquants for simple expressions

**Example:**
```
"No tattoos on hands or face"
Would require: -xy-zw (two contraquants)
Instead of: -43 (one contraquant)
```

#### 16+ Bits Would Be Overkill

**With 16 bits:**
- Could encode more zones, more types, more detail
- **Problem:** Don't need that much information
- Contraquants should be simple and broad
- Compact notation is valuable

**Example:**
```
16-bit contraquant: -abcd (4 hex digits)
8-bit contraquant: -ab (2 hex digits)
```

The extra precision doesn't help, and the notation becomes unwieldy.

#### 8 Bits Is Optimal

**Perfect balance:**
- 4 zone flags = up to 4 broad zones
- 4 type flags = up to 4 mark types
- Can combine both dimensions
- Fits in 2 hex digits (human-readable)
- Maximum compaction to 1-4 contraquants

### Why Not Include Arms in Anti-Zones?

#### Could Have Made Arms a 5th Anti-Zone

**Would give 5 zones, requiring 5 bits (3-bit encoding with one wasted value).**

#### Excluded for Practical Reasons

**Arms (zones 28-31) are special:**
1. **Require more exposure:** Need to remove or roll up long sleeves
2. **Modesty concerns:** More revealing than hands/face
3. **Seasonal variation:** Covered in winter, exposed in summer
4. **Cultural variation:** Some cultures always cover arms

**Including arms would:**
- Reduce verifiability (can't always check)
- Increase intrusiveness (more disrobing needed)
- Create cultural issues (some populations can't participate)

**Better to exclude arms:**
- Zone 0 (hands+wrists) can be checked even with long sleeves
- Other zones (face, ears, neck) are universally visible
- Contraquants remain practical and culturally acceptable

**If need to exclude arms:**
- Can use positive zones (define forearm zones explicitly)
- Or accept that arms are not covered by contraquants
- Trade-off is acceptable for the benefits

### Why Override Principle?

#### Positive Somidics Are Exceptions to Anti-Somidics

**Example:**
```
@+147293-41
  Positive: "Tattoo on left wrist" (zone 32, in contrazone 0)
  Anti: "No tattoos on hands+fingers+wrists"
```

**Without override principle:**
- This would be contradictory
- "I have a tattoo" vs "I have no tattoos"

**With override principle:**
- Positive creates exception to anti
- Meaning: "I have THIS tattoo, but no OTHER tattoos in that zone"

**This is natural and useful:**
- Allows precise specification ("only one tattoo")
- Verification clear: check for specific tattoo, then scan for others
- Semantics make sense

### Discrimination Analysis

#### Single Positive Somidic
- Entropy: ~11.7 bits
- Discrimination: ~1-in-3,300

#### Single Anti-Somidic Added
- Anti entropy: ~2-3 bits (depends on population)
- Combined: ~14-15 bits
- Discrimination: ~1-in-15,000 to ~1-in-30,000
- **Improvement: 5-10Ã—**

#### Multiple Anti-Somidics
```
@+147293-cf (no tattoos or piercings anywhere)
  Positive: ~11.7 bits
  Anti: ~3-4 bits (more restrictive)
  Combined: ~15-16 bits
  Discrimination: ~1-in-30,000 to ~1-in-60,000
```

#### Multiple Positives + Antis
```
@@+147293:582047-cf
  Two positives: ~23 bits
  One anti: ~3 bits
  Combined: ~26 bits
  Discrimination: ~1-in-60-million
  **High-security somidic**
```

### Use Case Fit

#### Excellent Fit: Credit Cards

**Problem:** Card theft common, PIN can be observed
**Solution:** Card + somidic protection

**Without anti:**
```
@+147293 (mole on wrist)
Discrimination: ~1-in-3,300
Thief's chance: Not terrible
```

**With anti:**
```
@+147293-41 (mole on wrist, no tattoos on hands)
Discrimination: ~1-in-15,000
Thief's chance: Much lower
Verification time: +30 seconds (acceptable)
```

#### Excellent Fit: Government ID

**High-value somidic, time acceptable:**
```
@@+147293:582047-cf
  Two natural marks
  No tattoos or piercings anywhere
  
Discrimination: ~1-in-50-million
Verification: ~2 minutes (acceptable for border control)
```

#### New Capability: Professional Somidics

**Pure contraquant somidics:**
```
@-cf
  No positives needed
  Just asserts: "No tattoos or piercings anywhere"
  
Use case: Lawyer bar card, surgeon registry, diplomatic somidics
Discrimination: ~1-in-20 (professional populations)
```

This was impossible in v0.3 - contraquants enable it.

### What We Deliberately Don't Encode in Anti-Somidics

#### Size/Texture Information

**Could have encoded:** "No large tattoos" or "No raised marks"

**Rejected:**
- Adds complexity (more bits needed)
- Harder to verify (subjective judgments)
- Doesn't add much discrimination
- Keep contraquants simple and broad

**Chosen:** Contraquants only filter by zone and type (broad categories).

#### Specific Positive Zones

**Could have used:** All 48 positive zones in contraquants

**Rejected:**
- Would need 6 bits for zones
- Verification tedious
- Over-precision

**Chosen:** 4 broad contrazones that collapse 44 positive zones.

#### Arms as Anti-Zone

**Could have added:** Arms as 5th contrazone

**Rejected:**
- Modesty concerns
- Verification issues (clothing)
- Would need 5 bits (waste one encoding)

**Chosen:** 4 contrazones excluding arms.

## Version Evolution Summary (Updated for v0.5)

### v0.1 â†’ v0.2: Discrimination Enhancement
- 10-bit â†’ 13-bit somid
- 32 â†’ 48 zones (finger/hand/face subdivision)
- CRC-8 â†’ CRC-5 (trade error detection for discrimination)
- Added multiplicity attribute
- Result: ~7.7Ã— better discrimination

### v0.2 â†’ v0.3: Semantic Enhancement
- Same bit structure (13-bit somid, 5-bit CRC)
- Context-dependent bit 12 (tattoo writing, anomalous intensity)
- Missing/Anomalous texture repurposing (4 subtypes)
- Refined entropy analysis
- Result: Better real-world discrimination through smarter encoding

### v0.3 â†’ v0.5: Optional Discrimination Boost
- Added contraquants (8-bit flag encoding)
- Hyphen notation for negative assertions
- Maximal compaction principle
- Override principle for positives
- Verifier discretion
- Result: 5-20Ã— discrimination improvement when used, zero downside when ignored

### Design Philosophy Evolution

**v0.1:** Prove the concept
**v0.2:** Maximize theoretical entropy
**v0.3:** Optimize practical entropy given real-world constraints
**v0.5:** Add optional negative assertions for discrimination enhancement without compromising simplicity

## Open Design Questions for Future Versions

### Questions Still Under Consideration

1. **Private zone map (zones 48-63):**
   - What zones should be defined?
   - Medical examiner protocols?
   - Consent frameworks?

2. **Zone-specific enhancements:**
   - Could hands use quadrant encoding? (thumb-upper, thumb-lower, pinky-upper, pinky-lower)
   - Would face benefit from more granular zones?
   - Trade precision vs complexity?

3. **Animal somidics (Plane 1):**
   - What attributes matter most for animals?
   - How to handle species variation?
   - Should gender be encoded?

4. **Machine-assisted verification (optional):**
   - Computer vision to suggest zone?
   - Human remains in loop?
   - Privacy implications?

5. **Verification strictness levels:**
   - Should system define levels (permissive/standard/strict)?
   - Who sets the level (issuer/holder/verifier)?
   - How to audit compliance?

### Questions Resolved in v0.5

6. **Contraquants (RESOLVED):**
   - âœ… 8-bit flag encoding (4 zone flags + 4 type flags)
   - âœ… Maximal compaction principle
   - âœ… Verifier discretion (always optional)
   - âœ… Hyphen notation for negative assertions
   - âœ… 4 broad contrazones (hands+wrists, face, ears, neck)

### Areas for Future Research

- Empirical inter-rater reliability studies
- Cross-cultural acceptability testing
- Real-world discrimination rates
- Longitudinal stability studies
- Integration with somidic standards (mDL, W3C VC)

## Notation Design (v0.6 Addition)

### Why `@+` Notation?

**Decision:** Use `@` prefix for all somidics, `+` for each positive, `-` for each anti

**Rationale:**

1. **Unambiguous in text**: Always starts with `@`, no false positives from negative numbers
   - Old: `-cf` could be negative number or somidic
   - New: `@-cf` unambiguously a somidic

2. **Symmetric operators**: `+` and `-` are mathematically appropriate
   - `+` represents set union (∪)
   - `-` represents set difference (\)
   - Notation matches mathematical semantics

3. **Trivial read-aloud**: Just say operators as-is
   - See `+` → Say "PLUS"
   - See `-` → Say "MINUS"
   - No special rules, no exceptions

4. **Clear boundaries**: Each component explicitly marked
   - Old: `@147293:582047-41cf` (how to parse antis?)
   - New: `@+147293+582047-41-cf` (clear boundaries)

5. **Set theory alignment**: Notation IS the mathematics
   - `@+P₁+P₂-A₁-A₂` directly represents (P₁ ∪ P₂) \ (A₁ ∪ A₂)
   - Students can understand somidic meaning visually

**Rejected alternatives:**

**Option A: Keep `:` separators**
```
@:147293:582047-41
```
- Less semantic clarity
- "Next" vs "PLUS" for reading aloud
- `:` doesn't represent union operation

**Option B: `@:` prefix with colon after @**
```
@:147293
@:147293:582047
```
- Awkward double separator (`@:`)
- Colon serves two purposes (prefix and separator)
- Visually confusing

**Option C: No `@` prefix, hyphen only for antis**
```
147293
147293-41
-cf
```
- Ambiguous: `-cf` looks like negative number
- Cannot grep reliably for somidics
- No clear start marker

**Why we chose `@+` notation:**
- Minimal verbosity (single characters)
- Maximum clarity (unambiguous)
- Mathematical elegance (matches set theory)
- Practical utility (easy to read aloud, easy to parse)

### Reading Aloud Protocol Rationale

**Design goal:** Anyone can communicate somidics verbally without training.

**Our solution:**
- Say what you see: "PLUS" for `+`, "MINUS" for `-`
- Group digits by threes: "one four seven, two nine three"
- Always "zero" (never "oh" or "o")
- Don't say `@` prefix (implied context)

**Why this works:**
1. **Trivial to learn**: No memorization needed
2. **Error detection**: CRC-5 catches most transcription errors
3. **Universal**: Works in any language (numbers are universal, PLUS/MINUS translate easily)
4. **Natural**: Matches how people already communicate numbers

**Alternative considered: NATO phonetic alphabet**
- Too complex for general use
- Overkill for most contexts
- Keep it simple

### Decoding vs. Rendering

**Key distinction added in v0.6:**

**Decoding (Normative):**
- Convert somidic string to structured attributes
- All implementations MUST produce same structure
- Required for interoperability
- Example: `@+147293` → `{"zone": 32, "type": 0, "size": 2, ...}`

**Rendering (Non-Normative):**
- Convert structured attributes to natural language
- Implementations MAY adapt for language/culture
- Guidance provided, not requirements
- Example: `{"zone": 32, ...}` → "Single raised mole on left wrist"

**Why this separation:**
1. **Interoperability**: Structured format ensures compatibility
2. **Flexibility**: Natural language can adapt to culture
3. **Clarity**: Separates machine requirements from human preferences
4. **Extensibility**: New languages/contexts don't require spec changes

This design respects both technical precision (decoding) and human diversity (rendering).

## Conclusion

Somidics v0.6 represents a mature design that balances:
- **Theoretical elegance** with **practical constraints**
- **Discrimination power** with **stability over time**
- **Precision** with **fuzzy human judgment**
- **Innovation** with **simplicity**
- **Optional enhancement** with **zero downside**

The evolution from v0.1 through v0.5 shows progressive refinement while maintaining core principles: equipment-free, human-verifiable, stable, and privacy-preserving identification for contexts where traditional biometrics are impractical or inappropriate.

The evolution from v0.1 through v0.6 shows progressive refinement while maintaining core principles: equipment-free, human-verifiable, stable, and privacy-preserving identification for contexts where traditional biometrics are impractical or inappropriate.

Version 0.4 added contraquants for optional discrimination enhancement. Version 0.6 formalized the notation and human interaction protocols, making the system more usable while preserving all technical capabilities. The system can be extended with powerful new capabilities while remaining backward compatible (with documented migration paths) and preserving the fundamental design philosophy of verifier discretion and graceful degradation.
