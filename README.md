# BYOB (Build Your Own Botnet)
[![license](https://img.shields.io/badge/license-GPL--3.0-green.svg)](https://github.com/colental/byob/blob/master/LICENSE)

BYOB is an open-source project that provides a library of packages
and modules which provide a basic framework for security researchers and
developers looking to roll-up their sleeves and get some hands-on experience
defending networks against a simulated botnet roll-out with realistic droppers
that will test the limits of your capacity to defend your network

The library contains 4 main parts:

1) `byob.client`: *generates unique, virtually undetectable droppers with staged payloads
  and a number of optional features can be added via intuitive command-line
  arguments* (`client.py -h/--help` for detailed usage information)

2) `byob.server`: *console based command & control server with a persistent database for
  managing the client's reverse TCP shell sessions, tracking tasks issued
  to each client, storing results of each client's completed tasks, as well
  as hosting the byob.remote package online for clients to access remotely*

3) `byob.core`: *supackage containing the core modules used by the command & control server
   and the client generator*
  - `byob.core.util`: *miscellaneous utility functions that are used by many modules*
  - `byob.core.handlers`: *request handlers which can be paired with the base Server class to form 
    2 different types of server instances which the C2 runs in parallel with
    the main server instance*
    - __RequestHandler__: handles requests for files in the byob.remote package
    - __TaskHandler__: tracks issued tasks & stores completed tasks in database

  - `byob.core.security`: *module containing the Diffie-Hellman Internet Key Exchange (RFC 2741)
    method for securing a shared secret key even over insecure networks,
    as well as encryption & decryption methods for 3 different modes to
    ensure secure communication no matter what*

    - __AES-256__ in authenticated OCB mode (*requirements*: `PyCrypto` & `pycryptodome`) 
    - __AES-256__ in CBC mode with HMAC-SHA256 authentication (*requirements*: `PyCrypto`)
    - __XOR-128__ stream cipher that uses only builtin python keywords (*requirements*: none)

  - `byob.core.loader`: *enables clients to remotely import any package/module/script from the server
    by requesting the code from the server, loading the code in-memory, where
    it can be directly imported into the currently running process, without 
    writing anything to the disk (not even temporary files - zero IO system calls)*

  - `byob.core.payload`: *reverse TCP shell designed to remotely import post-exploitation modules from
    server, along with any packages/dependencies), complete tasks issued by
    the server, and handles connections & communication at the socket-level*

  - `byob.core.generators`: *module containing functions which all generate code by using the arguments
    given to complete templates of varying size and complexity, and then output
    the code snippets generated as raw text*

3) `byob.modules`: *package containing 11 post-exploitation modules that the server hosts online
    for clients to import remotely*

   1) `byob.modules.keylogger`: *logs the user’s keystrokes & the window name entered*
   2) `byob.modules.screenshot`: *take a screenshot of current user’s desktop*
   3) `byob.modules.webcam`: *view a live stream or capture image/video from the webcam*
   4) `byob.modules.ransom`: *encrypt files & generate random BTC wallet for ransom payment*
   5) `byob.modules.outlook`: *read/search/upload emails from the local Outlook client*
   6) `byob.modules.packetsniffer`: *run a packet sniffer on the host network & upload .pcap file*
   7) `byob.modules.persistence`: *establish persistence on the host machine using 5 different methods*
      - launch agent   (*Mac OS X*)
      - scheduled task (*Windows*)
      - startup file   (*Windows*)
      - registry key   (*Windows*)
      - crontab job    (*Linux*)
   9) `byob.modules.phone`: *read/search/upload text messages from the client smartphone*
   10) `byob.modules.escalate`: *attempt UAC bypass to gain unauthorized administrator privileges*
   11) `byob.modules.portscanner`: *scan the local network for other online devices & open ports*
   12) `byob.modules.process`: *list/search/kill/monitor currently running processes on the host*
       - `byob.modules.payloads`: *package containing the payloads created by client generator that 
         are hosted locally by the server (rather than uploaded to Pastebin to be hosted there 
         anonymously) for the client stagers to load & execute on the target host machines*
       - `byob.modules.stagers`: *directory containing the stagers created by the client generator 
         which are hosted locally by the server (rather than uploaded to Pastebin to be hosted there 
         anonymously) for the client droppers to load & execute on target host machines*
