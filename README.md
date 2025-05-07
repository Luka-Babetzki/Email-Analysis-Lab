# Email Analysis (Phishing) Lab

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

---

### 1. Preliminary Analysis

I began by opening the `.xml` email file using **Notepad++**. I learned that emails contain three key types of header information:

1. **Standard Fields** – such as `Delivered-To`, and `Received`  
2. **X-Headers** – like `X-Google-Smtp-Source`, `X-Received`  
3. **ARC Headers** – including `ARC-Seal`, `ARC-Message-Signature`

One of the first red flags I noticed was the **SPF (Sender Policy Framework)** result:  
`spf=fail`. This indicates that the sending domain failed authentication, which is often a strong indicator of spoofing or unauthorised sending.

In the section containing the email addressing fields, I observed mismatches between the `From:` and `Reply-To:` headers:

- `From: <billjobs@microapple.com>`  
- `Reply-To: negeja3921@pashter.com`

This mismatch is a key indicator of phishing, suggesting the email may impersonate a legitimate sender while replies are directed elsewhere.

Next, I analysed the `Content-Type` of the email. The email declared itself as `multipart/mixed`, meaning it contains multiple parts in different formats — typical of emails with attachments or alternate body types. These parts were separated by a defined boundary string `(BOUND)`.

### 2. Investigation

The first part of my investigation involved analyzing the base64-encoded sections within the email. The body included a `Content-Type: text/plain` section that was encoded in base64. I copied this block into CyberChef, applied the "From Base64" operation, and successfully decoded a plaintext message. This output was preserved as a screenshot.

Next, I examined the email's attachment, labeled as: `Content-Type: application/pdf; name="PuzzleToCoCanDa.pdf"` This section was also base64-encoded. I decoded the content in CyberChef, but it did not resemble a PDF. To determine the true file type, I used the To Hex operation. The first four bytes were: `50 4B 03 04`. Using Garry Kessler’s File Signature Table, I confirmed that this signature indicates a `ZIP` archive, not a PDF. I verified this by looking up the correct PDF signature `(25 50 44 46)` and confirming they did not match. This suggested the attachment was spoofed, using a misleading name to trick the recipient.

I saved the decoded content as a `.zip file` and extracted it. The archive contained a directory named `PuzzleToCoCanDa`, which included the following three files:

- `DaughtersCrown`
- `GoodJobMajor`
- `Money.xlsx`

I ensured hidden files were visible in the folder by enabling "Hidden items" in Windows Explorer.

To inspect these files without executing them, I opened each one using HxD, a hex editor. Examining the first four bytes of each file:

- DaughtersCrown: `FF D8 FF E0` → JPEG image
- GoodJobMajor: Began with a valid PDF file signature
- Money.xlsx: Consistent with an Excel spreadsheet file

I renamed the files accordingly to reflect their true formats.

Next, I analyzed Money.xlsx using sqrx since my VM did not have Excel installed. The spreadsheet had two sheets, both of which initially appeared empty. To check for obfuscated content, I selected all cells and cleared formatting **(Ctrl+A > Right-click > Clear > Format)**. This revealed a base64-encoded string hidden in a cell on the second sheet. I decoded it in CyberChef to uncover its contents for further analysis.

### 3. Observations

| Key Field         | Value                                   | Description                                                  |
|-------------------|-----------------------------------------|--------------------------------------------------------------|
| SPF               | `spf=fail`                              | Fails domain verification; indicates spoofing                |
| From              | `billjobs@microapple.com`               | Likely impersonating a brand or user                         |
| Reply-To          | `negeja3921@pashter.com`                | Sends responses to a different domain                        |
| Attachment Name   | `PuzzleToCoCanDa.pdf`                   | Misleading filename to imply PDF                             |
| File Type (actual)| ZIP (`50 4B 03 04`)                     | Detected via file signature; not actually a PDF              |
| Embedded Files    | JPEG, PDF, Excel                        | Delivered inside spoofed ZIP archive                         |
| Hidden Content    | Base64-encoded string inside Excel      | Revealed by removing formatting in a spreadsheet cell        |

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
