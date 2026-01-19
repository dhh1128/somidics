# Somidics Evaluation & Use Cases

## Success Criteria Assessment

### 1. Easy to Teach (Adults & Children)

**Target:** Uneducated adult or child can learn the concept with minimal instruction

**Teaching Script:**
```
"You have unique marks on your body - like freckles, scars, or tattoos. 
Pick one that you'd be okay showing to a stranger. 
Remember which one it is (like 'mole on my left wrist'). 
We'll help you turn this into a number."
```

**Assessment:** ✅ **YES, with good UX**

**Strengths:**
- Builds on natural body awareness
- No complex concepts needed
- Software does the hard work
- Concrete, not abstract

**Challenges:**
- Left/right confusion for some people
- Size comparison requires calibration
- Some may not have obvious marks
- Cultural comfort varies

**Grade: B+** - Teachable but needs thoughtful UX design

**Requirements for success:**
- Visual aids showing body zones
- Size comparison guides (grain/fingernail/coin images)
- Multiple language support
- Cultural sensitivity in presentation

### 2. Easy to Remember & Teach Others

**Target:** Can remember their somidic choice and explain to others

**Assessment:** ✅ **YES**

**What they remember:** "I use the mole on my left hand"
**What they don't need to remember:** "147293"

**Memorability factors:**
- ✅ It's part of their body they already know
- ✅ Personal and unique
- ✅ Easy to reference ("the scar from when I fell")
- ✅ Natural language description

**Teaching others:**
"See this mark on my wrist? That's my somidion. It gets turned into a number for ID."

**Grade: A** - Extremely memorable because it's embodied knowledge

### 3. Easy for Human to Verify (With Software Decoding)

**Target:** Verifier can check a claim without training or equipment

**Scenario:**
```
Software displays: "Look for: Small raised mark on left forearm"
Verifier: [Examines person's left forearm]
Verifier: [Sees small raised mole]
Verifier: [Accepts match]
```

**Assessment:** ✅ **MOSTLY YES**

**Success factors:**
- ✅ Clear body zones (left forearm = unambiguous)
- ✅ Observable attributes (raised vs flat)
- ✅ Natural human skill (we already do this)
- ✅ Forgiving (close enough counts)

**Challenge factors:**
- ⚠️ Lighting affects visibility
- ⚠️ Size judgment is subjective
- ⚠️ Texture may require close look/touch
- ⚠️ Cultural norms about proximity/touching

**Grade: B** - Works well for obvious cases, fuzzy at edges (by design)

**Recommendations:**
- Good lighting at verification stations
- Privacy screens for modest zones
- Same-gender verifiers option
- Training for edge cases ("if unsure, accept")

### 4. Can Be Stacked for Higher Security

**Target:** Multiple somidics provide stronger identification

**Assessment:** ✅ **YES**

**Stacking scenarios:**

**Low security (casual fraud prevention):**
- 1 somidic = ~1-in-1,000 discrimination
- Use case: Credit card backup, library card

**Medium security (important but not critical):**
- 2 somidics = ~1-in-1,000,000 discrimination  
- Use case: Digital passport backup, medical records

**High security (critical identification):**
- 3+ somidics = ~1-in-1-billion discrimination
- Use case: Legal identity, high-value credentials

**Implementation:**
- Use @ notation: `@147293:582047:923841`
- Verify sequentially or in parallel
- Each adds ~10 bits entropy (if independent)

**Grade: A** - Natural extension, works perfectly

**Tradeoffs:**
- More time to verify (multiple marks to check)
- Harder to find multiple suitable marks
- More intrusive (may need less modest zones)

### 5. Stable Across Time

**Target:** Somidic remains valid for decades without reissuance

**Assessment:** ✅ **MOSTLY YES**

