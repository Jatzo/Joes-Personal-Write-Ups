RESEARCH · REFERENCE

# Where our detection rules sit in MITRE ATT&CK.

> Published on [jatzo.co.uk](https://jatzo.co.uk/research/mitre-attack-mapping) as a maintained reference, checked against attack.mitre.org on 1 November 2026. This copy was taken on 10 November 2026. The live page is the current version, and if the two ever differ, the live page is right.

Every one of Jatzo's 172 detection rules has been read against what it actually reports and placed in [MITRE ATT&CK](https://attack.mitre.org/) Enterprise v20.0. 140 carry at least one technique. 32 deliberately carry none, and say why.

Checked against attack.mitre.org on 1 November 2026.

## What this does not mean

This section is at the top rather than the bottom, because a mapping is the easiest thing in security to overstate.

Jatzo reads an email at the moment it is delivered. It sees the approach and the lure: who the message claims to be, what it is asking for, what it carries and where it points. It does not see what happens on the device afterwards, and it does not stop anything. A mapping says where a tool can find evidence of a technique. It does not say that the technique is detected every time, and it does not say that it is prevented.

That matters most where this page reaches the later tactics. Where a rule sits under Execution, Credential Access or Impact, what Jatzo found is the message asking somebody to do the thing, not the thing happening. We have never seen a theft. We have seen the email that would have caused one.

## What was mapped, and how

Each rule was mapped from what it reports when it fires, not from its name. A rule called "authentication checks passed" records a pass and is evidence of nothing, and two rules with similar names are often the strong and weak halves of one check.

Every technique identifier, name and tactic was read from MITRE's own pages rather than recalled. That was not a formality. ATT&CK v20.0 had renamed a tactic and revoked two techniques this mapping would otherwise have used, one of which turned out to be its most common row.

Each rule is placed at one of three strengths:

- **Direct.** The signal is evidence of that technique.

- **Partial.** The signal is evidence of part of that technique.

- **Context.** The signal supports a judgement about that technique without being evidence of it.

Most rules are not direct, and that is the honest result rather than a disappointing one. A single piece of evidence rarely proves a technique on its own, which is why a score is built from several.

## The numbers

_Jatzo's detection registry against ATT&CK Enterprise v20.0, 1 November 2026._

| Measure | Count |
| --- | --- |
| Detection rules in the registry | 172 |
| Rules carrying at least one technique | 140 |
| Rules deliberately mapped to nothing | 32 |
| Distinct techniques and sub-techniques reached | 30 |
| Tactics reached | 9 |
| Rule to technique placements | 195: 65 direct, 69 partial, 61 context |

## Where they land

Techniques reached, by ATT&CK tactic, in the order ATT&CK gives them. The concentration is where an email analyser would expect it to be, and the thin tactics later on are the lures described above rather than the acts.

_Distinct techniques reached in each tactic._

| Tactic | Techniques |
| --- | --- |
| Reconnaissance | 1 |
| Resource Development | 3 |
| Initial Access | 5 |
| Execution | 4 |
| Stealth | 11 |
| Credential Access | 3 |
| Lateral Movement | 1 |
| Command and Control | 1 |
| Impact | 1 |

## Eight of them, in full

Chosen for what they show about the judgement rather than for coverage: the ones where the fit was arguable, the one where ATT&CK has no technique for what the rule detects, and the one where a rule sees an attacker's preparation rather than the act. We do not publish which rules give evidence of what. That list is a design guide for getting past them.

### [T1684.002 Social Engineering: Email Spoofing](https://attack.mitre.org/techniques/T1684/002/)

Stealth

**What Jatzo sees.** Whether the sender identity survives the checks the domain owner published. A DMARC failure against a policy asking receivers to reject is the owner's own instruction being ignored, and that is the clearest evidence of a forged sender an email can carry.

**The judgement.** SPF and DKIM failures are mapped as partial rather than direct. Each is one mechanism of the two DMARC weighs, and forwarding and mailing lists break them on legitimate mail every day. A result of none is weaker still and is recorded as context, because nothing was tested.

### [T1684.001 Social Engineering: Impersonation](https://attack.mitre.org/techniques/T1684/001/)

Stealth

**What Jatzo sees.** Who the message claims to be against who sent it: the display name, the domain it was sent from, and whether a brand it invokes is one that domain may claim. This is the largest group in the mapping.

**The judgement.** These were the rows that had to be checked rather than remembered. ATT&CK revoked T1656 Impersonation in April 2026 and reissued it here, under a new Social Engineering parent. A mapping written from memory would have been wrong in its most common row.

### [T1566.002 Phishing: Spearphishing Link](https://attack.mitre.org/techniques/T1566/002/)

Initial Access

**What Jatzo sees.** A message asking the reader to act, and a destination that does not belong to whoever appears to be asking. The strongest form is a layout copied from a service, where every link goes to that service except the one button the message wants pressed.

**The judgement.** QR codes land here too, and that is a compromise. ATT&CK has no QR technique, so a destination decoded from an image is recorded as the link delivery it is, with the reason saying why. Inventing a fit would have been the easier and worse answer.

### [T1204.004 User Execution: Malicious Copy and Paste](https://attack.mitre.org/techniques/T1204/004/)

Execution

**What Jatzo sees.** A message that tells the reader to open a command window and paste something in, usually behind a story about fixing an error or proving they are human.

**The judgement.** A direct fit, and a recent one: ATT&CK only added this sub-technique as the campaigns using it became common. It is the clearest case of an attack that needs no attachment and no link, which is exactly why it works against tools that only inspect those two things.

### [T1027.006 Obfuscated Files or Information: HTML Smuggling](https://attack.mitre.org/techniques/T1027/006/)

Stealth

**What Jatzo sees.** An HTML attachment carrying the markers of browser-side file reconstruction alongside script, which is a payload assembled after the file is opened rather than one carried through the mail system.

**The judgement.** Direct, because the rule detects the mechanism ATT&CK describes rather than a symptom of it. Most attachment rules are weaker than this: a macro-enabled format is the container, so it is mapped as partial, while macro content found inside it is what actually runs.

### [T1528 Steal Application Access Token](https://attack.mitre.org/techniques/T1528/)

Credential Access

**What Jatzo sees.** A message asking the reader to approve an application or type a device code into a real sign-in page. Both hand over access without a password being entered anywhere.

**The judgement.** Direct, and worth saying plainly: the sign-in page in these attacks is genuine. There is no lookalike domain and no fake form to find, so a check that only looks for those sees nothing wrong at all.

### [T1583.001 Acquire Infrastructure: Domains](https://attack.mitre.org/techniques/T1583/001/)

Resource Development

**What Jatzo sees.** A sending domain that reads like one this account already deals with, or that spells out a brand it does not belong to.

**The judgement.** The technique is something the attacker did before the message was sent, and Jatzo only ever sees the consequence. Registering the domain is the act; a message arriving from it is evidence that it happened. Where the domain is a near miss of a real correspondent that is strong enough to call direct, and where it is only an odd-looking domain it is context.

### [T1657 Financial Theft](https://attack.mitre.org/techniques/T1657/)

Impact

**What Jatzo sees.** A request that moves money or changes where money goes: a beneficiary change, a payroll update, gift card codes, or a demand backed by a threat.

**The judgement.** The honest limit of the whole mapping. Jatzo has never seen a theft. It sees the message that would cause one, so almost every row here is partial, and the few marked direct are the ones where the request itself is the act.

## What we do not see

A page that lists only what a tool finds is read as a claim about everything it did not list. These are techniques an email analyser is asked about and cannot answer, and the reason each one is out of reach.

### [T1114.003 Email Collection: Email Forwarding Rule](https://attack.mitre.org/techniques/T1114/003/)

Collection

A rule inside the mailbox, set after somebody is already in, which quietly copies mail to an outside address. It survives a password change, and nothing about any single message reveals it. A tool reading one email at delivery is looking in the wrong place entirely.

### [T1539 Steal Web Session Cookie](https://attack.mitre.org/techniques/T1539/)

Credential Access

The sign-in page an adversary-in-the-middle serves is a real sign-in page, relayed. From the email it looks like any other link, and the theft happens in the browser afterwards. We can find the lure and we cannot tell this lure from one that only wants a password.

### [T1111 Multi-Factor Authentication Interception](https://attack.mitre.org/techniques/T1111/)

Credential Access

Where a code is taken rather than asked for. We detect a message asking somebody to hand a code over, because that is written in the email. Interception is not.

They share one shape. Each happens after delivery, on a device or inside a mailbox, and the message that started it carries no trace of what followed. A tool that reads the email at the door can describe who knocked and what they asked for. It cannot tell you what happened in the hall.

## The rules that map to nothing

32 of 172 carry no technique at all. Each says why in the file, and leaving them out would have been the easier way to make the coverage look wider.

_Rules carrying no technique, by the reason they carry none._

| Reason | Rules |
| --- | --- |
| Nuisance channel: unwanted or promotional mail, which is not an attack technique. | 19 |
| Records a check that passed or a normal-looking property, so it is evidence of absence rather than of a technique. | 7 |
| Uncertainty channel: records what could not be checked, so it is evidence of nothing in either direction. | 5 |
| Records a decision somebody in the customer's own team made, not anything the sender did. | 1 |

The first of those is a deliberate line through the middle of the product. Unwanted and promotional mail is scored separately from attack evidence, and it can never on its own make Jatzo call something an attack. Mapping a promotional rule to a technique would say the opposite in public.

## How this is kept from going stale

A mapping is right on the day it is written and quietly wrong six months later, because a rule is added or retired and nothing makes anybody open the other file.

The mapping lives beside the detection registry in the same repository, and a test fails the build if a rule has no entry, if an entry names a rule that no longer exists, or if an entry has lost its reasoning. A rule cannot be added to Jatzo without a decision being recorded about where it sits here.

The ATT&CK release this was checked against is recorded in the file, because technique names and tactics move between releases. When MITRE publishes a release that moves something used here, this page changes with it.

## Who produced this

Jatzo is a cyber security company in the North East. We make an email threat analyser that tells somebody whether a message in front of them is trying to steal from them, and explains what it found. MITRE ATT&CK is published by [The MITRE Corporation](https://www.mitre.org/) and we have no connection to them.
