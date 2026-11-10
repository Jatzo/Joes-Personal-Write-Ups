THREAT REPORT · TLP:CLEAR

# ClickFix: getting the victim to run the command

> Published on [jatzo.co.uk](https://jatzo.co.uk/research/clickfix) on 29 September 2026. This copy was taken on 10 November 2026. The live page is the current version, and if the two ever differ, the live page is right.

A technique that skips the attachment and the malicious link by getting the reader to paste the command themselves, and what an email analyser can see of it.

Published 29 September 2026. Every claim below carries its source.

## Bottom line up front

ClickFix persuades the person to run the attacker's command themselves, by dressing it up as a verification step or a fix for an error they have just been shown. It removes the two things most email security is built to inspect, the attachment and the link to a malicious page, which is why a message using it can read as completely clean while carrying the whole attack in its instructions.

## What it is

A ClickFix lure tells the reader that something needs verifying or repairing: prove you are human, fix a display error, repair a broken document. The instructions look like troubleshooting. Press the Windows key and R, paste what we have copied for you, press Enter.

What has been copied for them is a command. The victim runs it on their own machine, with their own permissions, having been walked through it a step at a time. Nothing was attached to the message and nothing had to survive a link check, because the payload arrives afterwards, fetched by the command the victim has just run.

It was first reported prominently in early 2024 and named by [Proofpoint](https://www.proofpoint.com/us/blog/threat-insight/security-brief-clickfix-social-engineering-technique-floods-threat-landscape) in November that year. By August 2025 [Microsoft Threat Intelligence](https://www.microsoft.com/en-us/security/blog/2025/08/21/think-before-you-clickfix-analyzing-the-clickfix-social-engineering-technique/) described campaigns using it reaching thousands of devices globally every day.

## How it works, step by step

- **The lure arrives.** An email, or a compromised website, or an advert. Email is the path an email analyser can see, and it is not the only one.

- **A page presents a problem.** A fake CAPTCHA, a browser error, a document that will not open. Fake CAPTCHA variants have been the common shape since August 2024 ([Proofpoint](https://www.proofpoint.com/us/blog/threat-insight/security-brief-clickfix-social-engineering-technique-floods-threat-landscape)).

- **The page copies a command to the clipboard**, usually without the reader noticing, while the visible text explains which keys to press.

- **The victim pastes it.** Into the Run dialog, or a terminal, or PowerShell. This is the step the whole technique exists to produce, and the one no scanner is standing in front of.

- **The command fetches and runs the payload.** Microsoft records interpreters being driven this way with encoded and concatenated command lines, and with the download and execution stitched together in one line ([Microsoft](https://www.microsoft.com/en-us/security/blog/2025/08/21/think-before-you-clickfix-analyzing-the-clickfix-social-engineering-technique/)).

## Who uses it

Only what the sources state.

- **TA571 and the ClearFake activity cluster** were the first prominently observed using it, and Proofpoint records campaigns delivering AsyncRAT, DanaBot, DarkGate, Lumma Stealer, NetSupport, Brute Ratel C4, Latrodectus and XWorm ([Proofpoint, 18 November 2024](https://www.proofpoint.com/us/blog/threat-insight/security-brief-clickfix-social-engineering-technique-floods-threat-landscape)).

- **Multiple unattributed clusters** have since adopted it, including one targeting transport and logistics firms ([Proofpoint](https://www.proofpoint.com/us/blog/threat-insight/security-brief-clickfix-social-engineering-technique-floods-threat-landscape)).

- **Microsoft Defender Experts** report it growing in popularity across enterprise and consumer devices ([Microsoft, 21 August 2025](https://www.microsoft.com/en-us/security/blog/2025/08/21/think-before-you-clickfix-analyzing-the-clickfix-social-engineering-technique/)).

We name no actor the sources do not name, and we have not attributed anything ourselves.

## Indicators

**No network addresses appear in this report.** The published research names infrastructure that has long since moved, and a list of dead domains is worse than none: it reads as coverage. What follows is behavioural, which is what survives the infrastructure changing.

### In the email

What an analyser can see before anybody clicks anything.

- An instruction to **paste, run or execute** sitting close to **PowerShell, a command, a terminal, a script or the Run dialog**.

- That instruction paired with a **verification or repair story**: a CAPTCHA, proving you are human, a browser error, a document that needs fixing.

- The two together are the signal. Either alone is ordinary: support desks send command lines, and websites ask people to prove they are human all day.

### On the host

From [Microsoft](https://www.microsoft.com/en-us/security/blog/2025/08/21/think-before-you-clickfix-analyzing-the-clickfix-social-engineering-technique/) and [Sekoia](https://www.sekoia.com/blog/clickfix-tactic-revenge-of-detection).

- **The `RunMRU` registry key**, which keeps a history of what has been typed into the Run dialog. Microsoft names it as the artefact that records this activity when little else does.

- **A script interpreter whose parent is `explorer.exe`**, because a command pasted into the Run dialog is launched by the desktop. Sekoia makes the same point: using Windows and R causes the command to run under `explorer.exe`.

- **`powershell.exe` or `bitsadmin.exe` whose parent is `mshta.exe`**, which Sekoia gives for one variant.

- **Command lines carrying** `iwr`, `irm`, `iex`, `FromBase64String`, string concatenation, or the `^` escape character, and stacked living-off-the-land binaries such as `mshta`, `rundll32`, `wscript`, `curl` and `wget` (Microsoft).

### In the network

- A **PowerShell user agent** making an outbound request, correlated with a file download in a short window (Sekoia).

- PowerShell connecting out, writing to a temporary folder under the user's profile, and something executing from that folder inside about five minutes (Sekoia).

## ATT&CK mapping

MITRE ATT&CK Enterprise v19.2. The split matters more than the list: the first three are visible in the message, and the rest happen after the victim has pasted.

| Technique | Tactic | Seen at delivery |
| --- | --- | --- |
| [T1566.002 Phishing: Spearphishing Link](https://attack.mitre.org/techniques/T1566/002/) | Initial Access | Yes, where the lure is an email pointing at the page |
| [T1204.004 User Execution: Malicious Copy and Paste](https://attack.mitre.org/techniques/T1204/004/) | Execution | Yes, as an instruction. Not as an execution |
| [T1027 Obfuscated Files or Information](https://attack.mitre.org/techniques/T1027/) | Stealth | Partly, where the message itself is obfuscated |
| [T1059.001 Command and Scripting Interpreter: PowerShell](https://attack.mitre.org/techniques/T1059/001/) | Execution | No |
| [T1218.005 System Binary Proxy Execution: Mshta](https://attack.mitre.org/techniques/T1218/005/) | Stealth | No |
| [T1105 Ingress Tool Transfer](https://attack.mitre.org/techniques/T1105/) | Command and Control | No |

## What Jatzo raises, and why

Three findings can fire on a message of this shape. Named as the customer sees them, because that is what a customer sees.

**Verification asks you to run a command.** The strongest of the three. It needs an instruction to paste, run or execute within a short distance of PowerShell, a command, a terminal, a script or the Run dialog, _and_ a verification or repair story somewhere in the message. It also checks that the instruction is not negated: a sentence beginning "never paste commands you are sent" is advice, not an instruction, and does not score.

**Command-paste or ClickFix-style instruction.** A broader net, matching four shapes: pressing the Windows key or Win and R and then pasting, typing or running; opening the Run dialog, PowerShell, a command prompt or a terminal and then pasting or copying; copy and paste near PowerShell, cmd, a command, a terminal or Run; and proving you are human followed by running, pasting or a command.

**CAPTCHA-gated phishing lure.** The weakest, and deliberately so. A CAPTCHA or human verification step standing between the reader and something they want is ordinary on the web, so on its own it is supporting evidence.

## Mitigation

**For a small business.**

- Nobody legitimate will ever ask you to press Windows and R and paste something. That single sentence is worth more than any tool here, and it is the one to put in front of staff.

- Consider whether the Run dialog is needed at all on the machines your staff use. Removing it costs most people nothing.

- If somebody has already pasted a command, treat the machine as compromised rather than cleaning it. The command ran with their permissions and fetched something you have not seen.

**For a SOC.**

- Hunt on the two queries at the end of this report rather than alerting on them. Both have legitimate results.

- Turn on PowerShell script block logging, which is what makes an obfuscated command readable after the fact ([Microsoft](https://www.microsoft.com/en-us/security/blog/2025/08/21/think-before-you-clickfix-analyzing-the-clickfix-social-engineering-technique/)).

- Treat `explorer.exe` as a parent process worth watching. It is a short list of children that should ever be normal.

- Network protection that blocks the payload fetch stops the chain even after the paste (Microsoft).

## Confidence

Using the [PHIA probability yardstick](https://www.gov.uk/government/publications/explaining-uncertainty-in-uk-intelligence-assessment/explaining-uncertainty-in-uk-intelligence-assessment), so that "likely" means what an assessment means by it.

- **It is highly likely that ClickFix lures will continue to appear in email through 2026.** Three independent vendors report growth rather than decline across two years, and the technique's advantage, avoiding the attachment and the link, is structural rather than a passing trick.

- **It is likely that a ClickFix email reaches the inbox rather than being filtered**, because a message with no attachment and no malicious link offers a filter very little to judge. This rests on the technique's design rather than on any measurement of our own.

- **It is a realistic possibility that the first sign of a successful attack is the payload's behaviour rather than the lure**, given that the lure may never have been an email at all.

## Limits

**Jatzo reads the email at delivery, so everything after the paste is invisible to it.** The interpreter running, the download, whatever is installed: none of that is in a message, and a mapping that claimed otherwise would be describing a product that does not exist.

**Where the lure is not an email, Jatzo never sees it.** ClickFix arrives through compromised websites and advertising as well as through mail. Those paths have no message for an analyser to read, and no email product covers them.

**Seeing the instruction is not stopping the attack.** A finding on a message is evidence for the person reading it. If they paste the command anyway, nothing in an email analyser is standing between them and the consequence.

## Hunting queries

Written for Microsoft Defender advanced hunting, where these table and column names come from. They are examples to adapt rather than finished detections: an estate streaming different data calls the same things by different names, and both of these return legitimate results by design.

### Commands typed into the Run dialog

The Run dialog keeps a history in the registry, which is the one place a pasted command leaves a record of its own. Entries naming a script interpreter are worth reading whatever else is true.

 `DeviceRegistryEvents | where RegistryKey has @"\Explorer\RunMRU" | where RegistryValueData has_any ("powershell", "mshta", "rundll32", "wscript", "curl", "wget", "iwr", "irm", "iex", "FromBase64String") | project Timestamp, DeviceName, InitiatingProcessAccountName, RegistryValueName, RegistryValueData | order by Timestamp desc`

Expect legitimate results. Administrators type these too, so this is a hunting query rather than an alert, and what makes a row interesting is the account it belongs to.

### A script interpreter started by the desktop

A command pasted into the Run dialog runs as a child of explorer.exe, because the desktop is what launched it. Ordinary software does not usually start PowerShell that way.

 `DeviceProcessEvents | where InitiatingProcessFileName =~ "explorer.exe" | where FileName in~ ("powershell.exe", "pwsh.exe", "mshta.exe", "rundll32.exe", "wscript.exe", "cmd.exe") | where ProcessCommandLine has_any ("-enc", "-EncodedCommand", "FromBase64String", "DownloadString", "Invoke-Expression", "iex", "iwr", "irm", "-w hidden", "-nop") | project Timestamp, DeviceName, AccountName, FileName, ProcessCommandLine | order by Timestamp desc`

The parent process is the signal, not the interpreter. Narrow it by tightening the command-line list rather than by removing the parent test, which is the part that carries the meaning.

## Sources

Published research only. Nothing in this report came from a sample, a corpus or an indicator feed, and no address named in any of it was visited.

- [Security brief: ClickFix social engineering technique floods the threat landscape](https://www.proofpoint.com/us/blog/threat-insight/security-brief-clickfix-social-engineering-technique-floods-threat-landscape). Proofpoint Threat Research, 18 November 2024.

- [ClickFix tactic: the revenge of detection](https://www.sekoia.com/blog/clickfix-tactic-revenge-of-detection). Sekoia Threat Detection & Research, 5 November 2024.

- [Think before you Click(Fix): analyzing the ClickFix social engineering technique](https://www.microsoft.com/en-us/security/blog/2025/08/21/think-before-you-clickfix-analyzing-the-clickfix-social-engineering-technique/). Microsoft Threat Intelligence and Microsoft Defender Experts, 21 August 2025.

- [ATT&CK Enterprise v19.2, techniques cited in the mapping below](https://attack.mitre.org/). MITRE, 6 August 2026.

- [Advanced hunting schema: DeviceProcessEvents and DeviceRegistryEvents](https://learn.microsoft.com/en-us/defender-xdr/advanced-hunting-schema-tables). Microsoft Learn, 7 August 2026.

Confidence language follows the [PHIA probability yardstick](https://www.gov.uk/government/publications/explaining-uncertainty-in-uk-intelligence-assessment/explaining-uncertainty-in-uk-intelligence-assessment).

## Who produced this

Jatzo is a cyber security company in the North East. We make an email threat analyser, which is why these reports are written from the message outwards and say plainly where that view ends.
