<h1>
  <picture>
    <source media="(prefers-reduced-motion: reduce)" srcset="images/banner.svg">
    <img src="images/banner.gif" alt="Personal write-ups" width="100%">
  </picture>
</h1>

Write-ups of security work I have done: what I measured, what was wrong, what I changed, and what I would not trust.
Each one stands on its own. Start anywhere.

## Contents

### Research

First published on [jatzo.co.uk](https://jatzo.co.uk/research), oldest first. Each is copied here as published and
links to its original, which is the current version. The charts are mine, drawn from the page itself.

**[Email spoofing protection across North East businesses](research/north-east-email-spoofing.md)** ([original](https://jatzo.co.uk/research/north-east-email-spoofing-2026))<br>
I checked the public DNS records of North East businesses to see how many publish a rule that would stop forged email
in their name.

**[Jatzo's detection rules, mapped to MITRE ATT&CK](research/mitre-attack-mapping.md)** ([original](https://jatzo.co.uk/research/mitre-attack-mapping))<br>
Each of Jatzo's detection rules placed in MITRE ATT&CK Enterprise, with the reasons some map to nothing.

**[ClickFix: getting the victim to run the command](research/clickfix.md)** ([original](https://jatzo.co.uk/research/clickfix))<br>
An attack that gets the reader to paste and run a command themselves, and what an email analyser can see of it.

**[Device code phishing: the sign-in page is real](research/device-code-phishing.md)** ([original](https://jatzo.co.uk/research/device-code-phishing))<br>
An account takeover with no spoofing, where the victim completes multi-factor sign-in on a real page, and what email
analysis can and cannot catch.

### Measuring detection quality

**[01. Cutting false alarms in a phishing analyser, and what the numbers did not show](01-cutting-false-alarms.md)**<br>
A paired before-and-after evaluation of one tuning round on [Jatzo](https://jatzo.co.uk), my phishing analyser, using
my own mailbox and a public phishing corpus with a held-out range. I triaged the worst false alarms by hand, and the
post reports the detection losses beside the gains.

### Reasoning about hostile input

**[02. Spotting prompt injection hidden in an email](02-prompt-injection-in-email.md)**<br>
How I designed a rule for instructions hidden in a message and aimed at an email assistant, how I tested what must not
fire, and the false alarm I found after it shipped.

**[03. Reading hostile email safely, and what a security audit found in my own sandbox](03-isolating-hostile-email.md)**<br>
How untrusted email is parsed in a disposable container, and six findings from an audit of that boundary, including one
that contradicted my own design and one that was not a bug.

### Owning failures

**[04. Postmortem: a safety stop that needed a person to switch it off](04-postmortem-late-container-removal.md)**<br>
A blameless postmortem of a failure-handling gap: timeline, causes, what went well, the fix, its tests and the action
items.

## How to read them

The numbers live in the write-ups and research pages, not on this page, so there is one place to check each of them.
Each numbered write-up ends with an Evidence table that says what every claim rests on, what is shown and
what is left out.

## What is private, and why

The engine, its evaluation reports, audits and commit history are private, because the product is live. In the
numbered write-ups I leave out sample content, sender domains, rule wording, thresholds, size limits and timings, and
any finding that is still open. The research pages are copied word for word as they were published on jatzo.co.uk,
including what they say about how detection works. The charts on them are my addition, and show nothing the page does
not say. Everything public is linked.

## Corrections

If I find a mistake, I fix it in a commit and say in the post what changed. The history of this repository is the
record of every edit.
