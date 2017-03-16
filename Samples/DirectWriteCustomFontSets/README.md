<!---
  category: 
  samplefwlink: 
--->

# DirectWrite Custom Font Sets Sample

Shows how to work with custom fonts in Windows 10 using DirectWrite.

## Overview

This sample is a console app that demonstrates different ways of creating custom
font sets using DirectWrite. There are command line options for selecting one of
five different scenarios that are demonstrated:

1. Creates a custom font set using font files in a local file folder. The fonts
   are not assumed to be known by the app, and all font properties used in the
   font set are extracted directly from the font files as they are added to the
   set.

2. Creates a custom font set using a set of known fonts contained in the app.
   The app assigns custom properties to each font as it is added to the set.

3. Creates a custom font set using remote fonts from the Web. The remote fonts 
   are assumed to be known, and custom font properties are used, allowing the 
   custom font set to be created without needing to download the font data 
   beforehand. This scenario also demonstrates how to interact with DirectWrite
   APIs that handle downloading of the fonts as the actual font data is needed.

4. Creates a custom font set using font data contained in in-memory buffers.

5. Creates a custom font set using packed, WOFF2 font data.  This scenario
   demonstrates use of new APIs for unpacking font data that is packaged in
   WOFF or WOFF2 formats.

When run, the app will display console results that reflect the fonts added to
the custom font set.

A font set has entries for fonts along with a set of informational font properties
for each font. These properties may come directly from the font or may be set
by an app using custom values (as in scenarios 2 and 3). Some of these font-set
property values are reported by the sample app. In order to show that these can 
differ from actual font values, and to demonstrate how to interact with APIs for
downloading remote fonts as needed (scenario 3), some additional font details
that come directly from the actual font data are also reported.

This app can be run on any Windows 10 version (build 10240) or later, though some
scenarios require the Windows 10 Creators Update (preview build 15021 or later):

- Scenario 1 is supported on any Windows 10 version, but is implemented so as  
  to use newer, recommended APIs when run on Windows 10 Creators Update. 
- Scenario 2 is supported on any Windows 10 version. 
- Scenarios 3, 4 and 5 require Windows 10 Creators Update.

There is a define statement in stdafx.h (commented out) that can be used to
force the app to behave as though it is running on earlier Windows 10 versions 
even when it is actually running on Windows 10 Creators Update.

All of the DirectWrite APIs specifically related to implementing these
scenarios are found in dwrite_3.h.


##  Key project files

The following project files are of particular interest for implementing these
scenarios:

- DWriteCustomFontSets.h/.cpp: This has the main entry point and handles the
  main flow of the app.

- CustomFontSetManager.h/.cpp: This has the core implementations for each
  scenario -- creating the font set, and getting font information from the
  font set. For use of remote fonts (scenario 3), this includes interaction
  with DirectWrite's font download APIs.

The following project files are used for specific scenarios:

- BinaryResources.h/.cpp: This loads font data embedded in the app as a
  binary resource. Used in scenario 4.

- Document.h/.cpp: This simulates a document file containing embedded font
  data. Used in scenario 4.

- Font files in the Fonts subfolder within the project: These are used in
  scenarios 1 and 2. This subfolder needs to be copied into the folder where 
  the built .exe file is located.

- FontDownloadListener.h/.cpp: This has a custom implementation of the
  IDWriteFontDownloadListener interface, which an app needs to implement in
  order to use remote fonts. Used in scenario 3.

- PackedFontFileLoader.h/.cpp: This has a custom implementation of the
  IDWriteFontFileLoader interface that can handle font data contained in
  a packed container format -- WOFF or WOFF2. Used in scenario 5.

- Statics.cpp: This contains static data used in scenarios 2, 3 and 5.

These project files are helpers that are ancillary to the main things being 
demonstrated by the app:

- CommandLineArgs.h/.cpp: Handles processing of commandline arguments.

- DXHelper.h: Used for handling errors when calling DirectWrite APIs.

- FileHelper.h/.cpp: Handles local file system interactions.

These are standard project files:

- DWriteCustomFontSets.vcxproj: The main project file for VC++ projects,
  generated by Visual Studio.

- DWriteCustomFontSets.vcxproj.filters: The filters file for VC++ projects,
  generated by Visual Studio.

- stdAfx.h/.cpp: Used to build a precompiled header (PCH) file named
  DWriteCustomFontSets.pch and a precompiled types file named StdAfx.obj.

- targetver.h


##  Related topics

- [Custom Font Sets](https://go.microsoft.com/fwlink/?linkid=843534)
- [IDWriteFactory3 interface](https://msdn.microsoft.com/en-us/library/windows/desktop/dn890753)
- [IDWriteFactory5 interface](https://go.microsoft.com/fwlink/?linkid=843506)
- [IDWriteFontSet interface](https://msdn.microsoft.com/en-us/library/windows/desktop/dn933235)
- [IDWriteFontSetBuilder interface](https://msdn.microsoft.com/en-us/library/windows/desktop/dn933236)
- [IDWriteFontSetBuilder1 interface](https://go.microsoft.com/fwlink/?linkid=843507) 
- [DWRITE_FONT_PROPERTY structure](https://msdn.microsoft.com/en-us/library/windows/desktop/dn933212)
- [DWRITE_FONT_PROPERTY_ID enumeration](https://msdn.microsoft.com/en-us/library/windows/desktop/dn933213)
- [IDWriteFontFile interface](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371060)
- [IDWriteInMemoryFontFileLoader interface](https://go.microsoft.com/fwlink/?linkid=843510)
- [IDWriteRemonteFontFileLoader interface](https://go.microsoft.com/fwlink/?linkid=843513)
- [IDWriteFontDownloadQueue interface](https://msdn.microsoft.com/library/windows/desktop/dn890778)
- [IDWriteFontDownloadListener interface](https://msdn.microsoft.com/library/windows/desktop/dn890775)


## System requirements

- Client: Windows 10
- Server: Windows Server 2016
- Phone: Windows 10


## Building the sample

This sample requires the Windows 10 SDK build 15021 or later, plus the Visual
Studio 2017 Release Candidate or later.

1.  Start Microsoft Visual Studio 2017 and select File > Open > Project/Solution.

2.  Starting in the folder where you unzipped the samples, go to the Samples
    subfolder, then the subfolder for this specific sample. Double-click the
    Visual Studio 2017 Solution (.sln) file.

3.  Press Ctrl+Shift+B, or select Build > Build Solution.


## Deploying the sample

The project includes a subfolder, "Fonts", containing font files. These are 
used for scenarios 1 and 2. This sample doesn't include any installer project. 
In order for scenarios 1 and 2 to work, the Fonts folder and contents must be
copied to the same folder as the app executable.


## Running the sample

From a commandline, run the app without any commandline parameters. This will
display help content.

Briefly, the app recognized two types of commandline parameter:

- "/?": A request for help content.

- "/s" followed by a scenario number, 1 to 5.

To run the sample from within Visual Studio, set commandline parameters in the
project properties for the Debug configuration or other configurations: select
Configuration Properties > Debugging, and enter parameters (e.g., "/s 1") in
the Command Arguments field. Also note the step regarding deployment given
above: copy the Fonts folder and contents into the configuration build folder
located at the project root.