---
layout: page
title: Processed Archives
nav_order: 1
parent: Born-Digital Archives
grand_parent: Ingest
---

## Data Model

Born-Digital Archives Data Model is created to accommodate a wide range of content collected by the NYPL
[Digital Archives program](https://nypl.github.io/digarch/).

### Data Model Description

The following data model describes how a FA component package will be structured after ingested
into the digital repository software, Preservica.

Each component forms a Structural Object (SO), named as "DI/EM/ER Container", which can be understood as a folder. DI stands for Digital Image;
EM stands for Email; and ER stands for Electronic Record. DI/EM/ER Container must have one metadata SO, named "(original folder title)_metadata", and one contents SO, "(original folder title)_contents". Within the metadata SO, there may not have any file, or it may have metadata file(s).
Within the contents SO, there can be Information Object(s) (IO), also known as asset(s), and/or file and folder hierarchy, depending on the original content structure.

![alt text]({{site.baseurl}}/assets/img/svg_data_model_born_digital_archives.svg "Diagram using the Unified Modeling Language showing the Data Model of
the Born-Digital Archives, including the data classification and its relationships, folder names, metadata fragments, security tags")

### Notes on Ingest

#### Incorrect Folder Names

The package folder name should conform to the pattern
`M[0-9]+_(ER|DI|EM)_[0-9]+`.
If a folder name deviates from this pattern:

1. Rename the folder as appropriate, e.g. `M1234_ER1` become `M1234_ER_1`
2. Review with the Digital
Archives program if the solution is unclear

#### Incorrect metadata files

The conventions for processed metadata files has varied over time.
If a file does not meet the expectation of being a CSV exported from FTK:

1. Review the metadata file and determine whether the file is necessary.
2. Exports from reporting tools like DROID can be deleted.
3. Non-standard extensions used for the FTK export should be updated to csv.

### Filename encoding issues

Filenames may include characters with unclear or unusable renderings. Two common
common sources of these issues are
[PUA encoded characters](https://en.wikipedia.org/wiki/Private_Use_Areas) and control
characters such as ASCII [BEL](https://en.wikipedia.org/wiki/Bell_character).

If a filename has an character encoding issue:

1. View the underlying bytes for the filename

    ```sh
    ls -1 path/digital_preservation.docx | xxd
    ```
2. Map the bytes back to a Unicode codepoint using a conversion tool, e.g. `\xEF \x80 \xA1` is `U+F021`
3. Convert the character to an acceptable version.
    * Control characters can be converted to visual representations, such as `\x7f` to `U+2421`
    * PUA characters can be converted to their original character if they were part of a PUA conversion block.
    * Characters may need to be deleted if they are unmappable, such as the `U+F8FF`

### Filenames with non-XML compatible characters

Preservica uses XML to store and transact metadata.
In XML files, the characters `&`, `<`, `>`, `"`, and `'` must be escaped if used as a value.
Any filename containing these characters must be escaped within the XML.
The packaging script does this escaping, but it may occasionally fail on complex cases.

If the packaging script has issues with escaping the character correctly:

1. Find the filename within the package's XML files.
2. Use an XML linter to determine the correct escaping.
3. Update the XML files.

### Virus detected in the file

Preservica scans for computer viruses with ClamAV before ingesting the package.
Virus detection causes the ingest workflow to abort. However, not all viruses are dangerous, especially within modern computing enviroments.
For example, macro viruses were very common in late 90s Microsoft Office files.
These macros are no longer executed in modern versions of Office, and even in emulated environments, their effects are contained to annoyances instead of serious damages.

If a virus is detected in a package:

1. Scan the file with ClamAV and other malware scanners to identify the issue. Malware can have different names depending on the scanning software, so it's most useful to collect all the names possible.
2. Research the malware and determine its effects.
3. If there is a risk, quarantine the entire package and discuss with Digital Presevation team.
4. If there is no risk, adjust the Preservica workflow to allow the ingest temporarily.
   1. Make sure no ingest workflows are active or being added.
   2. On the Manage page for Ingest, select "Workflow Error Configuration" for the workflow context
   3. Change the action for "The Virus Check step found a virus in the package" from  `Abort workflow` to `Continue workflow`
   4.  Resubmit the aborted package via the Ingest Monitor.
   5.  After the package is ingested, change the action back to `Abort Workflow`

##### Example

In one collection, a file was flagged as containing a trojan named "Win.Trojan.Cap-1" in ClamAV's virus registry.
After some research using the Internet Archive, we found that this computer virus, "CAP", was most likely a Microsoft Word Macro virus.
[This Microsoft Security Intelligence page](https://www.microsoft.com/en-us/wdsi/threats/malware-encyclopedia-description?name=Virus%3AWM%2FCap.A), [this Internet Archive capture](https://web.archive.org/web/20130729073004/http://vxheaven.org/29a/29a-2/29a-2.5_6), [the Virus Encyclopedia](http://virus.wikidot.com/cap) and [F-Secure](https://www.f-secure.com/v-descs/cap.shtml) give us information most relevant to this virus.

In this case, we determined this to be a low-risk file.

1. The specific variant in the files was a malformed one that did not execute anything.
2. Over the years, Microsoft has done many interventions about these viruses. One change from 2022 is that [macros from the internet are blocked by default in Microsoft office](https://learn.microsoft.com/en-gb/DeployOffice/security/internet-macros-blocked).
3. Microsoft also added more warnings before the use can enable the macro (see [25 years on, Microsoft makes another stab at stopping macro malware](https://grahamcluley.com/microsoft-stab-macro-viruses/)).
4. On top of the intervention from Microsoft, NYPL's processes for accessing these Microsoft Word document outside of emulated containers is to create PDF surrogates that cannot contain macros.

With these considerations, we made the decisions to ingest the files.