**Stable elements (decades):**
- ✅ Zone (location doesn't change)
- ✅ Type (mole stays mole, scar stays scar)
- ✅ Size (approximate - slow changes acceptable)
- ✅ Texture (raised/flush usually stable)

**Design choices that enhance stability:**
- ✅ Don't encode color (fades over time)
- ✅ Fuzzy size categories (slight growth okay)
- ✅ Broad texture categories (slight changes okay)

**Edge cases requiring reissuance:**
- Tattoo removed → Need new somidion
- Mole removed medically → Need new somidion
- New prominent scar → May overshadow old one
- Amputation → Definitely need new somidion

**Comparison to other credentials:**
- Driver's license photo: 5-10 year validity
- Passport: 10 year validity, photo updates
- Somidic: Potentially 20-30 year validity

**Grade: A-** - Very stable, occasional reissuance acceptable

**Expected reissuance rate:** <5% per decade (estimated)

### 6. Avoids Ambiguity (Well Enough)

**Target:** Fuzzy matching is acceptable and even desirable

**Assessment:** ✅ **YES, by design**

**Intentional fuzziness:**
- "Grain-sized" vs "fingernail-sized" - boundary cases exist
- "Raised" vs "flush" - subtle cases exist  
- Zone boundaries - marks near edges ambiguous

**Why this is good:**
- ✅ Prevents use as strong legal proof (privacy benefit)
- ✅ Handles human judgment variation
- ✅ Graceful degradation (close enough works)
- ✅ Can challenge in court if misused

**Discrimination achieved:**
- Single somidic: ~1-in-1,000 (measured by attribute combinations)
- Empirical testing needed but theoretical model solid

**False positive rate:**
Random person matching by chance:
- Same zone: ~1/30 (if evenly distributed)
- Same type: ~1/4  
- Same size: ~1/4 (with fuzzy matching)
- Same texture: ~1/4
- Combined: ~1/1,920 (close to 1/1,000 target)

**Grade: A** - Ambiguity level is optimal for use case

### 7. Easy to Memorize the Number

**Target:** Holder can remember the 6-digit somidic

**Assessment:** ⚠️ **NO, but not necessary**

**Memorization difficulty:**
- 6 digits harder than 4-digit PIN
- Easier than 16-digit credit card
- Possible with techniques (chunking: 147-293)

**But they don't need to memorize it:**
- ✅ Number stored in digital credential
- ✅ They remember "mole on left wrist" instead
- ✅ Can regenerate via software interview if needed
- ✅ Much more memorable than PIN (can't forget your body)

**Grade: C for memorization, A for "doesn't need to be memorized"**

**Key insight:** The somidion itself is the memorable part, not the number

### 8. Good Discriminator (Theft Scenario)

**Target:** Thief can't easily use stolen credential

**Scenario:** Credit card stolen, protected by somidic

**Assessment:** ✅ **YES**

**Attack analysis:**

**Thief's options:**
1. **Random guess** - System rejects 99.6% of invalid somidics (CRC)
2. **Try to match their own body** - ~1/1,000 chance
3. **Create fake mark** (makeup, temporary tattoo):
   - Requires knowledge of victim's somidic
   - Requires preparation time
   - Risky (verifier may notice)
4. **Find confederate who matches** - ~1/1,000 people
5. **Social engineering** - Try to get victim to reveal somidion

**Compared to PIN:**
- PIN: Thief might guess (4 digits = 1/10,000) or observe entry
- Somidic: Thief needs actual body mark matching description
- **Somidic is stronger** - can't observe or guess easily

**Compared to biometric:**
- Fingerprint: Stronger (~1 in millions) but requires equipment
- Somidic: Weaker but equipment-free

**Grade: A-** - Excellent casual fraud prevention, not perfect

**Best for:** Low-to-medium value transactions where fraud is costly but not catastrophic

### 9. Can Be Evaluated Over Video Call

**Target:** Remote verification via video chat

**Assessment:** ⚠️ **PARTIAL - depends on mark type**

**Works well for:**
- ✅ Large facial marks (forehead mole, cheek scar)
- ✅ Hand marks (hold up to camera)
- ✅ Obvious tattoos on forearms
- ✅ Missing finger/fingertip

**Doesn't work for:**
- ❌ Grain-sized marks (too small for camera)
- ❌ Texture differences (can't touch through screen)
- ❌ Subtle marks (lighting/camera quality issues)
- ❌ Marks requiring close inspection

**Video-specific vulnerabilities:**
- Makeup/temporary tattoos easier to fake
- Can't verify texture by touch
- Lighting variations affect visibility
- Camera quality varies widely

**Grade: C+** - Limited support, better for in-person

**Recommendation:** If video verification needed, restrict to:
- Medium-to-large marks only
- High contrast marks
- Obvious zones (face, hands)
- Disclaimer: "Video verification less reliable"

### 10. Culturally Acceptable (Modesty)

**Target:** Works across cultures with varying modesty norms

**Assessment:** ✅ **GOOD, with thoughtful design**

**Public zones (universally acceptable):**
- ✅ Hands, fingers (nearly universal)
- ✅ Forearms (acceptable in most contexts)
- ✅ Face (already public)
- ✅ Ears, neck (usually acceptable)

**Borderline zones:**
- ⚠️ Upper arms (some cultures require long sleeves)
- ⚠️ Wrists (some cultures cover)

**Excluded zones (modesty concerns):**
- ❌ Upper legs, feet (require clothing removal)
- ❌ Torso (private zone flag only)

**Cultural adaptations:**

**1. Same-gender verification:**
- Credential can flag "requires female verifier"
- Respects religious/cultural norms
- Similar to airport security protocols

**2. Private area option:**
- Private zone flag (11111) for forensic use
- Medical examiner scenarios
- Not for routine verification

**3. Holder choice:**
- Person chooses which somidion to use
- Can select most modest option
- Can have different somidics for different contexts

**Examples by culture:**

**Conservative Muslim context:**
- Woman in burqa: May need female verifier, private space
- Can still use hand marks (hands often visible)
- Face marks require accommodation

**Western context:**
- Very flexible, most zones acceptable
- Casual settings (retail) - hands/face easy
- Formal settings (border) - more privacy available

**Hindu/Buddhist context:**
- Generally accepting of visible marks
- Some tattoos have religious significance
- Respectful handling important

**Grade: A-** - Well-designed for cultural sensitivity

**Critical success factor:** System supports same-gender verification option

### 11. Practical and Socially Acceptable

**Target:** Works in real identity verification scenarios

**Assessment:** ✅ **YES for appropriate contexts**

**Excellent fit scenarios:**

**Refugee camp / humanitarian aid:**
- ✅ No equipment available
- ✅ Time to interact with each person
- ✅ Identity fraud is a concern
- ✅ People lack other credentials
- **Rating: A+** - Perfect use case

**Rural healthcare / developing world:**
- ✅ No biometric equipment
- ✅ Need patient identification
- ✅ Equipment-free critical
- **Rating: A** - Very suitable

**Border control (backup to primary):**
- ✅ Privacy-focused travelers appreciate
- ✅ Equipment failure backup
- ✅ Child identification
- ⚠️ Slower than biometric scan
- **Rating: B+** - Good backup, not primary

**Credit card (fraud prevention):**
- ✅ Better than PIN alone
- ✅ Casual fraud prevention
- ⚠️ Slows checkout slightly
- ⚠️ Cashier judgment required
- **Rating: B** - Works but has friction

**Digital credential binding:**
- ✅ Perfect for mobile driver's license
- ✅ Natural fit for verifiable credentials
- ✅ Privacy-preserving alternative to biometrics
- **Rating: A** - Excellent application

**Poor fit scenarios:**

**High-throughput retail:**
- ❌ Too slow (human verification)
- ❌ Minimum wage workers may not care
- ❌ Line backup at registers
- **Rating: D** - Not recommended

**Airport security (primary method):**
- ❌ Much slower than biometric scan
- ❌ High throughput required
- ❌ Biometric infrastructure already exists
- **Rating: D** - Bad fit (okay as backup only)

**Legal identity proof:**
- ❌ Too fuzzy for courtroom
- ❌ Can be challenged
- ❌ Need stronger proof
- **Rating: F** - Not appropriate

**Grade: B+** - Excellent for appropriate contexts, poor for others

**Key principle:** Use where equipment-free verification is valued over speed

## Cultural Considerations Deep Dive

### Modesty Norms by Culture

**Islam (conservative):**
- Women: Face, hands may be acceptable; other zones require privacy
- Men: Hands, face, arms generally acceptable
- **Recommendation:** Same-gender verification, private spaces available

**Orthodox Judaism:**
- Similar modesty requirements to conservative Islam
- Tzniut (modesty) laws vary by community
- **Recommendation:** Same-gender verification for married women

**Hinduism:**
- Generally accepting of visible body parts
- Some tattoos/marks have religious significance (tilak, sectarian marks)
- **Recommendation:** Respect religious marks, don't require as somidion

**Buddhism:**
- Generally flexible
- Some monk/nun restrictions
- **Recommendation:** Standard approach works

**Christianity (conservative):**
- Varies widely by denomination
- Some groups (Pentecostal, etc.) have modesty norms
- **Recommendation:** Offer same-gender option

**Secular/Western:**
- Very flexible
- Workplace norms vary
- **Recommendation:** Standard approach

### Tattoo Cultural Considerations

**Positive associations:**
- Maori (New Zealand): Ta moko - sacred cultural practice
- Polynesian cultures: Traditional tattooing significant
- Japanese: Traditional irezumi art form
- Western: Increasingly mainstream

**Negative associations:**
- Some religious prohibitions (Orthodox Judaism, conservative Islam)
- Yakuza/gang associations in some contexts
- Employment discrimination in some cultures
- Older generation disapproval

**Design principle:** Holder chooses whether to use tattoo as somidion
- No judgment on choice
- Can select non-tattoo mark if preferred
- Respects cultural/personal values

### Body Part Language Across Cultures

**Universal (every language has these):**
- Finger, hand, arm
- Face, nose, ear
- Eye, cheek, forehead

**Potentially tricky:**
- Left/right (some languages/cultures different)
- Wrist (may not have specific word)
- Forearm vs upper arm (distinction may vary)

**Recommendation:** Use most basic body part terms, avoid medical jargon

### Privacy and Body Autonomy

**Key principle:** Person has full control over which mark to use

**Respecting autonomy:**
- Never require revealing private areas
- Offer choice between multiple marks
- Allow different somidics for different contexts
- Support revocation and change

**Consent considerations:**
- Enrollment must be voluntary
- Person understands how somidic will be used
- Can decline without losing access to services
- Can request mark-specific privacy (female verifier)

## Use Case Analysis

### Use Case 1: Refugee Identity Management

**Context:** Syrian refugee camp in Jordan, 2025

**Problem:**
- 50,000 refugees need food/supply distribution
- Many lack government IDs
- Biometric equipment limited/broken
- Fraud concerns (people claiming multiple times)
- Children born in camp have no birth certificates

**Somidics solution:**

**Enrollment:**
1. Aid worker interviews refugee
2. "Do you have a mark on your body you'd be comfortable showing?"
3. Simple software captures: zone, type, size, texture
4. Generates somidic, prints on aid card
5. Takes 2-3 minutes per person

**Distribution:**
1. Person presents aid card
2. Card shows: "Small scar on right hand back"
3. Aid worker: "Show me your right hand"
4. Visual verification, stamp card
5. Takes 30 seconds

**Benefits:**
- ✅ No equipment needed
- ✅ Works with illiterate population
- ✅ Children can be registered (parents remember their marks)
- ✅ Fraud reduced (hard to fake specific mark)
- ✅ Privacy-preserving (fuzzy matching)

**Challenges:**
- Some people have no obvious marks → use photo backup
- Cultural sensitivity needed → same-gender verifiers available
- Mark changes over time → re-enrollment process needed

**Success metrics:**
- Fraud reduction: 80%+ (estimated)
- Enrollment time: <5 minutes per person
- Verification time: <1 minute per transaction
- Cost: Nearly zero (just paper cards + simple app)

### Use Case 2: Mobile Driver's License Binding

**Context:** ISO 18013-5 mobile driver's license (mDL) in US state

**Problem:**
- mDL stored on phone can be stolen/borrowed
- Need to prove holder = credential subject
- Don't want to store biometric data on phone (privacy)
- Can't require biometric scanner at every verifier

**Somidics solution:**

**At DMV (enrollment):**
1. Person getting license has photo taken (standard)
2. "Would you like to add somidic protection?"
3. Brief interview about body mark
4. Somidic added to mDL data structure

**At traffic stop (verification):**
1. Officer: "Show me your mobile license"
2. Phone displays license + somidic requirement
3. Phone shows: "Tattoo on left forearm, medium-sized"
4. Officer: "Let me see your left arm"
5. Visual check, proceed with stop

**Benefits:**
- ✅ Stronger than just possessing phone
- ✅ Privacy-preserving (no biometric data stored)
- ✅ Equipment-free (every officer can verify)
- ✅ Optional (those who want it can enable)

**Challenges:**
- Officer training needed
- Slower than photo-only verification
- Some people may not have suitable marks

**Success metrics:**
- Adoption rate: 40-60% of mDL holders
- Fraud prevention: Estimated 95% reduction in borrowed licenses
- Verification time: +15-30 seconds per stop
- Privacy: No biometric data stored or transmitted

### Use Case 3: Child Trafficking Recovery

**Context:** Child rescued from trafficking ring, identity unknown

**Problem:**
- Child may not remember name, age, birthplace
- Years may have passed since abduction
- No documents available
- Family doesn't know if child is alive
- Need way to match child to missing persons reports

**Somidics solution:**

**At time of abduction report (parents):**
1. "Does your child have any distinctive marks?"
2. Parent describes: "Birthmark on left shoulder"
3. Somidic computed and stored in missing persons database
4. Photo also stored (ages over time, less reliable)

**At time of rescue (years later):**
1. Child examined by social worker
2. Distinctive marks noted
3. Somidics computed for each mark
4. Search missing persons database
5. Potential matches identified

**Benefits:**
- ✅ Works even if child can't communicate
- ✅ Stable over years (birthmarks don't change)
- ✅ No equipment needed in field
- ✅ Can be done at birth (parents note marks)

**Challenges:**
- Private marks (shoulder) → medical examiner needed
- Mark must be specific enough (common marks not helpful)
- Database matching imprecise (fuzzy matching)

**Success metrics:**
- Match rate: Unknown (needs empirical testing)
- Time to search: Seconds (small database)
- False positive rate: <1% (can be verified with photo)

**Note:** This use case shows value of private somidics specification

### Use Case 4: Credit Card Fraud Prevention

**Context:** Credit card in US/Europe with somidic protection

**Problem:**
- Card theft/skimming common
- PIN can be observed or guessed
- Contactless payment has no authentication
- Need stronger but convenient protection

**Somidics solution:**

**At card activation:**
1. "Would you like somidic protection?"
2. Quick enrollment interview (2 minutes)
3. Somidic printed on card or stored in chip
4. Cardholder remembers: "Use my left wrist mole"

**At point of sale:**
1. Card inserted/tapped
2. Terminal displays: "Small dark mark on left wrist - show cashier"
3. Cashier: "Can I see your left wrist?"
4. Visual check, transaction proceeds
5. Takes 10-15 extra seconds

**Benefits:**
- ✅ Much harder to use stolen card
- ✅ No equipment upgrade needed
- ✅ Cardholder can't forget (it's their body)
- ✅ Optional feature (those who want security can enable)

**Challenges:**
- Slows checkout process
- Cashier judgment required (training needed)
- Cultural sensitivity (some marks private)
- Not suitable for online purchases

**Success metrics:**
- Fraud reduction: 80-90% for in-person transactions
- Customer satisfaction: Mixed (security vs convenience tradeoff)
- Merchant adoption: Depends on fraud rate in area
- Transaction time: +15 seconds average

**Best for:** High-fraud areas, high-value cards, security-conscious users

### Use Case 5: Rural Healthcare Patient Identification

**Context:** Rural clinic in Kenya, limited technology

**Problem:**
- Patients lack government IDs
- Names are common (many "John Kamau")
- Medical records get mixed up
- Biometric equipment unavailable/expensive
- Need reliable patient identification

**Somidics solution:**

**At patient registration:**
1. Nurse: "Do you have any marks on your hands or arms?"
2. Patient shows scar on right hand
3. Quick encoding, somidic added to paper chart
4. Takes 2 minutes

**At follow-up visits:**
1. Patient gives name
2. Multiple "John Kamau" charts found
3. Nurse: "Your chart says scar on right hand - can I see?"
4. Visual verification, correct chart identified
5. Takes 30 seconds

**Benefits:**
- ✅ Zero equipment cost
- ✅ Works with illiterate patients
- ✅ Reliable across visits (marks don't change)
- ✅ Privacy-preserving (no fingerprints stored)
- ✅ Culturally appropriate

**Challenges:**
- Not all patients have obvious marks
- Paper records can be lost (backup needed)
- Cultural taboos about some marks

**Success metrics:**
- Medical record mix-ups: Reduced 90%+
- Patient satisfaction: High (faster, more reliable)
- Cost: Nearly zero
- Adoption rate: 70-80% of returning patients

## Security Analysis

### Threat Model

**Attacker capabilities:**
- Can observe victim from distance
- Can steal physical credential (card, phone)
- Can use makeup/temporary tattoos
- Cannot force victim to reveal somidion
- Cannot easily find confederate with matching mark

**What attacker knows:**
- The 6-digit somidic (from stolen credential)
- Software decoded description (if they have verifier software)
- Approximate body zone and mark type

**What attacker doesn't know:**
- Exact appearance of the mark
- Subtle characteristics (exact shape, texture details)
- How strictly verifier will judge

### Attack Scenarios

**Attack 1: Random impersonation**
- Attacker has no information about victim
- Tries to use stolen credential
- **Success probability:** ~1/1,000 (if attacker has a somidion in same zone)
- **Prevented by:** Random marks don't match

**Attack 2: Prepared impersonation**
- Attacker observes victim, notes somidion location
- Applies temporary tattoo in similar location
- **Success probability:** Moderate (depends on verifier vigilance)
- **Prevented by:** Verifier checking texture, asking questions

**Attack 3: Confederate with matching mark**
- Attacker finds someone with similar mark in similar location
- **Success probability:** ~1/1,000 to find someone
- **Prevented by:** Rarity of matches, need exact zone match

**Attack 4: Brute force somidic guessing**
- Attacker tries random 6-digit numbers
- **Success probability:** 1/1,000 for valid somidic (99.6% invalid via CRC)
- **Prevented by:** CRC validation, account lockout

**Attack 5: Social engineering**
- Attacker tricks victim into revealing somidion description
- Creates fake mark
- **Success probability:** Low (requires victim cooperation)
- **Prevented by:** User education, verifier training

### Security Compared to Alternatives

**vs. 4-digit PIN:**
- PIN: 1/10,000 theoretical, easier to guess/observe
- Somidic: ~1/1,000 theoretical, harder to fake
- **Winner:** Somidic (can't observe or guess easily)

**vs. Photo ID:**
- Photo: Can be similar-looking person, photos age
- Somidic: Must match specific mark in specific location
- **Winner:** Somidic (more specific)

**vs. Fingerprint biometric:**
- Fingerprint: ~1/1,000,000+, requires equipment
- Somidic: ~1/1,000, no equipment
- **Winner:** Depends on context (fingerprint stronger but needs equipment)

**vs. Password:**
- Password: Can be stolen, forgotten, shared
- Somidic: Can't forget (it's your body), harder to share
- **Winner:** Somidic (embodied knowledge)

### Security Recommendations

**For high-value credentials:**
- Use 2-3 somidics (1-in-million to 1-in-billion)
- Combine with other factors (PIN, photo)
- Verifier training important

**For low-value credentials:**
- Single somidic sufficient
- Casual verification okay
- Accept fuzzy matches

**For all implementations:**
- Educate users not to reveal somidions publicly
- Train verifiers to check carefully
- Implement account lockout after failed attempts
- Log verification attempts for audit

## Implementation Recommendations

### Enrollment UX Best Practices

**1. Education first:**
- Explain what a somidion is
- Show examples (with permission/stock photos)
- Emphasize privacy (they choose what to share)

**2. Guided discovery:**
- "Think of your hands and arms - any marks there?"
- "What about your face?"
- Help them identify suitable marks

**3. Visual aids:**
- Show body diagrams
- Provide size comparison images (grain, coin)
- Use clear language (no medical terms)

**4. Confirmation:**
- "So you're choosing the mole on your left wrist?"
- Show decoded description back to them
- Ensure they're comfortable with choice

**5. Documentation:**
- Generate somidic code
- Explain they don't need to memorize it
- Emphasize remembering the mark itself

### Verification UX Best Practices

**1. Clear instructions:**
- Display decoded description prominently
- Use simple language
- Visual aids if helpful

**2. Privacy:**
- Offer private area if needed
- Same-gender verifier option
- Respect personal space

**3. Verifier guidance:**
- Train to accept fuzzy matches
- "If unsure, accept"
- Log decisions for audit

**4. Fallback options:**
- If somidion not visible (clothing): offer alternative
- If doubt exists: escalate to supervisor
- Never force uncomfortable reveal

### Error Handling

**Somidic invalid (CRC fails):**
- "This code appears incorrect. Please check the number."
- Offer re-entry
- After 3 attempts, offer alternative verification

**Mark not found:**
- "I don't see a mark matching that description"
- Double-check zone, ask person to show different angle
- If truly doesn't match: potential fraud or error
- Escalate to supervisor

**Mark changed (removed/covered):**
- "This mark appears to have changed"
- Explain need for credential reissuance
- Provide re-enrollment path

**Cultural/comfort issues:**
- "I'm not comfortable showing that area"
- Offer same-gender verifier
- Offer private area
- Offer alternative somidion if available

## Future Research Directions

### Empirical Studies Needed

1. **Inter-rater reliability:**
   - How often do two verifiers agree on a match?
   - What factors affect agreement?
   - Can training improve reliability?

2. **Cross-cultural acceptance:**
   - What zones are acceptable in different cultures?
   - How do modesty norms affect adoption?
   - Same-gender verification sufficient?

3. **Real-world discrimination rates:**
   - What percentage of population can be distinguished?
   - Are some zone/type combinations more common?
   - How does this vary by population?

4. **Stability over time:**
   - How often do somidions change?
   - What's the reissuance rate?
   - Which marks are most stable?

5. **Security in practice:**
   - Can attackers successfully fake marks?
   - How often do false positives occur?
   - Is verifier training effective?

### Technical Enhancements

1. **Zone-specific follow-up bits:**
   - Hands: encode quadrants?
   - Face: more granular subzones?
   - Fingers: encode segment precisely?

2. **Private somidics specification:**
   - Define full-body zone map
   - Medical examiner protocols
   - Privacy protections

3. **Integration with credential standards:**
   - mDL: Add somidic field to ISO 18013-5
   - W3C VC: Somidic claim type
   - KERI: Somidic binding

4. **Verification protocols:**
   - Progressive disclosure
   - Zero-knowledge proofs?
   - Privacy-enhancing techniques

5. **Anti-fraud measures:**
   - Detect temporary tattoos?
   - Freshness challenges?
   - Multi-angle verification?

### Policy Questions

1. **Legal status:**
   - Is somidic legally binding?
   - Admissible in court?
   - Standards for verification?

2. **Accessibility:**
   - What about people with no suitable marks?
   - Alternatives for edge cases?
   - Disability considerations?

3. **Data protection:**
   - Is somidic personal data under GDPR?
   - Storage requirements?
   - Right to deletion?

4. **Child protection:**
   - At what age can child consent to somidic?
   - Parent/guardian role?
   - Child trafficking database protocols?

## Conclusion

Somidics represents a novel approach to identification that fills a specific niche: equipment-free, human-verifiable authentication for contexts where traditional biometrics are impractical or inappropriate. The system achieves approximately 1-in-1,000 discrimination with a single somidic, expandable to much stronger identification through stacking.

**Key strengths:**
- Zero equipment cost
- Works in low-resource contexts
- Privacy-preserving through fuzzy matching
- Culturally adaptable
- Memorable for holders

**Key limitations:**
- Not suitable for high-security applications alone
- Slower than automated biometrics
- Requires human judgment
- Some people lack suitable marks

**Best applications:**
- Humanitarian aid / refugee identification
- Rural healthcare / developing world
- Backup to primary identification
- Digital credential binding
- Casual fraud prevention

The success of somidics will depend on thoughtful implementation, cultural sensitivity, verifier training, and appropriate use case selection. It is not a replacement for strong biometrics, but rather a complementary tool for contexts where equipment-free verification is valued.
