# Somidics: Open Questions & Next Steps

## Status Summary

### What We've Defined (v0.1)

✅ **Core concept:** Somidion-based identification using body marks
✅ **Terminology:** Somidion, somid, somidic, somidics
✅ **Encoding scheme:** 10-bit somid (zone + type + size + texture)
✅ **CRC validation:** CRC-8 for error detection
✅ **Plane architecture:** Humans (Plane 0), Animals (Plane 1), Reserved (Plane 2), Special (Plane 3)
✅ **32 public zones:** Fingers, hands, arms, face, ears, neck
✅ **Notation:** @ prefix, colon-separated, canonical ordering
✅ **Design rationale:** Why each decision was made
✅ **Evaluation framework:** 11 success criteria assessed
✅ **Use cases:** 5 detailed scenarios

### What Remains Open

This document captures unresolved questions, design alternatives under consideration, and work needed to move from concept to implementation.

## Open Design Questions

### 1. Zone-Specific Follow-Up Bits

**Question:** Should certain zones use attribute bits differently for higher precision?

**Current status:** v0.1 uses uniform attribute encoding (type/size/texture) for all zones

**Alternatives:**

**Option A: Uniform encoding (current)**
- Same bits mean same thing for all zones
- Simpler to implement and explain
- Some zones are less precise than optimal

**Option B: Zone-specific encoding**
- Hands encode quadrants (thumb-side, pinky-side, finger-side, wrist-side)
- Face encodes detailed subzones (13+ facial regions)
- Fingers encode segment (tip, middle, base) and side (pad, back)
- More efficient but more complex

**Option C: Hybrid with flag bit**
- Bit 5 (first attribute bit) signals "zone-specific encoding"
- If 0: standard attributes
- If 1: zone-specific interpretation
- Allows gradual addition of zone-specific encodings

**Considerations:**
- Complexity vs. precision tradeoff
- Implementation difficulty
- User comprehension
- Future extensibility

**Recommendation needed:** Empirical testing to determine if precision gain justifies complexity

**Timeline:** Could be added in v0.2 without breaking v0.1 compatibility (if designed carefully)

### 2. Private Somidics Specification

**Question:** What should the private zone map include?

**Current status:** Zone bit pattern 11111 reserved for private use, but no specification exists

**Potential private zones:**
- Torso (chest, abdomen, back)
- Upper legs / thighs
- Feet / toes
- Scalp (if bald or short hair)
- Intimate areas (medical examiner only)
- Buttocks, groin (forensic only)

**Use cases:**
- Medical examiner identification of deceased
- Forensic analysis
- Full-body medical documentation
- Specialized security applications

**Design questions:**
- Should private zones use same attribute encoding?
- How many private zones needed? (32 available in 5 bits)
- Should there be sub-flags (medical vs. forensic vs. security)?
- What are the privacy/consent implications?

**Who should define this?**
- Medical examiner associations?
- Forensic science community?
- International standards body?
- Legal framework needed first?

**Timeline:** Not urgent for v0.1 public release, but should be addressed before v1.0

### 3. Animal Somidics Design

**Question:** What attributes matter for animal identification?

**Current status:** Plane 1 (262144-524287) reserved but unspecified

