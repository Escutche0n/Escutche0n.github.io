# How to Connect Your Mac with UOB Printers

> Note for taught students: most personal Mac computers are charged at colour rates for print release, whichever queue they use. We are unable to find a solution at the current time and recommend that student Mac users print from University computers to avoid this cost. [Print via email](https://www.bristol.ac.uk//it-services/applications/printing/printviaemail.html) is available for occasional use.

*Note for all users: you will need to choose whether to print in Black and White or Colour at the time you are sending your document to the printer. For example, you will need to specify your colour settings via an application such as Preview or Microsoft Word after selecting File > Print...*

### A. Note printer queue name 

The Print Release queue names are as follows. These are used in the URL field when you add a printer (see below).

- **smb://inf-uniflow.cse.bris.ac.uk/Print_Release_Mac**

### B. Obtain the correct software (drivers) for your printer

Copy the <a href="https://github.com/Escutche0n/Escutche0n.github.io/raw/master/resources/CNADVC5235X1.PPD.gz">CNADVC5235X1.PPD.gz</a> file corresponding to the printer you are using to the following folder.

HAHAHA

### C. Add the printer to your Mac

1. Ensure your Mac is fully up to date. To do so:

2. - Apple Menu > About This Mac > Software Update...
   - Repeat this process until there are no remaining updates to be installed.

3. Open **System Preferences**

4. Click on **Printers & Scanners**

5. Click the **+** button to add a new printer

6. The first time you do this, you need to add the

    

   Advanced

    

   button to the

    

   Printers & Scanners

    

   window. To do so:

   - Hold the **Ctrl** key and **Left click** on the toolbar, next to the **Windows** icon.
   - Select **Customize Toolbar...**
   - Drag the **Advanced** button into the toolbar.
   - Click **Done**

7. You're now ready to add the printer. To do so:

8. - Click **Advanced**
   - Type: **Windows printer via spoolss**
   - Device: **Another Device**
   - URL: **smb://inf-uniflow.cse.bris.ac.uk/Print_Release_Mac**
   - Name: **UoB Print Release**
   - Location: **UoB**
   - Use: **Select Software...**

9. You will be presented with a list of drivers for Canon printers. You should locate the driver that corresponds to the model of Canon printer that you are most likely to be using. Generally, this is most likely to be:

10. - **Canon iR-ADV C5235-5240 PS**
    - Click **OK**

11. Click the **Add** button to save this printer.

12. You will be presented with printer customisation options. It is advisable to select duplexing to enable 2-sided printing.

### D. Authenticate

You will be prompted to authenticate when you first print to the UoB Print Release system.

- Make sure to use your University username and password, which may not be the same as your Mac's computer account username.
- Prefix your username with **uob\** (e.g. uob\ab12345)
- See this example:
- ![img](/Users/elvischen/Library/Mobile%20Documents/com~apple~CloudDocs/Materials/Markdown_Pics/printer-auth.png)

- Note: We recommend that you don't tick the "Remember this password in my keychain" box until you have verified that your username and password work.
- If you have entered the incorrect credentials, or change your UoB password, and have previously ticked the "keychain" box, you will need to modify your personal Keychain. To do so:
  - Browse to Finder > Utilities > Keychain Access.
  - Click on "Passwords", and find the entry with the name of the printer. Right-click on it, and delete it.