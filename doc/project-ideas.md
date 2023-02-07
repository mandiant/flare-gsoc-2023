# Project Ideas
*FLARE @ Google Summer of Code 2023*

This document lists examples of projects that would be great for GSoC '23 contributors. The list doesn't include everything - feel free to identify your own idea and propose it!

All of our project ideas revolve around reverse engineering tools. That is, we want to improve the lives of malware analysts through novel techniques and automation. To succeed with any of these examples, you should have a basic familiarity with reverse engineering or a strong desire to learn.

Briefly, [capa](https://github.com/mandiant/capa) identifies the capabilities in executable files, such as "installs a service" or "downloads data via HTTP". [FLOSS](https://github.com/mandiant/flare-floss) automatically deobfuscated protected strings in malware. [FakeNet](https://github.com/mandiant/flare-fakenet-ng) intercepts and redirects all network traffic while simulating legitimate network services. Each of these tools is used by thousands of analysts to identify, describe, and stop malware.


## capa: Ghidra Integration
*Develop a Ghidra Feature Extractor for capa*

| Difficulty | Size   | Potential Mentors | Link                          |
| ---------- | ------ | ----------------- | ----------------------------- |
| Medium     | Medium | Mike Hunhoff      | [github.com/mandiant/capa #49](https://github.com/mandiant/capa/issues/49) |

### Description

capa is the FLARE team’s open-source tool to identify program capabilities using an extensible rule set. Each rule is matched against features that capa extracts from a program. Extracted features include file-level features such as strings, section names, imports, and exports and function-level features such as API calls, string and byte references, instruction mnemonics, and number constants.

capa uses feature extractors, called "backends", to extract features from supported file types (PE, ELF, and .NET) and architectures (32- and 64-bit x86). Each backend is built around an existing tool or library that provides file parsing and disassembly capabilities. capa uses this to extract features. capa currently implements backends using Vivisect, IDA Pro, and dnfile.

Ghidra is a popular open-source disassembly framework with a robust API to access its analysis. Programs can interact with a wealth of information that includes parsed file formats and disassembled code. The goal of this project is to develop a Ghidra backend for capa using Python 3 (via Ghidrathon) and Ghidra’s scripting API. Users should be able to invoke capa such that it uses Ghidra’s analysis engine and/or invoke capa from within Ghidra.

### Deliverables

  - Develop Ghidra backend for capa using Python 3 (via Ghidrathon) and Ghidra’s scripting API
    - Support PE and ELF file format and 32- and 64-bit x86
    - Pass unit tests used to validate existing backends (Vivisect, IDA Pro)
    - Output results to Ghidra console window
    - Support analysis in Ghidra headless mode
  - Stretch goals:
    - User interface for Ghidra that shows the results of capa analysis using appropriate widgets, like the capa explorer plugin for IDA Pro
    - Standalone, headless invocation of Ghidra analysis from capa

### Required Skills

  - Medium knowledge of Python 3
  - Basic understanding of reverse engineering (focus: Windows PE files)
  - Basic understanding of 32- and 64-bit x86 assembly language
  - Basic understanding of disassembly tools including Ghidra, IDA Pro, and Binary Ninja
  - Interest in malware analysis with focus on static analysis
  - Basic understanding of Git


## capa: Capabilities from Dynamic Analysis
*Extract Program Capabilities from Dynamic Sandbox Analysis Runs*

| Difficulty | Size  | Potential Mentors  | Link                          | 
| ---------- | ----- | ------------------ | ----------------------------- |
| Hard       | Large | Moritz Raabe       | [github.com/mandiant/capa #48](https://github.com/mandiant/capa/issues/48) |
|            |       | Willi Ballenthin   |                               |
|            |       | Ana Martinez Gomez |                               |

### Description

capa is an open-source tool to identify program capabilities using an extensible rule set. Currently, the project relies purely on static analysis of code structures to identify patterns. This project extends capa to work on dynamic execution data such as sandbox run traces or code emulation analysis.

Anti-analysis techniques like packing or other implementation intrinsics can hinder pure static analysis. With the functionality to inspect and reason about dynamic analysis results the project can provide further insight into the capabilities of unknown executables. The current capa rule set does not directly translate to identify behaviors from sandbox or debugging output, but we anticipate that there is a lot of overlap.

We are currently aware of two Master’s thesis that cover similar approaches. As part of this work, contributors should study related existing work to create the most flexible and useful solution.


### Deliverables

  - Research
    - Literature review and identification of further existing work and related projects
    - Compare sandbox and debugger-generated dynamic analysis data to identify best tool/technique
  - Feature extraction
    - Survey features to extract from dynamic analysis
    - Write feature extractors for identified tool/technique
  - Identification rules
    - Analyze how to reuse existing rule set and identify required adjustments
    - Create proof of concept rules
    - Adjust existing or create new rule syntax (rule statements and features)
  - Evaluation
    - Compare results of static and dynamic analysis
    - Write blog post about experience and project achievements

### Required Skills

  - Medium knowledge of Python 3
  - Basic understanding of reverse engineering (focus: Windows PE files)
  - Basic understanding of 32- and 64-bit x86 assembly language
  - Interest in malware analysis with a focus on dynamic analysis
  - Basic understanding of Git


## FLOSS: New Display Modes
*Overlay File Format and Metadata over Human Readable Strings*

| Difficulty | Size         | Potential Mentors | Link                          | 
| ---------- | ------------ | ----------------- | ----------------------------- |
| Easy       | Small-Medium | Moritz Raabe      | [github.com/mandiant/flare-floss #370](https://github.com/mandiant/flare-floss/issues/370) |
|            |              | Willi Ballenthin  |                               |

### Description

During reverse engineering, inspecting the strings in a program is one of the most easy yet valuable static analysis steps. Unfortunately, few existing tools take other available information about a file into consideration when showing strings. For example, strings.exe doesn’t indicate if a string comes from a popular library or if the string is an artifact of the file format.

Here we propose to extend the FLARE tool called FLOSS to leverage knowledge of the PE file format to improve the overview of a file’s strings. This means developing an intuitive display for technical information to guide analysts towards the most interesting part of a program. For example, the contributor may research ways to overlay the PE file format alongside the strings data to show that a string comes from the import section, statically linked code, or as an inline or stack string.

Today, FLOSS automatically deobfuscates protected strings found in malware. A new paradigm to display strings and potentially other static information would further its usefulness and manifest success as the default tool used by security analysts.

### Deliverables

  - Develop mechanism to overlay file format with strings extracted by FLOSS.
    - Indicate strings from headers such as the import table.
    - Show relationships between sections and strings.
  - Develop mechanism to overlay disassembly with strings extracted by FLOSS.
    - Ignore strings that are actually false positives in code.
    - Highlight strings referenced by non-library code.
    - De-emphasize strings referenced by library code.
  - Evaluation
    - Collect feedback from users and make changes as appropriate.
    - Write blog post about experience and project achievements

### Required Skills

  - Medium knowledge of Python 3
  - Basic understanding of reverse engineering (focus: Windows PE files)
  - Basic understanding of 32- and 64-bit x86 assembly language
  - Basic understanding of disassembly tools including Ghidra, IDA Pro, and Binary Ninja
  - Interest in malware analysis with focus on static analysis
  - Basic understanding of Git


## FLOSS: Language Specific Strings (Go, Rust, ...)
*Extract Language and Runtime Specific Strings*

| Difficulty | Size   | Potential Mentors | Link                          | 
| ---------- | ------ | ----------------- | ----------------------------- |
| Medium     | Medium | Moritz Raabe      | [github.com/mandiant/flare-floss #370](https://github.com/mandiant/capa/issues/370) |
|            |        | Willi Ballenthin  |                               |

### Description

Various programming languages embed the constant data, like strings, used within executables in different ways. Most tools, like strings.exe, just look for printable character sequences. This doesn’t work well for files compiled from Go or Rust. 

Here we propose to extend the FLARE tool called FLOSS to include a framework to extract language specific strings from executables. After identifying the language, a specific extractor can use specialized logic to pull out the strings embedded into a program by the author. When possible, the extractor should indicate library and runtime-related strings. For example, the extractor may parse debug information to recognize popular third party libraries and annotate the related strings appropriately. 

Today, FLOSS automatically deobfuscates protected strings found in malware. Better categorization of its output would make its users more efficient. Extracting language-specific strings would make FLOSS more useful and manifest success as the default tool used by security analysts.


### Deliverables

  - Develop language identification module
    - Initial focus on Go and Rust
    - Consider also Swift, Zig, …
  - Research language string embeddings and create extractor code
    - We can share existing knowledge and code to bootstrap this
  - Identify strings related to runtime and library code for targeted programming languages
  - Extend standard output format and render results

### Required Skills

  - Medium knowledge of Python 3
  - Basic understanding of reverse engineering (focus: Windows PE files)
  - Experience with golang or Rust (internals) is a plus, but not required
  - Interest in malware analysis with focus on static analysis
  - Basic understanding of Git


## FakeNet-NG: Interactive Graphical Summary of NBIs
*Build an HTML5 Interactive, Graphical Summary of NBIs extracted by FakeNet*

| Difficulty | Size  | Potential Mentors | Link                          | 
| ---------- | ----- | ----------------- | ----------------------------- |
| Medium     | Large | Tina Johnson      | [github.com/mandiant/flare-fakenet-ng #19](https://github.com/mandiant/flare-fakenet-ng/issues/19) |

### Description

FakeNet-NG is a dynamic network analysis tool built for malware analysis with the goal of intercepting malicious network traffic and simulating Internet-like conditions in a VM. Currently, there are two ways in which FakeNet logs its output, either print out the logs to the console or write it to a log file. These formats of output are not user-friendly and can make it a tedious job for the analyst to glean any important information from the output. To improve this, we would like the mentee to build an HTML5 page that displays the network-based indicators (NBIs) from the requests intercepted by FakeNet as a list, grouping NBIs by process. We would also like the mentee to build a graph visualizing the relationship between individual processes and their network requests. 

### Deliverables

  - Add a callback to HTTP, TCP, UDP, and DNS listeners to supply NBIs from the request they handle to the Diverter component.
  - Add data structures in Diverter to track network requests, proxied network requests (requests redirected to proxy listener by FakeNet), and related process information.
  - Use the data stored in data structures mentioned above to list the NBIs in an HTML5 page, grouped by process. The user should have an option to select/deselect, filter, and copy the listed information.
  - Create a graph that maps processes to their network requests. Edges of the graph should connect a process node to other nodes such as IP address or domain name.
  - Test and verify that the feature works for both SingleHost and MultiHost mode of FakeNet.


### Required Skills

  - Good knowledge and experience working with Python 3
  - Basic knowledge of networking concepts and common protocols
  - Basic knowledge of HTML5
  - Basic knowledge of JavaScript
  - Basic knowledge of Git
  - Ability to run Linux and Windows VMs
