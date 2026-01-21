# Somidics: Open Questions & Next Steps (v0.6 Updated)

## Status Summary

### What We've Defined (v0.5)

âœ… **Core concept:** Somidion-based identification using body marks
âœ… **Terminology:** Somidion, somid, somidic, somidics, contrasomidion, contrasomid, contraquant
âœ… **Encoding scheme:** 13-bit somid (zone + type + size + texture + multiplicity/special)
âœ… **Contraquants:** 8-bit flag encoding for negative assertions (NEW in v0.5)
âœ… **CRC validation:** CRC-5 for positive somidics (contraquants use flag validation)
âœ… **Plane architecture:** Humans (Plane 0), Animals (Plane 1), Reserved (Plane 2), Special (Plane 3)
âœ… **48 public zones:** Fingers (20), hands (8), arms (6), face (10), ears & neck (4)
âœ… **Notation (v0.6):** `@` prefix required for all, `+` for each positive, `-` for each anti, canonical ordering
âœ… **Read-aloud protocol (v0.6):** PLUS/MINUS convention, trivial rules
âœ… **Formal decoding (v0.6):** Normative structured output specification
âœ… **Rendering guidance (v0.6):** Non-normative natural language templates
âœ… **Anti-zone design:** 4 broad zones (hands+fingers+wrists, face, ears, neck)
âœ… **Maximal compaction:** Contraquants automatically compacted to minimum count
âœ… **Verifier discretion:** Contraquants always optional from verifier's perspective (NEW in v0.5)
âœ… **Override principle:** Positive somidics create exceptions to contraquants (NEW in v0.5)
âœ… **Design rationale:** Why each decision was made
âœ… **Evaluation framework:** 11 success criteria assessed (updated for v0.5)
âœ… **Use cases:** 6 detailed scenarios (credit card significantly enhanced, professional somidics added)

### What Remains Open

This document captures unresolved questions, design alternatives under consideration, and work needed to move from concept to implementation.

## Open Design Questions

### 1. Zone-Specific Follow-Up Bits

**Question:** Should certain zones use attribute bits differently for higher precision?

**Current status:** v0.5 uses context-dependent encoding (bit 12 varies by type/texture context)

**Already implemented context-dependent features (v0.3-v0.5):**
- Bit 12 means "multiplicity" for natural marks and scars
- Bit 12 means "writing indicator" for tattoos
- Bit 12 means "intensifier" for anomalous features
- Texture bits repurposed for missing/anomalous subtypes

**Still under consideration:**

**Option A: Uniform encoding (current baseline)**
- Context-dependent variations already in place
- Same bits mean same thing within type/texture contexts
- Simpler to implement and explain
- Some zones still less precise than optimal

**Option B: Zone-specific encoding**
- Hands encode quadrants (thumb-side, pinky-side, finger-side, wrist-side)
- Face encodes detailed subzones (13+ facial regions)
- Would require additional context flag
- More efficient but more complex

**Option C: Hybrid with zone flag**
- Use reserved bit to signal "zone-specific encoding"
- Allows gradual addition of zone-specific encodings
- Maintains backward compatibility

**Considerations:**
- Complexity vs. precision tradeoff
- Implementation difficulty
- User comprehension
- Future extensibility
- **V0.4 note:** Contraquants already provide discrimination boost, reducing urgency

**Recommendation needed:** Empirical testing to determine if precision gain justifies complexity

**Timeline:** Could be added in future version without breaking v0.5 compatibility (if designed carefully)

### 2. Private Somidics Specification

**Question:** What should the private zone map include?

**Current status:** Zones 48-63 reserved for private use, but no specification exists

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
- How many private zones needed? (16 available)
- Should there be sub-flags (medical vs. forensic vs. security)?
- What are the privacy/consent implications?
- **NEW v0.5:** Should private zones have corresponding anti-zones?

**Who should define this?**
- Medical examiner associations?
- Forensic science community?
- International standards body?
- Legal framework needed first?

**Timeline:** Not urgent for v1.0 public release, but should be addressed before v2.0

**V0.4 consideration:** Contraquants make private zones less urgent for public somidics (can use public zones + anti for good discrimination).

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
- Bits 9-12: Mark type/attributes

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

**V0.4 note:** Contraquants concept may not translate well to animals (different marking patterns).

