## GPG Export to Windows

1. Export Keys on Old Machine (Linux/macOS)
   Open your terminal and use the following commands, replacing your_email@example.com with your key's email or ID.
   Export Private Key (Secret Key):
   bash
   gpg --export-secret-keys --armor your_email@example.com > private-key.asc
   Export Public Key (Optional):
   bash
   gpg --export --armor your_email@example.com > public-key.asc
   Export Ownertrust (Optional, but recommended for trusted keys):
   bash
   gpg --export-ownertrust > otrust.txt

2. Move Files to Windows
   Transfer private-key.asc, public-key.asc, and otrust.txt to your Windows machine via a secure method (e.g., encrypted USB drive).
   Gist
   Gist
   +1
3. Import Keys on Windows
   Install Gpg4win: Download and install it from gpg4win.org.
   Import via Command Line: Open PowerShell/CMD in the folder containing your files and run:
   powershell
   gpg --import private-key.asc
   gpg --import public-key.asc
   gpg --import-ownertrust otrust.txt
   Import via Kleopatra (GUI):
   Open Kleopatra.
   Click File -> Import Certificates...
   Select your .asc files.
   Gpg4win
   Gpg4win
   +4
4. Verify
   Run gpg --list-secret-keys in PowerShell to confirm your keys are present.
   Gist
   Gist
