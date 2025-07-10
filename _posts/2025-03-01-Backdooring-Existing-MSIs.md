---
layout: post
title: "Backdooring Existing MSIs"
date: 2025-01-04 19:43:00 +0300
categories: [MSI,PROGRAMMING]
tags: 
pin: false
description: How attackers embed malicious binaries in legitimate Microsoft Windows Installation (.msi). 
toc: true
image: /assets/img/MSI/msi.png
---


# Introduction

This chapter explains the fundamentals of Windows Installer packages (MSI).

## Overview

MSI, formerly known as Microsoft Installer, is a Windows installer package format. MSI allows for the installation and deployment of necessary Windows applications or packages to end-users’ machines. MSI is a standardized installation method that simplifies the installation process for users. The installation process with MSI files is simple and often does not require much user interaction. Usually, the installation process with MSI is similar to running an executable. 

Simple `wxs` file:

```wxs
<?xml version="1.0" encoding="UTF-8"?>
<Wix xmlns="http://schemas.microsoft.com/wix/2006/wi">
  <Product Id="*" Name="ExampleApp" Language="1033" Version="1.0.0.0" Manufacturer="ExampleCorp" UpgradeCode="PUT-GUID-HERE">
    <Package InstallerVersion="500" Compressed="yes" InstallScope="perMachine" />

    <MediaTemplate />

    <Directory Id="TARGETDIR" Name="SourceDir">
      <Directory Id="ProgramFilesFolder">
        <Directory Id="INSTALLFOLDER" Name="ExampleApp"/>
      </Directory>
    </Directory>

    <ComponentGroup Id="ProductComponents" Directory="INSTALLFOLDER">
      <Component Id="MaliciousPayload" Guid="PUT-GUID-HERE">
        <!-- This file could be any executable, e.g., a backdoor EXE -->
        <File Id="DroppedExe" Name="backdoor.exe" Source="backdoor.exe" KeyPath="yes" />
        <!-- Auto-execution via CustomAction and InstallExecuteSequence -->
      </Component>
    </ComponentGroup>

    <CustomAction Id="RunPayload" FileKey="DroppedExe" ExeCommand="" Execute="deferred" Return="ignore" />

    <InstallExecuteSequence>
      <Custom Action="RunPayload" After="InstallFiles">NOT Installed</Custom>
    </InstallExecuteSequence>

    <Feature Id="ProductFeature" Title="ExampleApp" Level="1">
      <ComponentGroupRef Id="ProductComponents" />
    </Feature>
  </Product>
</Wix>
```

MSI utilizes Microsoft’s technology COM Structured Storage, which allows an MSI file to have a file system structure. This technology is also referred to as OLE documents, components often used by Microsoft Office. This technology is constructed with two types of objects called storage and streams, which are conceptually similar to directories and files. This structure allows MSI components to store multiple files to allow for easy access.


The ability to store and deploy necessary files with minimal user action makes this technology similar to self-extracting zip files, which are often in the form of executables.  

The installation procedure also allows execution with a LocalSystem account (NT Authority\System). The elevated privilege behavior as well as the COM Structure makes MSI an attractive malware deployment method for threat actors.  



# Structure

MSI contains a main stream called a **Database stream**. This section provides an overview of the Database stream and its relevant components.

## Database

The Database stream consists of tables that form a relational database. These tables are linked through primary and foreign key values. This structure allows MSI to maintain relationships between different tables and store the information required to perform the installation.

The information within each table plays an important role in the installation process. For example, some tables store information about installing software applications:

* **File**
  Metadata of application files to be installed.

* **Registry**
  Registry key values for the installing applications.

* **Shortcut**
  Shortcuts related to the installed applications.

Other tables contain information about the package executions and execution sequences. These tables are essential for MSI to determine the execution flow and the specific actions required to complete the installation. The installation logic is often defined and manipulated using *Actions* and *Sequences*.

## Actions

MSI contains a set of subroutines to execute installation tasks. These routines are divided into two types:

* **Standard Actions**
  Built-in actions that perform core installation steps. Notable examples include:

  * Initializing installation directories
  * Dropping files into installation directories
  * Adding registry values for installed software

* **Custom Actions**
  When Standard Actions are insufficient, Custom Actions allow developers to embed additional logic by executing binaries or scripts during installation. Common examples include:

  * Executing executables stored within the MSI file
  * Running specific exported functions from DLLs
  * Running JavaScript or Visual Basic scripts

Actions can be further customized using **Properties**, which function similarly to environment variables. Properties contain data and control parameters needed for certain actions, such as:

* A flag indicating whether a reboot is required
* Installation directory path
* System information
* Control information for the installation

The **Property table** exists within the Database Stream, but not all properties are stored there. Some properties are set dynamically at runtime during specific actions, so they may not appear in the Property table.

## Sequences

Developers can define the order in which Standard Actions and Custom Actions are executed using the **Sequence table**. Within this table, conditions can also be assigned to control when actions run.

There are two main types of sequences:

* **Install UI Sequence**
  The sequence of actions responsible for the installation user interface, including user dialogs and interactions.

* **Install Execute Sequence**
  The sequence of actions for the actual installation, such as copying files and creating registry entries. During a silent installation, the Install UI Sequence is ignored, and only the Install Execute Sequence is processed.

# Backdooring Existing MSI
