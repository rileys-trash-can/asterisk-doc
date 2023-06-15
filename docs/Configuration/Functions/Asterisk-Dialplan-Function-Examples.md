---
title: Asterisk Dialplan Function Examples
pageid: 36215464
---

UNDER CONSTRUCTION

 

Function Examples
=================

Asterisk includes a wide variety of functions. Here we'll show you a few commonly used functions and a selection of others to give you an idea of what you can do.

CHANNEL and CHANNELS
--------------------

CHANNEL Gets or sets various pieces of information about the channel. Additional arguments may be available from the channel driver; see its documentation for details. Any item requested that is not available on the current channel will return an empty string. CHANNELS on the other hand, gets the list of channels while optionally filtering by a regular expression (provided via argument). If no argument is provided, all known channels are returned. The regular\_expression must correspond to the POSIX.2 specification, as shown in regex(7). The list returned will be space-delimited.

See the CHANNEL function reference documentation for an extensive list of arguments.

**Examples:**

Push a hangup handler subroutine onto the channel. The hangup handler must exist at the location specified (default,s,1).

same = n,Set(CHANNEL(hangup\_handler\_push)=default,s,1)Using the CHANNEL function along with the Log application, we can log the current state of the channel.

same = n,Log(NOTICE, This channel is: ${CHANNEL(state)})Set the channel variable myvar to a space-delimited list of all channels.

same = n,Set(myvar=${CHANNELS}) 

DB and other DB functions
-------------------------

The DB function will read from or write a value to the . On a read, this function returns the corresponding value from the database, or blank if it does not exist. Reading a database value will also set the variable DB\_RESULT. There are a few related functions. DB\_EXISTS, DB\_DELETE and DB\_KEYS.

If you wish to find out if an entry exists, use the DB\_EXISTS function. The DB\_DELETE function will retrieve a value from the Asterisk database and then remove that key from the database. DB\_RESULT will be set to the key's value if it exists. Finally, the DB\_KEYS will return a comma-separated list of keys existing at the prefix specified within the Asterisk database. If no argument is provided, then a list of key families will be returned.

**Examples:**

Set the key "testkey" in family "testfamily" to the value "Alice".

same = n,Set(DB(testfamily/testkey)=Alice)Dialing a PJSIP endpoint using the value of the previously set key as the endpoint name.

same = n,Dial(PJSIP/${DB(testfamily/testkey)})Go to a specific dialplan location (via label) depending on if the key exists or does not.

same = n,Gotoif($[${DB\_EXISTS(testfamily/testkey)}]?keyexists:keydoesnotexist)Delete the entry while logging the value of the key!

same = n,Log(NOTICE, Deleting the key testfamily/testkey which had the value: ${DB\_DELETE(testfamily/testkey)})