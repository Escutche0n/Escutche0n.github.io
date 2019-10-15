---
layout:     post
title:      How to Connect UOB Printers to your Macbook? 
subtitle:   "如何在 Macbook 上设置布里斯托大学打印机"
date:       2019-10-15
author:     "Elvis"
catalog: true
comments: true
guid: urn:uuid:bd69ccfc-e840-4bc4-99ec-54cefac7f749
tags:
    - tutorial
		- university
---

# How to Connect UOB Printers to your Macbook? 

> **Print via email** is available for occasional use.
>
> **[print-release-bw@bristol.ac.uk](mailto:print-release-bw@bristol.ac.uk)**  for B&W documents.
>
> **[print-release-colour@bristol.ac.uk](mailto:print-release-colour@bristol.ac.uk)** for documents in colour.



Note for all users: you will need to choose whether to print in **Black and White** or **Colour** at the time you are sending your document to the printer. For example, you will need to specify your colour settings via an application such as Preview or Microsoft Word after selecting `File` > `Print`...



### A. Note printer queue name 

The Print Release queue names are as follows. These are used in the URL field when you add a printer (see below).

```javascript
smb://inf-uniflow.cse.bris.ac.uk/Print_Release_Mac
```

### B. Install the correct driver

Locate the path and copy the <a href="https://github.com/Escutche0n/Escutche0n.github.io/raw/master/resources/CNADVC5235X1.PPD.gz">CNADVC5235X1.PPD.gz</a> file to this folder.

```javascript
disc/Library/Printers/PPDs/Contents/Resources/
```

### C. Add the printer to your Mac

1. Update the latest **macOS**(10.15 Catalina tested).
2. Open **System Preferences**
3. Click on **Printers & Scanners**
4. Click the **`+`** button to add a new printer
5. The first time you do this, you need to add the `Advanced` button to the `Printers & Scanners` window. To do so:

   - Hold the **`⌃`** key and **Left click** on the toolbar, next to the **Windows** icon.
   - Select **Customize Toolbar...**
   - Drag the **Advanced** button into the toolbar.
   - Click **Done**
6. You're now ready to add the printer. To do so:
   1. Click **Advanced**
   2. Type: **Windows printer via spoolss**
   3. Device: **Another Device**
   4. URL: **smb://inf-uniflow.cse.bris.ac.uk/Print_Release_Mac**
   5. Name: **UoB Print Release**
   6. Location: **UoB**
   7. Use: **Select Software...**
7. Click the `Add` button to save this printer.
8. You will be presented with printer customisation options. It is advisable to select duplexing to enable 2-sided printing.

### D. Authenticate

You will be prompted to authenticate when you first print to the UoB Print Release system.

- Make sure to use your University username and password, which may not be the same as your Mac's computer account username.
- Prefix your username with `uob\` (e.g. `uob\ab12345`)
- See this example:
- ![img](https://www.bristol.ac.uk//it-services/applications/printing/images/printer-auth.png)

- Note: We recommend that you don't tick the "Remember this password in my keychain" box until you have verified that your username and password work.
- If you have entered the incorrect credentials, or change your UoB password, and have previously ticked the "keychain" box, you will need to modify your personal Keychain. To do so:
  - Browse to `Finder` > `Utilities` > `Keychain Access`.
  - Click on `Passwords`, and find the entry with the name of the printer. Right-click on it, and delete it.