**Key differences from human somidics:**
- Species/breed highly variable (St. Bernard vs. Chihuahua)
- Gender is identifying (unlike humans where it's not polite to encode)
- Coat patterns major discriminator (stripes, spots)
- Less precise body part language (not everyone knows "fetlock")
- Size categories more important
- Different mark types (natural patterns vs. brands/tags)

**Proposed attribute schema (draft):**
- Bits 0-3: Animal class (16 types: dog, cat, horse, cattle, sheep, etc.)
- Bits 4-5: Gender (4 options: male, female, neutered-male, spayed-female)
- Bits 6-8: Body zone (8 zones: head, neck, body, tail, legs, feet, ears)
- Bits 9-10: Mark type (4 types: natural, brand, tag, other)

**Challenges:**
- Species variation enormous
- Coat patterns complex
- Veterinary terminology needed?
- International coordination (different animals important in different regions)

**Who should define this?**
- Veterinary associations
- Animal identification experts (e.g., horse registries)
- Wildlife biologists (for conservation tracking)

**Prior art to review:**
- Horse identification systems (face markings, leg markings)
- Cattle branding practices
- Pet microchip standards
- Wildlife tracking methodologies

**Timeline:** Separate working group needed, not blocking human somidics v1.0

### 4. Negative Somidics

**Question:** Should we support assertions about marks NOT present?

**Original idea:** "I have a tattoo on front of neck BUT NOT on back of neck"

**Potential encoding:**
- Use a special zone flag?
- Add a "negative" bit to attribute encoding?
- Create separate somidic that encodes absence?

**Use case:**
- Slightly more identifying (eliminates some population)
- Could distinguish twins (one has mark, other doesn't)
- Unusual but potentially useful edge case

**Challenges:**
- More complex to explain
- Lower discrimination value (negative info less useful)
- Verification awkward ("Show me you DON'T have X")
- Might be confusing

**Recommendation:** Defer to v2.0 or later, not essential for v0.1

### 5. Composite Somidions (Single Somidic Encoding Multiple Marks)

**Question:** Should one somidic encode multiple marks, or use multiple somidics?

**Current approach:** Multiple marks = multiple somidics with @ notation
- Example: `@147293:582047` encodes two separate marks

**Alternative:** Encode 2-3 marks in a single somidic
- Lose CRC bits to gain space for second mark
- Example: 5 bits first somid + 5 bits second somid + 8 bits joint CRC
- More compact but less flexible

**Tradeoffs:**

**Current (multiple somidics):**
- ✅ Flexible (use 1, 2, 3, or more)
- ✅ Strong CRC per mark
- ✅ Can mix human + animal (future)
- ❌ More digits to communicate

**Alternative (composite):**
- ✅ Fewer total digits
- ✅ Atomic (can't separate marks)
- ❌ Fixed number of marks per somidic
- ❌ Weaker error detection
- ❌ Less flexible

**Recommendation:** Keep current approach (multiple somidics), simpler and more flexible

### 6. Somidic Rotation and Revocation

**Question:** How should credential systems handle somidic changes?

**Scenarios requiring change:**
- Mark removed (tattoo removal, mole removed)
- Mark added (new large scar overshadows old somidion)
- Privacy concerns (want to use different mark)
- Mark becomes culturally inappropriate

**Options:**

**Option A: Reissue credential**
- Similar to name change
- New somidic issued
- Old credential invalid
- Simple but disruptive

**Option B: Update credential**
- Keep credential, update somidic field
- Digital credentials can be updated
- Requires revocation of old somidic

**Option C: Multiple somidics with validity dates**
- Credential contains multiple somidics
- Each with "valid from" date
- Gradual transition supported

**Questions:**
- How to prevent fraud (claiming mark changed to evade)?
- Should old somidic remain in revocation list?
- What verification does re-enrollment require?

**Recommendation needed:** Standardize revocation/rotation protocol

### 7. Verification Strictness Levels

**Question:** Should verifiers use different strictness thresholds?

**Current approach:** Human judgment, "if close enough, accept"

**Alternative:** Define strictness levels

**Level 1 (Permissive):**
- Any reasonable interpretation accepted
- Used for low-value transactions
- Example: Library card, gym membership

**Level 2 (Standard):**
- Verifier should be reasonably confident
- Used for medium-value transactions
- Example: Credit card, prescription pickup

**Level 3 (Strict):**
- Very close match required
- May require second opinion
- Used for high-value transactions
- Example: Border control, legal identity

**Implementation:**
- Credential encodes required strictness level
- Verifier software displays appropriate guidance
- Training materials differ by level

**Questions:**
- Who sets the strictness level (issuer, holder, verifier)?
- How to audit strictness compliance?
- Does this undermine simplicity?

**Recommendation:** Start without levels, add if empirical evidence shows need

### 8. Machine-Assisted Verification

**Question:** Should we add computer vision as an OPTIONAL aid (not replacement)?

**Current approach:** 100% human verification, no ML/CV

**Alternative:** Computer vision suggests zone, verifier confirms

**Workflow:**
1. Camera captures image of mark
2. CV suggests: "Looks like left forearm, raised mark, coin-sized"
3. Human verifier confirms or overrides
4. Decision remains with human

**Benefits:**
- Faster verification
- Consistent suggestions
- Training aid for verifiers
- Could work for remote verification

**Challenges:**
- Requires equipment (camera)
- Requires ML training data
- Potential bias in CV system
- Privacy concerns (storing mark images?)
- Undermines equipment-free principle

**Recommendation:** Keep as future research direction, not in core spec
- Core spec remains equipment-free
- Optional CV extension for those who want it
- Must preserve human-in-the-loop

## Technical Implementation Questions

### 9. CRC-8 Parameter Details

**Question:** Should we specify exact CRC-8 algorithm parameters?

**Current status:** "Standard CRC-8 with polynomial 0x07"

**Need to specify:**
- Initial value: 0x00 or 0xFF?
- Final XOR: 0x00 or 0xFF?
- Input reflection: true or false?
- Output reflection: true or false?
- Bit order: MSB or LSB first?

**Recommendation:** 
```
Polynomial: 0x07 (x^8 + x^2 + x + 1)
Initial value: 0x00
Final XOR: 0x00
Input reflection: false
Output reflection: false
Process bits: MSB to LSB
```

**Rationale:** Simplest configuration, widely supported

**Action item:** Add to specification with test vectors

### 10. Test Vectors and Reference Implementation

**Question:** What test vectors should specification include?

**Needed:**
- Known somidion → somid → CRC → somidic (full pipeline)
- Edge cases (zone 31, type combinations)
- Multiple somidics encoding/decoding
- Invalid somidics that should fail CRC

**Example test vector:**
```
Somidion: Raised natural mark on left forearm, coin-sized
Zone: 10000 (left forearm)
Type: 00 (natural mark)
Size: 10 (coin-sized)
Texture: 01 (raised)
Somid: 0b1000000010 = 514 (decimal)
CRC-8: [to be computed]
Combined: [to be computed]
Somidic: [to be computed as 6-digit decimal]
```

**Reference implementation:**
- Python: Most accessible for specification
- JavaScript: For web implementations
- C: For embedded systems
- Should all produce identical results

**Action item:** Create test vectors and reference implementations in multiple languages

### 11. Credential Storage Format

**Question:** How should somidics be stored in digital credentials?

**Options:**

**Option A: Plaintext somidic**
```json
{
  "somidic": "147293"
}
```
- Simple, clear
- ⚠️ Verifier sees somidic directly

**Option B: Hashed somidic**
```json
{
  "somidic_hash": "sha256_hash_of_147293"
}
```
- More private
- ❌ How does verifier decode to get description?

**Option C: Encrypted somidic**
```json
{
  "somidic": "encrypted_147293",
  "encryption": "verifier_public_key"
}
```
- Only authorized verifiers can decode
- ❌ Complex key management

**Option D: Progressive disclosure**
```json
{
  "somidic_zone": "encrypted_left_forearm",
  "somidic_full": "encrypted_147293"
}
```
- Reveal zone first, full details only if needed
- More complex but better privacy

**Recommendation:** Start with Option A (plaintext), add privacy-enhancing options in future versions

**Related question:** Should somidic be in claim or presentation layer?

### 12. Multi-Somidic Cardinality

**Question:** What's the maximum useful number of somidics in a set?

**Current:** Unlimited (syntax supports N somidics)

**Considerations:**
- 1 somidic: ~1-in-1,000 (casual fraud prevention)
- 2 somidics: ~1-in-1,000,000 (good identification)
- 3 somidics: ~1-in-1-billion (strong identification)
- 4+ somidics: Diminishing returns, increasingly intrusive

**Practical limits:**
- Verification time: 30 seconds × N
- User comfort: Showing 5+ marks intrusive
- Finding suitable marks: Hard to find 4+ good marks

**Recommendation:** 
- Support 1-5 somidics technically
- Recommend 1-3 for most use cases
- Document tradeoffs

### 13. Error Messages and User Feedback

**Question:** What should users see when somidic fails validation?

**Scenarios:**

**Scenario A: Invalid CRC**
- Technical issue or typo
- Message: "This code is invalid. Please check the number."
- Don't reveal CRC internals to user

**Scenario B: Mark doesn't match**
- Potential fraud or mark changed
- Message: "Unable to verify this mark. Please contact issuer."
- Log for fraud detection

**Scenario C: Mark changed (legitimate)**
- User reports mark removed/changed
- Message: "We'll need to update your credential. Please re-enroll."
- Provide clear re-enrollment path

**Scenario D: Cultural/comfort issue**
- User uncomfortable showing mark
- Message: "Would you prefer a same-gender verifier or private area?"
- Provide accommodation options

**Action item:** Develop comprehensive error message guidelines

## Standardization and Governance

### 14. Standards Body Engagement

**Question:** Which standards organizations should be involved?

**Potential bodies:**

**ISO (International Organization for Standardization):**
- ISO/IEC JTC 1/SC 17 (Cards and security devices)
- ISO/TC 215 (Health informatics)
- Pros: International recognition, formal process
- Cons: Slow, requires significant resources

**IETF (Internet Engineering Task Force):**
- Potential for RFC on somidic format
- Integration with digital identity standards
- Pros: Internet-focused, open process
- Cons: Limited biometric expertise

**W3C (World Wide Web Consortium):**
- Verifiable Credentials working group
- Decentralized Identifiers
- Pros: Digital credential focus
- Cons: Web-centric

**FIDO Alliance:**
- Authentication standards
- Could add as FIDO2 extension
- Pros: Industry backing
- Cons: Technology-focused

**IEEE (Institute of Electrical and Electronics Engineers):**
- Biometric standards groups
- Pros: Technical expertise
- Cons: Engineering-focused

**Recommendation:** 
1. Start with academic publication (credibility)
2. Engage W3C VC group (digital credentials)
3. Approach ISO for international standard (long-term)

### 15. Open Source Implementation

**Question:** Should there be an official open-source reference implementation?

**Components needed:**
- Encoding library (somidion → somidic)
- Decoding library (somidic → description)
- CRC-8 implementation
- Validation functions
- Test vectors

**Languages:**
- Python (specification reference)
- JavaScript (web/mobile apps)
- Java/Kotlin (Android)
- Swift (iOS)
- C (embedded systems)

**Licensing:**
- MIT or Apache 2.0 (permissive)
- Allow commercial use
- Encourage adoption

**Governance:**
- Who maintains reference implementation?
- How are changes approved?
- How to prevent fragmentation?

**Recommendation:** Create reference implementations, host on GitHub, establish governance

### 16. Patent and IP Considerations

**Question:** Should somidics be patented, or kept as open standard?

**Current status:** Not patented (as of January 2026)

**Options:**

**Option A: Patent defensively**
- File patent, license freely
- Prevents others from patenting
- Pros: Protection from patent trolls
- Cons: Creates perception of control

**Option B: Publish as prior art**
- Academic paper + open specification
- Prevents future patents via prior art
- Pros: Truly open, no perception issues
- Cons: No defensive protection

**Option C: Patent pool**
- Multiple parties contribute patents
- Free licensing for standard implementations
- Pros: Industry collaboration
- Cons: Complex to establish

**Recommendation:** Option B (publish as prior art)
- Academic paper establishes prior art
- Open specification prevents lock-in
- Encourage wide adoption

## Research Questions

### 17. Inter-Rater Reliability Studies

**Question:** How reliably do different verifiers agree on matches?

**Study design needed:**
- 100+ volunteers with various somidions
- 10+ verifiers evaluate each
- Measure agreement rates
- Identify factors affecting agreement

**Hypotheses:**
- Agreement higher for obvious marks (large, high contrast)
- Agreement lower at size/texture boundaries
- Training improves agreement
- Cultural background affects interpretation

**Metrics:**
- Cohen's kappa (inter-rater agreement)
- False positive rate
- False negative rate
- Time to decision

**Funding needed:** Academic research grant

### 18. Cross-Cultural Acceptability Research

**Question:** How do different cultures respond to somidic verification?

**Study design:**
- 10+ countries, various cultures
- Survey acceptance of different zones
- Observe actual verification interactions
- Document cultural adaptations

**Questions:**
- Which zones are universally acceptable?
- Where is same-gender verification required?
- How do religious norms affect adoption?
- Are there zones we missed or shouldn't include?

**Methodology:**
- Ethnographic observation
- Semi-structured interviews
- Survey of cultural consultants
- Pilot implementations

**Expected outcomes:**
- Updated zone recommendations
- Cultural guidance document
- Training materials for verifiers

### 19. Real-World Discrimination Rates

**Question:** What percentage of population can be distinguished?

**Study design:**
- Catalog somidions in representative sample (N=1000+)
- Compute actual somidics
- Measure collision rates
- Analyze distribution across zones/attributes

**Questions:**
- Are some zone/type combinations more common?
- Does discrimination vary by population (age, ethnicity, geography)?
- How many people have NO suitable somidions?
- Are there unexpected patterns?

**Challenges:**
- Privacy of collecting body mark data
- Representative sampling difficult
- Cultural sensitivity needed

**Expected outcome:** Empirical entropy measurements, update theoretical model

### 20. Longitudinal Stability Study

**Question:** How stable are somidions over 5, 10, 20 years?

**Study design:**
- Enroll participants, document somidions
- Follow-up at 1, 5, 10 years
- Measure change rates
- Document reasons for changes

**Metrics:**
- Percentage requiring re-enrollment
- Types of changes (removal, new marks, fading)
- Stability by mark type
- Age effects

**Expected outcome:** 
- Reissuance rate estimates
- Mark type recommendations
- Stability guidelines

**Timeline:** Long-term study, preliminary results in 1-2 years

## Practical Next Steps

### Phase 1: Specification Finalization (Months 1-3)

**Month 1:**
- ✅ Complete core specification document (DONE)
- ✅ Finalize terminology (DONE)
- ✅ Define test vectors
- ✅ Specify exact CRC-8 parameters
- Create reference implementation (Python)

**Month 2:**
- Write academic paper (draft)
- Create visual aids and diagrams
- Develop enrollment UX mockups
- Build simple demonstration app

**Month 3:**
- Submit paper to academic conference
- Release specification v0.1 (public review)
- Open-source reference implementation
- Create documentation website

### Phase 2: Validation and Testing (Months 4-6)

**Month 4:**
- Conduct inter-rater reliability pilot study (N=50)
- Test reference implementation across platforms
- Gather feedback from identity experts
- Refine based on feedback

**Month 5:**
- Implement verification demo system
- Test with diverse volunteers
- Document edge cases and solutions
- Create verifier training materials

**Month 6:**
- Analyze pilot study results
- Update specification if needed
- Prepare case studies
- Plan larger field trials

### Phase 3: Field Trials (Months 7-12)

**Month 7-9:**
- Partner with humanitarian organization (refugee camp trial)
- Partner with rural health clinic (patient ID trial)
- Document implementations
- Collect usage data

**Month 10-12:**
- Analyze field trial results
- Update specification to v1.0
- Publish findings
- Engage standards bodies

### Phase 4: Standardization (Year 2+)

- W3C VC integration proposal
- ISO standardization process
- Industry adoption outreach
- Developer ecosystem building

## Critical Path Items

### Must-Have for v1.0
1. ✅ Core encoding specification (DONE)
2. ✅ 32 public zones defined (DONE)
3. ✅ CRC-8 parameters specified (NEEDS EXACT PARAMS)
4. ✅ Terminology finalized (DONE)
5. Reference implementation (IN PROGRESS)
6. Test vectors (NEEDED)
7. Academic paper (IN PROGRESS)

### Should-Have for v1.0
8. Verification UX guidelines
9. Verifier training materials
10. Error handling specification
11. Multiple somidics handling
12. Credential storage format recommendations

### Nice-to-Have for v1.0
13. Zone-specific encoding extensions
14. Machine-assisted verification (optional)
15. Privacy-enhancing protocols
16. Integration examples (mDL, VC)

### Future Versions
17. Private somidics specification (v1.5)
18. Animal somidics specification (v2.0)
19. Advanced features (negative somidions, etc.)
20. International standardization

## Decision Log

### Decisions Made
- ✅ Use "somidics" terminology (paralleling biometrics)
- ✅ 10-bit somid encoding
- ✅ CRC-8 for validation
- ✅ Plane architecture (262k per plane)
- ✅ @ notation for multiple somidics
- ✅ Canonical ordering (numerically ascending)
- ✅ Equipment-free human verification (no ML required)
- ✅ Fuzzy matching as feature (not bug)
- ✅ Exclude color encoding (stability reasons)
- ✅ Public zones only for v0.1 (private reserved for later)

### Decisions Deferred
- ⏸️ Zone-specific follow-up bits (evaluate in v0.2)
- ⏸️ Private somidics map (needs medical examiner input)
- ⏸️ Animal somidics design (separate working group)
- ⏸️ Negative somidions (v2.0 or later)
- ⏸️ Machine-assisted verification (optional extension)
- ⏸️ Credential storage encryption (start with plaintext)

### Open for Discussion
- ❓ Verification strictness levels (needs empirical data)
- ❓ Maximum useful somidic count in set (recommend 1-3)
- ❓ Standards body engagement strategy
- ❓ Patent vs. prior art approach

## Contact and Collaboration

### How to Contribute

**For researchers:**
- Propose empirical studies
- Share findings from related work
- Suggest improvements to methodology

**For implementers:**
- Test reference implementation
- Report bugs and edge cases
- Contribute code (open source)

**For standards experts:**
- Review specification
- Suggest integration points
- Advise on standardization process

**For identity/humanitarian practitioners:**
- Propose use cases
- Field test in real contexts
- Provide feedback on practical challenges

### Mailing List / Forum
(To be established)

### GitHub Repository
(To be created for reference implementation)

### Academic Contacts
(To be added after paper submission)

## Conclusion

Somidics v0.1 represents a solid foundation for equipment-free, human-verifiable identification. Many design questions have been resolved, but important work remains:

**Immediate priorities:**
1. Finalize CRC-8 parameters and test vectors
2. Complete reference implementation
3. Submit academic paper
4. Conduct pilot studies

**Medium-term goals:**
5. Field trials in humanitarian contexts
6. Engage standards bodies (W3C, ISO)
7. Build developer ecosystem
8. Refine based on empirical evidence

**Long-term vision:**
9. International standard for equipment-free biometrics
10. Wide adoption in low-resource contexts
11. Integration with digital credential standards
12. Extensions for specialized use cases (medical, animal, etc.)

The path from concept to deployed standard is long, but the need is clear and the approach is sound. With thoughtful implementation, rigorous evaluation, and community collaboration, somidics can fill an important gap in the identification technology landscape.
