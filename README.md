<h1>
  <picture>
    <source media="(prefers-reduced-motion: reduce)" srcset="images/banner.svg">
    <img src="images/banner.gif" alt="Personal write-ups" width="100%">
  </picture>
</h1>

Write-ups of security work I have done: what I measured, what was wrong, what I changed, and what I would not trust.
Each one stands on its own. Start anywhere.

## Contents

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

The numbers live in the posts, not on this page, so there is one place to check each of them. Each post ends with an
Evidence table that says what every claim rests on, what is shown and what is left out.

## What is private, and why

The engine, its evaluation reports, audits and commit history are private, because the product is live. I leave out
sample content, sender domains, rule wording, thresholds, size limits and timings, and any finding that is still open.
Everything public is linked.

## Corrections

If I find a mistake, I fix it in a commit and say in the post what changed. The history of this repository is the
record of every edit.