### 4. Composite Somidions (Single Somidic Encoding Multiple Marks)

**Question:** Should one somidic encode multiple marks, or use multiple somidics?

**Current approach:** Multiple marks = multiple somidics with @ notation
- Example: `@147293:@+582047` encodes two separate marks
- v0.5: Can combine with contraquants: `@+147293+582047-cf`

**Alternative:** Encode 2-3 marks in a single somidic
- Lose CRC bits to gain space for second mark
- Example: 6 bits first somid + 6 bits second somid + 6 bits joint attributes + 0 CRC
- More compact but less flexible

**Tradeoffs:**

**Current (multiple somidics):**
- âœ… Flexible (use 1, 2, 3, or more)
- âœ… Strong CRC per mark
- âœ… Can mix human + animal (future)
- âœ… **Can combine with contraquants flexibly** (v0.5)
- âŒ More digits to communicate

**Alternative (composite):**
- âœ… Fewer total digits
- âœ… Atomic (can't separate marks)
- âŒ Fixed number of marks per somidic
- âŒ Weaker/no error detection
- âŒ Less flexible
- âŒ **Would complicate contraquant integration**

**Recommendation:** Keep current approach (multiple somidics), simpler and more flexible

**V0.4 note:** Contraquants provide an alternative path to higher discrimination without multiple positives, reducing pressure for composite encoding.

### 5. Somidic Rotation and Revocation

**Question:** How should somidic systems handle somidic changes?

**Scenarios requiring change:**

**Positive somidics:**
- Mark removed (tattoo removal, mole removed)
- Mark added (new large scar overshadows old somidion)
- Privacy concerns (want to use different mark)
- Mark becomes culturally inappropriate

**Contraquants (NEW considerations in v0.5):**
- Getting new tattoo in excluded zone â†’ invalidates contraquant
- Getting new piercing in excluded zone â†’ invalidates contraquant
- Removing tattoo â†’ might enable new contraquant
- **Advantage:** Can update just contraquant portion

**Options:**

**Option A: Reissue somidic**
- Similar to name change
- New somidic issued
- Old somidic invalid
- Simple but disruptive

**Option B: Update somidic**
- Keep somidic, update somidic field
- Digital somidics can be updated
- Requires revocation of old somidic
- **V0.4:** Can selectively update positive or anti portions

**Option C: Multiple somidics with validity dates**
- Somidic contains multiple somidics
- Each with "valid from" date
- Gradual transition supported

**Option D: Modular updates (NEW v0.5 consideration)**
- Positive somidics and contraquants tracked separately
- Can update contraquants without touching positive
- Example: `@+147293-41` â†’ `@+147293` (just drop anti if get hand tattoo)
- Simpler than full reissuance

**Questions:**
- How to prevent fraud (claiming mark changed to evade)?
- Should old somidic remain in revocation list?
- What verification does re-enrollment require?
- **NEW v0.5:** Should contraquant updates be easier than positive updates?

**Recommendation needed:** Standardize revocation/rotation protocol, consider modular updates

**V0.4 insight:** Contraquants are more likely to change than positive somidics (easier to get tattoo than to change birthmark), so modular updates are valuable.

### 6. Verification Strictness Levels

**Question:** Should verifiers use different strictness thresholds?

**Current approach:** Human judgment, "if close enough, accept"
**V0.4 addition:** Verifier discretion on checking contraquants

**Alternative:** Define strictness levels

**Level 1 (Permissive):**
- Any reasonable interpretation accepted
- Check positive only, skip contraquants
- Used for low-value transactions
- Example: Library card, gym membership
- Time: 30 seconds

**Level 2 (Standard):**
- Verifier should be reasonably confident
- Check positive, optionally check first contraquant
- Used for medium-value transactions
- Example: Credit card, prescription pickup
- Time: 45-60 seconds

**Level 3 (Strict):**
- Very close match required
- Check positive and all contraquants
- May require second opinion
- Used for high-value transactions
- Example: Border control, legal identity
- Time: 90-120 seconds

**Implementation:**
- Somidic encodes required minimum strictness level
- Verifier software displays appropriate guidance
- Training materials differ by level
- **V0.4:** Contraquant checking integrated into levels

**Questions:**
- Who sets the strictness level (issuer, holder, verifier)?
- How to audit strictness compliance?
- Does this undermine simplicity?
- **NEW v0.5:** How to balance strictness with verifier discretion principle?

**Recommendation:** V0.4's verifier discretion may be sufficient, add formalized levels only if empirical evidence shows need

**V0.4 note:** The verifier discretion principle already enables context-appropriate verification without formal strictness levels.

### 7. Machine-Assisted Verification

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
- **V0.4:** Could help verify contraquants (scan for tattoos)

**Challenges:**
- Requires equipment (camera)
- Requires ML training data
- Potential bias in CV system
- Privacy concerns (storing mark images?)
- Undermines equipment-free principle
- **V0.4:** Contraquant scanning might be harder for CV (absence detection)

**Recommendation:** Keep as future research direction, not in core spec
- Core spec remains equipment-free
- Optional CV extension for those who want it
- Must preserve human-in-the-loop
- **V0.4:** May be more useful for positive verification than contraquants

**Timeline:** Research project, not v1.0

## Technical Implementation Questions

### 8. Test Vectors and Reference Implementation

**Question:** What test vectors should specification include?

**Needed:**

**Positive somidics:**
- Known somidion â†’ somid â†’ CRC-5 â†’ somidic (full pipeline)
- Edge cases (zone 47, type combinations)
- Multiple somidics encoding/decoding
- Invalid somidics that should fail CRC-5

**Contraquants (NEW v0.5):**
- Known contrasomidion â†’ contrasomid â†’ hex encoding
- Flag combination examples
- Maximal compaction examples
- Invalid contraquants (nibble = 0)
- Combined positive+anti examples

**Example test vector (positive):**
```
Somidion: Raised natural mark on left forearm, coin-sized, single
Zone: 30 (left forearm) = 011110
Type: 00 (natural)
Size: 10 (coin-sized)
Texture: 01 (raised)
Multiplicity: 0 (single)
Somid: 0b0111100010010 = 3858 (decimal)
CRC-5: [to be computed]
Combined: [to be computed]
Somidic: [to be computed as 6-digit decimal]
```

**Example test vector (contraquant):**
```
Contrasomidion: No tattoos on hands, fingers, wrists
Anti-zone: 0001 (hands+fingers+wrists)
Anti-type: 0100 (tattoos)
Contrasomid: 0b01000001 = 0x41
Contraquant: 41 (2 hex digits)
```

**Example test vector (combined):**
```
Full somidic: "Mole on left wrist, no tattoos on hands"
Positive: @+147293
Anti: 41
Encoded: @+147293-41
```

**Reference implementation:**
- Python: Most accessible for specification
- JavaScript: For web implementations
- C: For embedded systems
- Should all produce identical results

**Action item:** Create test vectors and reference implementations in multiple languages

**V0.4 additions:**
- Contraquant encoding/decoding test vectors
- Maximal compaction algorithm test cases
- Override principle test scenarios
- Combined positive+anti test vectors

### 9. Somidic Storage Format

**Question:** How should somidics be stored in digital somidics?

**Options:**

**Option A: Plaintext somidic**
```json
{
  "somidic": "@+147293-41"
}
```
- Simple, clear
- âš ï¸ Verifier sees somidic directly
- **V0.4:** Contraquants visible too

**Option B: Hashed somidic**
```json
{
  "somidic_hash": "sha256_hash_of_@+147293-41"
}
```
- More private
- âŒ How does verifier decode to get description?
- **V0.4:** Loses ability to decode contraquants

**Option C: Encrypted somidic**
```json
{
  "somidic": "encrypted_@+147293-41",
  "encryption": "verifier_public_key"
}
```
- Only authorized verifiers can decode
- âŒ Complex key management

**Option D: Progressive disclosure**
```json
{
  "somidic_positive": "encrypted_147293",
  "somidic_anti": "encrypted_41"
}
```
- Reveal positive first, anti only if needed
- More complex but better privacy
- **V0.4:** Aligns with modular update principle

**Option E: Structured format (NEW v0.5)**
```json
{
  "somidic": {
    "positives": ["@+147293", "@+582047"],
    "antis": ["41"],
    "version": "0.4"
  }
}
```
- Clear separation of positive and anti
- Enables modular updates
- Easy to add/remove antis

**Recommendation:** Start with Option A (plaintext) for v1.0, add privacy-enhancing options in future versions. Consider Option E for clarity.

**V0.4 consideration:** Structured format enables modular updates and makes contraquant handling clearer.

**Related question:** Should somidic be in claim or presentation layer?

### 10. Multi-Somidic Cardinality

**Question:** What's the maximum useful number of somidics in a set?

**Current:** Unlimited (syntax supports N somidics)

**Considerations:**

**Positive somidics:**
- 1 somidic: ~1-in-3,300 (casual fraud prevention)
- 2 somidics: ~1-in-10-million (good identification)
- 3 somidics: ~1-in-1-billion (strong identification)
- 4+ somidics: Diminishing returns, increasingly intrusive

**Contraquants (NEW v0.5):**
- 1 anti with 1 positive: ~1-in-15,000+ (excellent for medium security)
- 2 antis with 1 positive: ~1-in-30,000+ (very good)
- 1 anti with 2 positives: ~1-in-60-million (high security)
- Multiple antis: Maximal compaction reduces to 1-4 typically

**Practical limits:**

**Positive somidics:**
- Verification time: 30 seconds Ã— N
- User comfort: Showing 5+ marks intrusive
- Finding suitable marks: Hard to find 4+ good marks

**Contraquants:**
- Verification time: 20-60 seconds Ã— N (depending on zones)
- User comfort: Less intrusive (no specific mark shown)
- Finding suitable antis: Most people have some exclusions

**Recommendation:** 
- Support 1-5 positive somidics technically
- Support 1-4 contraquants (maximal compaction limits naturally)
- Recommend 1-2 positives + 0-2 antis for most use cases
- Document tradeoffs

**V0.4 strategic insight:** Contraquants provide excellent discrimination boost per unit of intrusiveness. One positive + one anti may be optimal balance for many use cases.

### 11. Error Messages and User Feedback

**Question:** What should users see when somidic fails validation?

**Scenarios:**

**Scenario A: Invalid CRC (positive somidic)**
- Technical issue or typo
- Message: "This code is invalid. Please check the number."
- Don't reveal CRC internals to user

**Scenario B: Invalid contraquant format (NEW v0.5)**
- Nibble = 0 in contraquant
- Message: "This code format is incorrect. Please contact issuer."
- Rare (should not happen with proper encoding)

**Scenario C: Positive mark doesn't match**
- Potential fraud or mark changed
- Message: "Unable to verify this mark. Please contact issuer."
- Log for fraud detection

**Scenario D: Contraquant violation (NEW v0.5)**
- Person has marks in excluded zones
- Message: "Your body marks have changed. Please update your somidic."
- Could be fraud or legitimate change (got tattoo)
- **Different tone than positive mismatch** (more understandable)

**Scenario E: Mark changed (legitimate)**
- User reports mark removed/changed
- Message: "We'll need to update your somidic. Please re-enroll."
- Provide clear re-enrollment path
- **V0.4:** May only need to update anti portion

**Scenario F: Cultural/comfort issue**
- User uncomfortable showing mark
- Message: "Would you prefer a same-gender verifier or private area?"
- Provide accommodation options

**Scenario G: Verifier uncertain about contraquant (NEW v0.5)**
- Small ambiguous mark might be tattoo
- Message: "If uncertain, accept. Contraquants use benefit of doubt."
- Training emphasis on permissiveness

**Action item:** Develop comprehensive error message guidelines including v0.5 contraquant scenarios

**V0.4 note:** Contraquant violations should be treated as potentially legitimate changes (person got tattoo) rather than definite fraud.

## Standardization and Governance

### 12. Standards Body Engagement

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
- Verifiable Somidics working group
- Decentralized Identifiers
- Pros: Digital somidic focus
- Cons: Web-centric
- **V0.4:** Contraquants align well with progressive disclosure

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
2. Engage W3C VC group (digital somidics)
3. Approach ISO for international standard (long-term)

**V0.4 consideration:** Contraquants' verifier discretion principle aligns well with W3C progressive disclosure concepts.

### 13. Open Source Implementation

**Question:** Should there be an official open-source reference implementation?

**Components needed:**

**Positive somidics:**
- Encoding library (somidion â†’ somidic)
- Decoding library (somidic â†’ description)
- CRC-5 implementation
- Validation functions

**Contraquants (NEW v0.5):**
- Flag encoding/decoding
- Maximal compaction algorithm
- Override principle implementation
- Contraquant validation

**Combined features:**
- Combined notation parsing/generation
- Canonical ordering
- Complete test suite

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

**V0.4 additions:**
- Contraquant encoding/decoding modules
- Maximal compaction algorithm (well-specified)
- Test vectors for combined positive+anti

### 14. Patent and IP Considerations

**Question:** Should somidics be patented, or kept as open standard?

**Current status:** Not patented (as of January 2026)

**Options:**

**Option A: Patent defensively**
- File patent, license freely
- Prevents others from patenting
- Pros: Protection from patent trolls
- Cons: Creates perception of control
- **V0.4:** Would need to include contraquants in patent

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
- **V0.4:** Contraquants published in same paper

**Timeline:** Academic paper submission should happen before significant commercial interest

## Research Questions

### 15. Inter-Rater Reliability Studies

**Question:** How reliably do different verifiers agree on matches?

**Study design needed:**
- 100+ volunteers with various somidions
- 10+ verifiers evaluate each
- Measure agreement rates
- Identify factors affecting agreement
- **NEW v0.5:** Include contraquant verification agreement

**Hypotheses:**
- Agreement higher for obvious marks (large, high contrast)
- Agreement lower at size/texture boundaries
- Training improves agreement
- Cultural background affects interpretation
- **NEW v0.5:** Contraquant agreement may be higher (clearer criteria)

**Metrics:**
- Cohen's kappa (inter-rater agreement)
- False positive rate
- False negative rate
- Time to decision
- **NEW v0.5:** Contraquant agreement rates separately

**Funding needed:** Academic research grant

**V0.4 additions:**
- Separate metrics for positive vs. anti verification
- Agreement on when to check contraquants (verifier discretion)
- Effect of contraquants on overall confidence

### 16. Cross-Cultural Acceptability Research

**Question:** How do different cultures respond to somidic verification?

**Study design:**
- 10+ countries, various cultures
- Survey acceptance of different zones
- Observe actual verification interactions
- Document cultural adaptations
- **NEW v0.5:** Test contraquant acceptability specifically

**Questions:**
- Which zones are universally acceptable?
- Where is same-gender verification required?
- How do religious norms affect adoption?
- Are there zones we missed or shouldn't include?
- **NEW v0.5:** Are contraquants MORE acceptable in conservative cultures?

**Methodology:**
- Ethnographic observation
- Semi-structured interviews
- Survey of cultural consultants
- Pilot implementations

**Expected outcomes:**
- Updated zone recommendations
- Cultural guidance document
- Training materials for verifiers
- **NEW v0.5:** Contraquant cultural acceptability findings

**V0.4 hypothesis:** Anti-zone 0 (hands+fingers+wrists) may be MORE culturally acceptable than many positive zones since hands are visible even with conservative dress.

### 17. Real-World Discrimination Rates

**Question:** What percentage of population can be distinguished?

**Study design:**
- Catalog somidions in representative sample (N=1000+)
- Compute actual somidics
- Measure collision rates
- Analyze distribution across zones/attributes
- **NEW v0.5:** Measure tattoo/piercing prevalence for contraquant discrimination

**Questions:**
- Are some zone/type combinations more common?
- Does discrimination vary by population (age, ethnicity, geography)?
- How many people have NO suitable somidions?
- Are there unexpected patterns?
- **NEW v0.5:** What percentage have no tattoos/piercings (by zone)?
- **NEW v0.5:** How does tattoo prevalence vary by age/geography/culture?

**Challenges:**
- Privacy of collecting body mark data
- Representative sampling difficult
- Cultural sensitivity needed
- **NEW v0.5:** Collecting tattoo/piercing data sensitive

**Expected outcome:** Empirical entropy measurements, update theoretical model

**V0.4 specific questions:**
- Measured discrimination improvement with contraquants
- Contraquant effectiveness by population
- Optimal contraquant combinations
- Population segments where contraquants most effective

### 18. Longitudinal Stability Study

**Question:** How stable are somidions over 5, 10, 20 years?

**Study design:**
- Enroll participants, document somidions
- Follow-up at 1, 5, 10 years
- Measure change rates
- Document reasons for changes
- **NEW v0.5:** Track contraquant invalidation rates (getting tattoos)

**Metrics:**
- Percentage requiring re-enrollment
- Types of changes (removal, new marks, fading)
- Stability by mark type
- Age effects
- **NEW v0.5:** Contraquant invalidation rates and reasons

**Expected outcome:** 
- Reissuance rate estimates
- Mark type recommendations
- Stability guidelines
- **NEW v0.5:** Contraquant update frequency vs. positive update frequency

**Timeline:** Long-term study, preliminary results in 1-2 years

**V0.4 hypothesis:** Contraquants will have higher invalidation rates than positive somidics (easier to get tattoo than to lose birthmark), supporting modular update approach.

### 19. Verifier Discretion Study (NEW v0.5)

**Question:** When and how do verifiers choose to check contraquants?

**Study design:**
- Field deployment with optional contraquant somidics
- Track when verifiers check vs. skip contraquants
- Correlate with transaction value, time pressure, fraud history
- Interview verifiers about decision factors

**Questions:**
- What percentage of verifications include contraquant checking?
- How does transaction value affect checking rate?
- Does fraud history influence checking behavior?
- Are there systematic biases in checking decisions?
- Does checking rate correlate with fraud reduction?

**Metrics:**
- Contraquant checking rate by context
- Time cost of contraquant verification
- Fraud rate with vs. without anti-checking
- Verifier satisfaction with discretion

**Expected outcome:**
- Guidelines for when to check contraquants
- Training materials for verifier discretion
- Evidence for/against formalized strictness levels

**Timeline:** 6-month field study after v1.0 deployment

### 20. Anti-Somidic Security Analysis (NEW v0.5)

**Question:** How effective are contraquants against different attack types?

**Study design:**
- Red team attacks on contraquant somidics
- Attempt to fake absence of marks
- Attempt tattoo removal/cover-up
- Measure success rates

**Attack scenarios:**
- Attacker with tattoos tries to use "no tattoos" somidic
- Attacker attempts temporary cover-up
- Attacker attempts to find confederate without tattoos
- Sophisticated attacker laser removal attempt

**Metrics:**
- Attack success rate by method
- Time/cost to execute successful attack
- Verifier detection rate
- Comparison to positive-only somidics

**Expected outcome:**
- Real-world security validation of contraquants
- Identification of vulnerabilities
- Improved verifier training

**Timeline:** Post-v1.0 deployment, requires deployed system to test against

## Practical Next Steps

### Phase 1: Specification Finalization (Months 1-3)

**Month 1:**
- âœ… Complete core specification document v0.5 (DONE)
- âœ… Finalize terminology including contraquants (DONE)
- âœ… Contraquant encoding fully specified (DONE)
- â³ Define test vectors (positive + anti)
- â³ Specify exact CRC-5 parameters
- â³ Create reference implementation (Python) including contraquants

**Month 2:**
- Write academic paper (draft) including v0.5 features
- Create visual aids and diagrams (including contraquant notation)
- Develop enrollment UX mockups (with contraquant flow)
- Build simple demonstration app (showing verifier discretion)

**Month 3:**
- Submit paper to academic conference
- Release specification v0.5 (public review)
- Open-source reference implementation
- Create documentation website (with contraquant examples)

### Phase 2: Validation and Testing (Months 4-6)

**Month 4:**
- Conduct inter-rater reliability pilot study (N=50) including contraquants
- Test reference implementation across platforms
- Gather feedback from identity experts on v0.5 features
- Refine based on feedback

**Month 5:**
- Implement verification demo system with verifier discretion
- Test with diverse volunteers
- Document edge cases and solutions
- Create verifier training materials (including contraquant guidance)

**Month 6:**
- Analyze pilot study results (positive and anti verification)
- Update specification if needed
- Prepare case studies (especially credit card use case)
- Plan larger field trials

### Phase 3: Field Trials (Months 7-12)

**Month 7-9:**
- Partner with humanitarian organization (refugee camp trial)
- Partner with rural health clinic (patient ID trial)
- **NEW:** Partner with financial institution (credit card pilot with contraquants)
- Document implementations
- Collect usage data (including verifier discretion behavior)

**Month 10-12:**
- Analyze field trial results
- Measure contraquant discrimination improvement
- Assess verifier discretion patterns
- Update specification to v1.0
- Publish findings
- Engage standards bodies

### Phase 4: Standardization (Year 2+)

- W3C VC integration proposal (with contraquant support)
- ISO standardization process
- Industry adoption outreach (focus on credit card industry for contraquants)
- Developer ecosystem building

## Critical Path Items

### Must-Have for v1.0
1. âœ… Core encoding specification (DONE - v0.5)
2. âœ… 48 public zones defined (DONE - v0.5)
3. âœ… Contraquant encoding specified (DONE - v0.5)
4. â³ CRC-5 parameters specified (NEEDS EXACT PARAMS)
5. âœ… Terminology finalized (DONE - v0.5)
6. â³ Reference implementation (IN PROGRESS - needs contraquants)
7. â³ Test vectors (NEEDED - positive + anti)
8. â³ Academic paper (IN PROGRESS - needs v0.5 update)

### Should-Have for v1.0
9. Verification UX guidelines (including verifier discretion)
10. Verifier training materials (contraquant emphasis)
11. Error handling specification (contraquant scenarios)
12. Multiple somidics handling (positive + anti combinations)
13. Somidic storage format recommendations (structured format for v0.5)
14. Contraquant maximal compaction algorithm specification

### Nice-to-Have for v1.0
15. Zone-specific encoding extensions (lower priority with contraquants)
16. Machine-assisted verification (optional) - including contraquant scanning
17. Privacy-enhancing protocols (progressive disclosure of contraquants)
18. Integration examples (mDL, VC) with contraquants
19. Modular update protocol specification (positive vs. anti)

### Future Versions
20. Private somidics specification (v1.5)
21. Animal somidics specification (v2.0)
22. Advanced features (zone-specific encoding, etc.)
23. International standardization
24. Contraquant extensions (more zones, finer granularity)

## Decision Log

### Decisions Made
- âœ… Use "somidics" terminology (paralleling biometrics)
- âœ… 13-bit somid encoding (v0.2, maintained through v0.6)
- âœ… CRC-5 for validation (v0.2, maintained through v0.6)
- âœ… Plane architecture (262k per plane)
- âœ… `@+` notation for all somidics (v0.6)
- âœ… Separated contraquants with `-` prefix each (v0.6)
- âœ… Read-aloud protocol: PLUS/MINUS (v0.6)
- âœ… Canonical ordering (numerically ascending)
- âœ… Equipment-free human verification (no ML required)
- âœ… Fuzzy matching as feature (not bug)
- âœ… Exclude color encoding (stability reasons)
- âœ… 48 public zones (v0.2, maintained in v0.5)
- âœ… Context-dependent bit 12 encoding (v0.3, maintained in v0.5)
- âœ… **Contraquants implemented** (v0.5)
- âœ… **8-bit flag encoding for contraquants** (v0.5)
- âœ… **Maximal compaction principle** (v0.5)
- âœ… **Verifier discretion principle** (v0.5)
- âœ… **Hyphen notation for contraquants** (v0.5)
- âœ… **4 broad anti-zones excluding arms** (v0.5)
- âœ… **Override principle (positives are exceptions)** (v0.5)

### Decisions Deferred
- â¸ï¸ Zone-specific follow-up bits (evaluate in future version)
- â¸ï¸ Private somidics map (needs medical examiner input)
- â¸ï¸ Animal somidics design (separate working group)
- â¸ï¸ Machine-assisted verification (optional extension)
- â¸ï¸ Somidic storage encryption (start with plaintext)
- â¸ï¸ Formalized strictness levels (v0.5 verifier discretion may be sufficient)

### Decisions Resolved (Previously Open, Now Closed)

**4. Negative Somidics (RESOLVED in v0.5):**
- âœ… Implemented as contraquants
- âœ… Flag encoding (not enumeration)
- âœ… Maximal compaction
- âœ… Verifier discretion
- âœ… Override principle
- **Timeline:** Completed in v0.5 (January 2026)

**5. Notation Format (RESOLVED in v0.6):**
- âœ… `@` prefix required for all somidics
- âœ… `+` operator for each positive (set union)
- âœ… `-` operator for each anti (set difference)
- âœ… Trivial read-aloud protocol (say operators)
- âœ… Unambiguous parsing (no false positives)
- **Timeline:** Completed in v0.6 (January 2026)

**6. Decoding/Rendering Specification (RESOLVED in v0.6):**
- âœ… Normative decoding to structured format
- âœ… Non-normative rendering guidance
- âœ… Multi-language considerations
- âœ… Context-specific verbosity levels
- **Timeline:** Completed in v0.6 (January 2026)

### Open for Discussion
- â“ Verification strictness levels (needs empirical data, v0.5 may have addressed with discretion)
- â“ Maximum useful somidic count in set (recommend 1-2 positives + 0-2 antis)
- â“ Standards body engagement strategy
- â“ Patent vs. prior art approach
- â“ Modular update protocol (contraquants separate from positives)
- â“ Contraquant extensions (more zones? finer granularity?)

## Contact and Collaboration

### How to Contribute

**For researchers:**
- Propose empirical studies (especially contraquant focused)
- Share findings from related work
- Suggest improvements to methodology

**For implementers:**
- Test reference implementation (including contraquants)
- Report bugs and edge cases
- Contribute code (open source)
- Test verifier discretion implementations

**For standards experts:**
- Review specification (especially v0.5 additions)
- Suggest integration points (W3C VC, ISO)
- Advise on standardization process

**For identity/humanitarian practitioners:**
- Propose use cases
- Field test in real contexts
- Provide feedback on practical challenges
- Test contraquant verification workflows

**For financial/fraud prevention experts (NEW v0.5):**
- Evaluate contraquants for fraud prevention
- Propose credit card/payment use cases
- Test verifier discretion in retail contexts
- Measure discrimination improvement

### Mailing List / Forum
(To be established)

### GitHub Repository
(To be created for reference implementation with contraquant support)

### Academic Contacts
(To be added after paper submission with v0.5 features)

## Conclusion

Somidics v0.6 represents a mature and feature-complete design for equipment-free, human-verifiable identification. The addition of contraquants resolves a major open question and significantly expands the system's capabilities without compromising its core principles.

**Immediate priorities:**
1. Finalize CRC-5 parameters and test vectors (including contraquants)
2. Complete reference implementation with full v0.5 support
3. Submit academic paper with v0.5 features
4. Conduct pilot studies including contraquant effectiveness

**Medium-term goals:**
5. Field trials in humanitarian contexts AND financial/fraud prevention
6. Engage standards bodies (W3C, ISO) with v0.5 spec
7. Build developer ecosystem with contraquant support
8. Refine based on empirical evidence (especially verifier discretion)

**Long-term vision:**
9. International standard for equipment-free biometrics
10. Wide adoption in low-resource contexts
11. Significant adoption in fraud prevention (credit cards)
12. Integration with digital somidic standards (mDL, VC)
13. Extensions for specialized use cases (medical, animal, professional)

**V0.5 Update:**

Version 0.6 formalizes notation and human interaction protocols, making the system more usable without changing any technical capabilities. The `@+` notation provides unambiguous parsing, trivial read-aloud rules, and mathematical clarity.

**V0.4 Impact Summary:**

The resolution of the "negative somidics" question through contraquants represents a major milestone. Contraquants provide:

1. **Dramatic discrimination improvement** (3-10Ã—) without requiring additional body exposure
2. **Asymmetric defense** against sophisticated attacks (can't fake absence)
3. **Verifier discretion** enabling context-appropriate security
4. **New use cases** (professional somidics with pure contraquants)
5. **Cultural acceptability** (hands+fingers+wrists verifiable with long sleeves)
6. **Modular updates** (can update anti portion independently)

The path from concept to deployed standard is long, but v0.6 represents a complete, well-designed system ready for standardization. The notation is clear, the protocols are documented, and the technical foundation is solid. The need is clear, the approach is sound, and contraquants provide a powerful enhancement that maintains the core philosophy of equipment-free, human-verifiable identification with appropriate privacy protections.

With thoughtful implementation, rigorous evaluation, and community collaboration, somidics v0.5 can fill an important gap in the identification technology landscape, particularly in fraud prevention and contexts where equipment-free verification is critical.
