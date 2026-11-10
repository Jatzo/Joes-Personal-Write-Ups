RESEARCH · 25 SEPTEMBER 2026

# Most North East firms publish nothing to stop an email sent in their name.

> Published on [jatzo.co.uk](https://jatzo.co.uk/research/north-east-email-spoofing-2026) on 25 September 2026. This copy was taken on 10 November 2026. The live page is the current version, and if the two ever differ, the live page is right.

We audited the public DNS records of 192 businesses across the North East on one day. 152 of them, 79%, publish no rule that tells a receiving mail server to reject a message forged in their name. Only 16 are properly protected.

## What was measured

Every domain publishes a rule, in public DNS, telling other mail servers what to do with a message that claims to come from it. That rule is DMARC. Anyone can read it, and every receiving mail server does, before deciding what to do with a message.

Where there is no effective rule, a receiving server is given no instruction to reject a forgery. An email sent in that firm's name is likely to reach its clients looking exactly like the firm.

## Method, in full

- Businesses were identified from the Companies House register by SIC code and registered office, restricted to the NE, SR, DH, DL and TS postcode areas.

- Each domain was confirmed to belong to the business named, by finding that business's town, registered postcode or company registration number on the site the domain serves. Any domain that could not be confirmed was excluded, whatever its records said. That is why the sample is 192 and not the several hundred it started from.

- Public DNS records only. Nothing belonging to any business was accessed or probed, and no mail was sent.

- A further 15 domains published no mail server at all, so they are not the address those firms run on. They are outside every figure below rather than counted as unprotected.

- No business is named. The findings are aggregate only.

## Protection

_All 192 businesses checked, 25 September 2026._

| Protection | Businesses | Share |
| --- | --- | --- |
| Exposed, no effective policy | 152 | 79% |
| Partial, forged mail sent to junk | 24 | 12% |
| Protected, forged mail rejected | 16 | 8% |

## By sector

Sectors with fewer than 20 businesses checked are counted in the total above but not broken out, because a percentage of a handful is not a finding.

_Businesses checked and businesses exposed, by sector._

| Sector | Checked | Exposed | Share |
| --- | --- | --- | --- |
| Property | 84 | 73 | 87% |
| Legal | 48 | 31 | 65% |
| Accountancy | 44 | 35 | 80% |

## By town

Towns with fewer than 10 businesses checked are not broken out, for the same reason.

_Businesses checked and businesses exposed, by town._

| Town | Checked | Exposed | Share |
| --- | --- | --- | --- |
| Newcastle upon Tyne | 47 | 36 | 77% |
| Gateshead | 20 | 16 | 80% |
| Sunderland | 17 | 15 | 88% |
| Middlesbrough | 12 | 11 | 92% |
| Durham | 10 | 6 | 60% |

## SPF

SPF lists which servers may send on a domain's behalf. On its own it does not tell a receiver what to do when a message fails, which is what DMARC adds.

- 8 of 192, 4%, publish no SPF record at all.

- 69 of 192, 36%, publish an SPF record that asks receiving servers to reject nothing.

## What it means

These are firms that move client money on emailed instructions. A forged email reaches the client, the client pays, and it clears within hours. The firm hears about it by telephone, afterwards.

Nothing touched the firm's own systems, so there is nothing to find on them afterwards. The message was never theirs to see.

## This is not a criticism

None of this says any business has been compromised. These records are invisible in day to day use, no software warns anyone they are missing, and most small firms have never been told they exist.

## The fix

Publishing a DMARC policy is one line added to a domain's DNS records by whoever looks after them. It costs nothing. It is not our product, and we do not sell it.

## Who produced this

Jatzo is a cyber security company in the North East. We make an email threat analyser that tells somebody whether a message in front of them is trying to steal from them. It does not change anybody's DNS records, and it is not what fixes the findings above.
