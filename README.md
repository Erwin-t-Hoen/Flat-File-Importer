Name	
Flat & delimited file import

 Author	E. P. 't Hoen
 Type	Module
 Latest Version	v4.4.0
 Package filename	 FlatFileInterfacev4.4.0.mpk
 

Description
 

This module allows you to simplify interfacing with fixed length and delimited files (e.g. csv).

The module is completely configurable and will read files from the configured directories with the defined extension by means of a scheduler. The configuration is completely done from the run time after you have setup the entities that will be populated with the data from the files.

Starting at version 3 the module has been expanded with a queueing mechanism to allow for files that are stored in your Mendix app to be processed in the same way files from directories can be processed. To use the queueing mechanism just select the use internal queue option in the configuration and fill in the other fields. To import the files from the queue just add the file in the correct queue either by a function (microflow) or manually.

By using this module you will be able to create interfaces with delimited and fixed length files by configuration and not be bothered with the java work involved.

Typical usage scenario
 

An external system will provide data in a delimited or fixed length file and you need to import this data into your application. After setting up the configuration for reading the files and importing them to your entities you can control the possible further flow of the data with modelling only.
Let say you have a delimited file that contains person information with organisations and locations in the file and you modelled your data model into 3 entities with relations: 
Organisation
Person (with relation to Organisation and Location)
Location
You could create an interface entity that will receive the data from the file and trigger functionality on this interface table to create the different records in the Organisation, Person and Location entities together with their relations.
 

Features and limitations
 

Import delimited files.
Import fixed length files.
Skip columns.
Reuse columns.
Schedule imports on intervals.
Multiple configurations.
Complete logging.
File renaming.
Test imports.
Supported data types:
String
Integer
Long
Float
Currency
Enumeration
Boolean
Date and Time
Unsupported date types:
Autonumber
Binary
Hash string
Supported languages:
English
Dutch
Installation
 

First install the MxModelReflection module then install this module.

Dependencies
 

Mendix 5.13.1 Environment
MxModel Reflection module
Configuration
Setup:

1) Add the forms in the #Implementation folder to your navigation

2) Add module permissions to a user role in Project --> Security

3) Create your destination tables (optional)

4) Define how long queued files are retained in the queue (default is 1 day) with the FileQueueAge constant.
5) Synchronize objects in the MxModelReflection module
6) Setup the configuration(s)

In the module # implementation folder there is a constant that will provide this information (the constant can be deleted after implementation!)


Configure:

The configuration is done from the InterfaceDefinition_Overview form.

Create a new definition by filling out the following fields:

General

Use internal queue: Select this option if you're placing the files from your app directly in one of the queues

Interface name: The name identifing the interface

Import directory: The directory used for this configuration ( Make sure the Mendix server has access to the directory in the cloud or when using emulate cloud security)

Queue number: The number for the queue that you place your files in (for example queue number 0 will contain files from interface 1 and queue1 will contain the files from another interface)

File Extension: The extension of the files to be read from the import directory (e.g. csv) without the '.' character

File Type:  The type of data in the files (fixedlength/delimited)

Delimiter: If file type is delimited the delimiting character is required. When your delimiter is a regex special characters then escape the character with a backslash.

Text Qualifier: If your delimited file contains text qualifiers you can select them here (quote or double quote)

Select Entity: Select the entity that serves as the destination for the data from the file(s)

Skip first row: If set to true the first row of the import file will be skipped

Trigger Events: If set to true the submit of data in the destination entity will trigger events when defined

 

Now it's time to define your columns, to do this save your configuration.

Columns:

When using delimited files the following fields will be need configuration:

Select Attribute:  Select the attribute from the destination entity for the column (The number is generated automatically)

Date Time Format: If the selected attribute is of type Datetime then a date time format is required (The default is dd-MM-yyyy HH:mm:ss)

Skip Column:  If the column should be skipped select true

 

When using delimited files the following fields need configuration:

Start Position: The position where the column starts (a line in the file starts at position 0)

Length: The length of the column to read (e.g. to read the first column existing of 5 positions enter 0 at start position and length= 5)

Select Attribute:   Select the attribute from the destination entity for the column  (The number is generated automatically)

Date Time Format:  If the selected attribute is of type Datetime then a date time format is required  (The default is dd-MM-yyyy HH:mm:ss)

 

  Known Bugs

 

None
 

Frequently Asked Questions
 

Ask your question at the Mendix Community Forum

 

Can't access the files the module returns an error means that permissions for your application have not been setup correctly for the directories that you are using (e.g. on local deployment you have emulate cloud security set to true)
Unable to select an attribute from my destination entity means that either you need to run the sync from the MxModelReflection module or you are trying to map an unsupported data type.
Be careful setting the FFI Import logging to the debug or trace level as a large amount of logging is created when using large files.
The loglevel field on the configuration screen allows you to measure the time the import takes when setting this field to info.(default the loglevel is error)