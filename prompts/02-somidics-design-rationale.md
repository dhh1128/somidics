# Somidics Design Rationale v0.3

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
- Is a mark "grain-sized" or "fingernail-sized"? → Fuzzy boundary
- Is it "raised" or "flush"? → Sometimes subtle
- Is it on "left cheek" or "nose"? → If near boundary

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
- Entropy gain: log₂(20) vs log₂(10) = 4.32 vs 3.32 bits (+1 bit)

**Natural language examples:**
- "Scar on ring portion of left index finger"
- "Mole on upper portion of right thumb"

### Hand Subdivision (v0.2 Innovation)

**Why subdivide into thumb-side/pinky-side:**
- Hands are large zones with many marks
- Natural reference points (thumb side vs pinky side immediately clear)
- Entropy gain: log₂(8) vs log₂(4) = 3 vs 2 bits (+1 bit)

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
- ✗ Modesty concerns (requires removing pants)
- ✗ Practical concerns (less accessible)
- ✗ Cultural unacceptability in many contexts

**Feet/toes:**
- ✗ Requires shoe removal (impractical)
- ✗ Hygiene concerns
- ✗ Less common to have distinctive marks

**Torso (chest, back, abdomen):**
- ✗ Requires clothing removal
- ✗ Strong modesty concerns
- ✓ Moved to reserved zones (48-63) for forensic use only

**Scalp/hair:**
- ✗ Hair coverage varies
- ✗ Marks not consistently visible
- ✗ Cultural variations in hair covering

**Teeth:**
- ✗ Too unstable (dental work changes)
- ✗ Requires opening mouth (intimate)
- ✗ Not reliably visible

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
- Intentional → person chose to be identifiable

### Size: Why Four Categories?

**Design pressure:** Need human-comparable references

**Rejected approaches:**
- ✗ Millimeter measurements - Requires ruler, too precise
- ✗ "Small/medium/large" - Too vague, only 3 categories
- ✗ Comparison to body parts (thumb joint) - Requires looking at person's thumb

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
- Repurposing gives 4× discrimination for these rare but important cases
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

#### For Anomalous Features (Type=10, Texture≠00):
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
- ✗ Fades over time (scars, tattoos)
- ✗ Changes with sun exposure
- ✗ Subjective to skin tone
- ✗ Lighting-dependent
- ✗ Reduces stability

**This was a hard decision** - color is very identifying! But:
- Stability is more important than discrimination
- A somidic should be valid for decades
- "Dark mole" becomes "faded mole" → breaks the encoding

**Origin (birth/accident/intentional) - Excluded**

**Why not?**
- ✗ Not observable at verification time
- ✗ Doesn't help verifier make determination
- ✗ Adds complexity without utility

**Age of mark - Excluded**

**Why not?**
- ✗ Not determinable by looking
- ✗ Not helpful for matching
- ✗ Would be guesswork

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
- Gained 3 bits for somid (10→13 bits)
- 13-bit somid with dependencies ≈ 11.9 bits real entropy
- Much better than 10-bit somid with dependencies ≈ 8-9 bits
- Trade 2.7% error detection for ~3× discrimination improvement

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
- For anomalous (texture≠00), bit 12 means mild/severe
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
- Old encoding: "dark tattoo" → breaks when it fades
- Our encoding: "tattoo with writing on left forearm, coin-sized" → still valid

### When Re-issuance IS Required

**Acceptable reasons:**
- Tattoo completely removed
- Mole surgically removed
- New prominent scar overshadows old somidion
- Missing body part (amputation)

**These are analogous to:**
- Name changes (require credential re-issue)
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

2. **Anomalous intensifier:** Since Type=10+Texture≠00 identifies anomalies, repurpose bit 12
   - Gain: ~0.06 bits average

3. **Missing texture repurposing:** Since Type=10 texture was wasted, create subtypes
   - Gain: ~0.06 bits average (from better discrimination of rare cases)

**Result:** Partial recovery of lost entropy through smarter encoding

## Success Criteria Review

### What We Optimized For

1. ✅ **Equipment-free** - Zero cost, works anywhere
2. ✅ **Human-verifiable** - No training needed
3. ✅ **Memorable** - Remember the mark, not the number
4. ✅ **Stackable** - Multiple somidics for higher security
5. ✅ **Stable** - Decades of validity
6. ✅ **Privacy-preserving** - Fuzzy enough to avoid overreach
7. ✅ **Culturally adaptable** - Respects modesty norms
8. ✅ **Practical** - Works in real-world scenarios

### What We Explicitly Did NOT Optimize For

1. ✗ **Courtroom-proof identification** - Too strong for our use case
2. ✗ **High-throughput** - Human verification is inherently slow
3. ✗ **Video-call verification** - Requires good cameras, lighting
4. ✗ **Perfect precision** - Would sacrifice stability
5. ✗ **Uniqueness guarantee** - 1-in-3,300 is good enough
6. ✗ **Universal coverage** - Some people may lack suitable marks

## Version Evolution Summary

### v0.1 → v0.2: Discrimination Enhancement
- 10-bit → 13-bit somid
- 32 → 48 zones (finger/hand/face subdivision)
- CRC-8 → CRC-5 (trade error detection for discrimination)
- Added multiplicity attribute
- Result: ~7.7× better discrimination

### v0.2 → v0.3: Semantic Enhancement
- Same bit structure (13-bit somid, 5-bit CRC)
- Context-dependent bit 12 (tattoo writing, anomalous intensity)
- Missing/Anomalous texture repurposing (4 subtypes)
- Refined entropy analysis (more accurate correlation modeling)
- Result: Better real-world discrimination through smarter encoding

### Design Philosophy Evolution

**v0.1:** Prove the concept
**v0.2:** Maximize theoretical entropy
**v0.3:** Optimize practical entropy given real-world constraints

The progression shows increasing sophistication while maintaining core simplicity and stability principles.

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

### Areas for Future Research

- Empirical inter-rater reliability studies
- Cross-cultural acceptability testing
- Real-world discrimination rates
- Longitudinal stability studies
- Integration with credential standards (mDL, W3C VC)

## Conclusion

Somidics v0.3 represents a mature design that balances:
- **Theoretical elegance** with **practical constraints**
- **Discrimination power** with **stability over time**
- **Precision** with **fuzzy human judgment**
- **Innovation** with **simplicity**

The evolution from v0.1 through v0.3 shows progressive refinement while maintaining core principles: equipment-free, human-verifiable, stable, and privacy-preserving identification for contexts where traditional biometrics are impractical or inappropriate.
