# OSWA
A collection of useful commands, scripts and resources for the OSWA (WEB-200) exam of Offensive Security.

[Full write-up about the OSWA exam](https://bastijnouwendijk.com/my-oswa-certification-journey){:target="_blank"}

## Tools
Tools to install on fresh Kali VM:
- [SecLists](https://github.com/danielmiessler/SecLists) 
	- *Install*: `sudo apt-get install seclists`
	- *Path*: `/usr/share/seclists/`
- [Payload All The Things](https://github.com/swisskyrepo/PayloadsAllTheThings)
	- *Install*: `sudo apt-get install payloadsallthethings`
	- *Path*: `/usr/share/payloadallthethings/`
- gobuster
	- *Install*: `sudo apt-get install gobuster`

## Folder hierarchy used during exam on Kali VM
The following folder hierarchy can be used during the exam. The exam-connection folder contained all connection package files. The filehosting folder should be loaded up with pre-made malicious files, such as a JavaScript file that steals cookies. All files in this folder can be hosted on the kali VM by cd’ing into the folder and running `python3 -m http.server 80`. In the `target-0X` folders you can save specific files or POST requests related to the target. This setup can save you time during the exam.

```plaintext
oswa/exam
├── exam-connection
│   └── connection pack files
├── filehosting
│   └── xss.js
│   └── cookies.js
│   └── etc...
├── target-01
├── target-02
├── target-03
├── target-04
└── target-05
```

## Commands and payloads
Commands and payloads to use during the exam can be found in the folder `commands`. It consists mainly of wfuzz commands using wordlists from SecLists. To be able to quickly execute the prepared commands, it relays on having set terminal variables with the URL and IP of the target machine. You can do this with the commands `export URL="http://192.168.XX.XX"` and `export IP="192.168.XX.XX"`. 

## Notes and reporting
To spare time on the exam, use both the note and report template which can be found in the folders `note taking` and `reporting`. For note-taking, I used Obsidian where I created a note for each target machine using the `machine_template.md` template. For reporting, I created a Word template based on the exam template provided by Offensive Security but with improved visuals, layout, and headers.


