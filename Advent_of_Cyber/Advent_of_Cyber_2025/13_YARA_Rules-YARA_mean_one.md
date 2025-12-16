## Introduction 

---

#### The Story

When McSkidy went missing, there was chaos and uncertainty at The Best Festival Company (TBFC). However, even in her disappearance, McSkidy was trying to help the TBFC blue team. Taking a page out of the crisis communication process, McSkidy sent what looks like a bunch of images to the blue team from an anonymous location. These images looked like they were related to Easter preparations, but they contained a message sent by McSkidy. The crisis communication process outlines that a message might be sent through a folder of images containing hidden messages that can be decoded if you know the keyword. The blue team has to create a YARA rule that runs on the directory containing the images. The YARA rule must trigger on a keyword followed by a code word. After extracting all the code words in ascending order, the blue team will be able to decode the message.

#### Learning Objectives
- Understand the basic concept of YARA.
- Learn when and why we need to use YARA rules.
- Explore different types of YARA rules.
- Learn how to write YARA rules.
- Practically detect malicious indicators using YARA.

---

## Walkthrough 

**01 YARA Overview**

In this mission, McSkidy entrusts you with YARA, a core defensive tool used by TBFC to uncover hidden malicious activity threatening SOC-mas. YARA is designed to identify and classify malware by scanning files, memory, and systems for unique patterns left behind by attackers. These patterns act like digital fingerprints, allowing defenders to recognize malware even when it tries to disguise itself. Instead of relying solely on signatures from antivirus vendors, YARA empowers defenders to define their own detection logic based on observed attacker behavior. Within TBFC, YARA operates quietly in the background, uncovering subtle traces of King Malhare’s operations that might otherwise go unnoticed, helping restore stability as the network faces growing threats.


**02 YARA Matters**

YARA matters because modern threats are rarely obvious. Attackers often hide malware inside documents, scripts, or lightweight loaders that appear harmless at first glance. Traditional detection tools may miss these threats, especially when they are new or customized. YARA allows defenders to search for malware based on patterns, behaviors, and shared traits rather than filenames or known hashes. This makes it invaluable during post-incident investigations, where analysts need to confirm whether malware artifacts remain elsewhere in the environment. It is also heavily used in threat hunting, intelligence-driven scans using shared community rules, and memory analysis to detect malicious code running in active processes. For TBFC’s defenders, YARA ensures that even stealthy threats can be uncovered before they escalate.


**03 YARA Values**

YARA provides clarity and control in environments filled with noise and uncertainty. One of its greatest strengths is speed, allowing defenders to scan large collections of files and systems efficiently. Its flexibility supports detection using plain text, hexadecimal byte patterns, and regular expressions, making it adaptable to a wide range of malware techniques. YARA also gives analysts full control over what is considered malicious, enabling custom detections for emerging threats rather than waiting for external updates. Rules can be shared, reused, and improved by other defenders, strengthening collective defenses across kingdoms. Most importantly, YARA helps connect isolated indicators into a coherent picture, transforming scattered clues into actionable intelligence and enabling proactive threat hunting instead of passive monitoring.


**04 YARA Rules**

A YARA rule is the foundation of detection and is composed of three main parts: metadata, strings, and conditions. Metadata provides context about the rule, such as the author, purpose, and creation date, making rule management easier as collections grow. The strings section defines what YARA should search for, including text strings, hexadecimal byte patterns, or regular expressions. These strings represent the clues left behind by malware, such as suspicious commands, encoded payloads, or binary fragments. The condition section contains the logic that determines when a rule should trigger, combining strings and file properties using logical operators. Together, these components allow defenders to precisely describe malicious behavior and decide when enough evidence exists to flag a threat.

Strings can be simple text, enhanced with modifiers like nocase, wide, xor, or base64 to defeat obfuscation techniques. Hexadecimal strings are used to match raw byte sequences common in executables or shellcode, while regular expressions allow flexible matching for variable patterns like URLs or encoded commands. Conditions then bring these clues together, using logic such as any of them, all of them, or more complex expressions that include file size or exclusions, ensuring accurate and efficient detection.


**05 YARA Study Use Cases**

In a real TBFC investigation, analysts discovered that King Malhare’s forces used an IcedID trojan distributed as small loader executables. These files shared common traits, including the “MZ” header of Windows executables, specific malicious byte fragments, and identifiable strings related to Malhare’s operations. Using this intelligence, defenders wrote a YARA rule that combined text strings, hex patterns, and file size restrictions to reliably detect the malware while reducing false positives.

The rule was saved to a YARA file and executed recursively across the system, successfully identifying a malicious loader hidden deep within a user directory. By using YARA command-line options such as recursive scanning and optional string output, analysts gained both detection results and visibility into what triggered the match. This practical use case demonstrates how YARA transforms threat intelligence into actionable detection, allowing TBFC’s defenders to identify malware quickly, confirm its presence across systems, and prepare for the next stage of defense as the battle for SOC-mas continues.

---

## Objectives 

How many images contain the string TBFC?

Create a YARA rule 

<img width="1108" height="670" alt="image" src="https://github.com/user-attachments/assets/b9d12bc2-f8b2-4c97-992c-d3b6d8814364" />

<img width="1104" height="606" alt="image" src="https://github.com/user-attachments/assets/85b954da-86f5-4962-959d-579a731c1c11" />

And run the YARA rule 

<img width="1114" height="767" alt="image" src="https://github.com/user-attachments/assets/a2297afd-c508-452e-aeab-40e76ffebb38" />

`5`


What regex would you use to match a string that begins with TBFC: followed by one or more alphanumeric ASCII characters?

<img width="1116" height="537" alt="image" src="https://github.com/user-attachments/assets/61bd15ae-79a1-4d7f-8201-710bda9cb97d" />

`/TBFC:[A-Za-z0-9]+/`

What is the message sent by McSkidy?

<img width="1344" height="847" alt="image" src="https://github.com/user-attachments/assets/239b09f1-ab38-4345-b85e-18863542e7f8" />

`Find me in HopSec Island`

---

## Key Takeaways 

- YARA enables defenders to detect threats based on patterns and behavior rather than relying solely on filenames or hashes.

- Custom YARA rules give blue teams full control over what they define as suspicious or malicious activity.

- Strings, hexadecimal patterns, and regular expressions each play a distinct role in identifying different types of indicators.

- The condition section is the core decision-making logic that determines when a YARA rule should trigger.

- YARA can be used to analyze unconventional artifacts, such as images, not just executable malware.

- Proper use of regex improves detection accuracy while minimizing false positives.

- Running YARA recursively across directories helps uncover hidden indicators spread throughout a system.

- Combining multiple indicators into a single rule results in more reliable and meaningful detections.


---

## Reflection 

Working through this challenge emphasized the mindset required of a blue team defender: attention to detail, logical thinking, and the ability to translate clues into detection logic. Creating a YARA rule to extract hidden messages from images illustrated how attackers and defenders alike can use unconventional techniques, and why defenders must be equally adaptable. The process of identifying the correct regex pattern, validating detections, and ordering extracted code words demonstrated how small technical decisions directly affect investigative outcomes. Overall, this exercise strengthened my understanding of YARA as both a technical tool and a strategic asset, reinforcing its role in proactive defense and intelligence-driven security operations within TBFC’s SOC-mas environment.




