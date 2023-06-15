---
title: Asterisk Architecture, The Big Picture
pageid: 4817479
---

Before we dive too far into the various types of modules, let's first take a step back and look at the overall architecture of Asterisk.

Asterisk a big program with many components, with complex relationships. To be able to use it, you don't have to know how everything relates in extreme detail. Below is a simplified diagram intended to illustrate the relationships of some major components to each other and to entities outside Asterisk. It is useful to understand how a component may relate to things outside Asterisk as Asterisk is not typically operating without some connectivity or interaction with other network devices or files on the local system.

An Asterisk System
==================

AsteriskArchitecture800

Remember this is not an exhaustive diagram. It covers only a few of the common relationships between certain components.

On this Page 2

Asterisk Architecture
=====================

Asterisk has a **core** that can interact with many **modules**. Modules called channel drivers provide **channels** that follow Asterisk **dialplan** to execute programmed behavior and facilitate communication between devices or programs outside Asterisk. Channels often use **bridging** infrastructure to interact with other channels. We'll describe some of these concepts in brief below.

The Core
--------

The heart of any Asterisk system is the **core**. The PBX core is the essential component that provides a lot of infrastructure. Among many functions it reads the configuration files, including dialplan and loads all the other **modules**, distinct components that provide more functionality.

The core loads and builds the dialplan, which is the logic of any Asterisk system. The dialplan contains a list of instructions that Asterisk should follow to know how to handle incoming and outgoing **calls** on the system.

Modules
-------

Other than functionality provided by the core of Asterisk, modules provide all other functionality. The source for many modules is distributed with Asterisk, though other modules may be available from community members or even businesses that make commercial modules. The modules distributed with Asterisk can be optionally be built when Asterisk is built.

Modules are not only optionally built, but you can affect at load-time whether they will be loaded at all, the loading order or even unload/load them during run-time. Most modules are independently configurable and have their own configuration files. Some modules have support for configuration to be read statically or dynamically(realtime) from database backends.

From a logistical standpoint, these modules are typically files with a **.so** file extension, which live in the Asterisk modules directory (which is typically **/usr/lib/asterisk/modules**). When Asterisk starts up, it loads these files and adds their functionality to the system.

Asterisk modules which are part of the core have a file name that look like **pbx\_xxxxx.so**. All of the modules types are discussed in the section .

A Plethora of ModulesTake just a minute and go look at the Asterisk modules directory on your system. You should find a wide variety of modules. A default installation of Asterisk has over one hundred fifty different modules!### A Few Module Examples

* **chan\_pjsip** uses **res\_pjsip** and many other res\_pjsip modules to provide a SIP stack for SIP devices to interact with Asterisk and with each other through Asterisk.
* **app\_voicemail** provides traditional PBX-type voicemail features.
* **app\_confbridge** provides conference bridges with many optional features.
* **res\_agi** provides the Asterisk Gateway Interface, an API that allows call control from external scripts and programs.

Calls and Channels
------------------

As was mentioned in the  section, the primary purpose of Asterisk is being an engine for building Real Time Communication systems and applications.

In most but not all cases this means you'll deal with the concept of "calls". Calls in telephony terminology typically refer to one phone communicating with (calling) another phone over a medium, such as a [PSTN](http://en.wikipedia.org/wiki/Public_switched_telephone_network) line. However in the case of Asterisk a call typically references one or more **channels** existing in Asterisk.

Here are some example "calls".

* A phone calling another phone through Asterisk.
* A phone calling many phones at once (for example, paging) through Asterisk.
* A phone calls an application or the reverse happens. e.g., app\_voicemail or app\_queue
* A local channel is created and interacts with an application or another channel.

Note that I primarily use phones as an example, however you could refer to any channel or group of channels as a call. It doesn't matter if the devices are phones or something else, like an alarm system sensor or garage door opener.

### Channels

 are created by Asterisk using . They can utilize other resources in the Asterisk system to facilitate various types of communication between one or more devices. Channels can be **bridged** to other channels and be affected by **applications** and **functions**. Channels can make use of many other resources provided by other modules or external libraries. For example SIP channels when passing audio will make use of the **codec** and **format** modules. Channels may interact with many different modules at once.

Dialplan
--------

Dialplan is the one main method of directing Asterisk behavior. Dialplan exists as text files (for example extensions.conf) either in the built-in dialplan scripting language, AEL or LUA formats. Alternatively dialplan could be read from a database, along with other module configuration. When writing dialplan, you will make heavy use of **applications** and **functions**to affect channels, configuration and features.

Dialplan can also call out through other interfaces such as AGI to receive call control instruction from external scripts and programs. The  section of the wiki goes into detail on the usage of dialplan.  
  


 
