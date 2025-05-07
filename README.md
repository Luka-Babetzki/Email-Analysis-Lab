# Email Analysis Lab

_**Source:** https://blueteamlabs.online/home/challenge/network-analysis-malware-compromise-e882f32908_

## Objective

The objective of this lab is to investigate a suspicious email that may be part of a phishing campaign. By analysing the raw email structure, header data, attachments, and embedded content, I aimed to uncover indicators of compromise, spoofed file types, and potential payload delivery. This exercise simulates a real-world SOC investigation into a potentially malicious email sent to a user.

### Skills Learned

- Email header analysis and identification of spoofed fields
- Using base64 decoding to analyse email body and attachments
- Identifying file types based on binary signatures
- Extracting and inspecting hidden or embedded files
- Investigating encoded content for additional IOCs

### Tools Used

- Notepad++
- CyberChef
- HxD (Hex Editor)
- Garry Kessler's File Signature Table
- sqrx (to view Excel file content)
- Exif

---
## Steps

### Scenario

CoCanDa, a planet known as 'The Heaven of the Universe' has been having a bad year. A series of riots have taken place across the planet due to the frequent abduction of citizens, known as CoCanDians, by a mysterious force. CoCanDa’s Planetary President arranged a war-room with the best brains and military leaders to work on a solution. After the meeting concluded the President was informed his daughter had disappeared. CoCanDa agents spread across multiple planets were working day and night to locate her. Two days later and there’s no update on the situation, no demand for ransom, not even a single clue regarding the whereabouts of the missing people. On the third day a CoCanDa representative, an Army Major on Earth, received an email.

### 1. <Step 1>

<...>

---
### 2. <Step 2>

<...>

---
## Lab Answers

1). **What is the email service used by the malicious actor?** _`emkei.cz`_

2). **What is the Reply-To email address?** _`negeja3921@pashter.com`_

3). **What is the filetype of the received attachment which helped to continue the investigation?** _`.zip`_

4). **What is the name of the malicious actor?** _`Pestero Negeja`_

5). **What is the location of the attacker in this Universe?** _`The Martian Colony, Beside Interplanetary Spaceport.`_

6). **What could be the probable C&C domain to control the attacker's autonomous bots?** _`pashter.com`_

---
## Lessons Learned

- Header mismatches like `From` vs. `Reply-To` are common phishing indicators.
- File extensions can be misleading; always check actual file signatures.
- Base64 encoding is often used to obfuscate data.
- CyberChef is a powerful tool for safe, in-browser decoding and inspection.
- Clearing formatting in Office files can reveal hidden or obfuscated data.
