---
title: API reference
redirect_from: /RPC_API_reference
---
## Status of this document
The document describes methods available in NZBGet **version 13.0 and later**. The document is updated after a new stable version is out. Current testing version may have methods or fields not described here; these methods or fields may change before getting stable.

## Protocols
NZBGet supports XML-RPC, JSON-RPC and JSON-P-RPC. RPC-protocols allow to control the program from other applications. Many programming languages have libraries, which make the usage of XML-RPC, JSON-RPC and JSON-P-RPC very simple. The implementations of all three protocols in NZBGet are equal: all methods are identical. You may choose the protocol, which is better supported by your programming language. 

## Authentication
NZBGet has three pairs of username/password:
- options **ControlUsername** and **ControlPassword** - Full access, usually used when connecting to web-interface;
- options **RestrictedUsername** and **RestrictedPassword** - **`v15.0`** Restricted user can control the program with few restrictions. He has access to web-interface and can see most program settings. He can not change program settings and can not view security related options or options provided by extension scripts. In terms of RPC-API the user:
  - cannot use method "saveconfig";
  - methods "config" and "loadconfig" return string "***" for options those content is protected from the user.
- options **AddUsername** and **AddPassword** - **`v15.0`** This user has only two permissions:
  - add new downloads using RPC-method "append";
  - check program version using RPC-method "version".

The RPC server uses HTTP basic authentication. The URL would be something like:
 - for XML-RPC: `http://username:password@localhost:6789/xmlrpc`
 - for JSON-RPC: `http://username:password@localhost:6789/jsonrpc`
 - for JSON-P-RPC: `http://username:password@localhost:6789/jsonprpc`

If HTTP basic authentication is somewhat problematic the username/password can also be passed in the URL as the first part of the path:
 - `http://localhost:6789/username:password/xmlrpc`

**Security warning:** HTTP authentication is not secure. Although the password is encoded using Base64 it is not encrypted. For secure communication use HTTPS (needs to be explicitly enabled in NZBGet settings by user).

## Features and limitations
 - Multicalls are supported for XML-RPC and not supported for JSON-RPC.
 - Introspection is not supported.
 - Only positional parameters are supported in JSON-RPC. Named parameters are not supported. Therefore parameter names are ignored but the order of parameters is important. All parameters are mandatory.
 - Each call to JSON-P-RPC has one additional parameter - the name of callback-function. This parameter must be named "callback" and must be passed first (before any other parameters).
 - 64 bit integers are returned in two separate fields ''Hi'' and ''Lo'' (for example ''FileSizeHi'' and ''FileSizeLo''). These fields are unsigned 32 bit integers. Although dynamic languages such as PHP or Python have no problems with these fields the XML-RPC specification does not allow unsigned integers. This may cause troubles in statically typed languages such as Java or C++ if XML-RPC-parser expects only signed integers in 32 bit range. As a solution use JSON-RPC instead (which does allow unsigned integers) instead of XML-RPC.

## Program control
- [version](version)
- [shutdown](shutdown)
- [reload](reload)

## Queue and history
- [listgroups](listgroups)
- [listfiles](listfiles)
- [history](history)
- [append](append)
- [editqueue](editqueue)
- [scan](scan)

## Status, logging and statistics
- [status](status)
- [log](log)
- [writelog](writelog)
- [loadlog](loadlog)
- [servervolumes](servervolumes)
- [resetservervolume](resetservervolume)

## Pause and speed limit
- [rate](rate)
- [pausedownload](pausedownload)
- [resumedownload](resumedownload)
- [pausepost](pausepost)
- [resumepost](resumepost)
- [pausescan](pausescan)
- [resumescan](resumescan)
- [scheduleresume](scheduleresume)

## Configuration
- [config](config)
- [loadconfig](loadconfig)
- [saveconfig](saveconfig)
- [configtemplates](configtemplates)