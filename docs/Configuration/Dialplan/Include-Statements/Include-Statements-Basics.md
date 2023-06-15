---
title: Include Statements Basics
pageid: 4817357
---

 

To set the stage for our explanation of include statements, let's say that we want to organize our dialplan and create a new context called **features**. We'll leave our extensions **6001** and **6002** for Alice and Bob in the **users** context, and place extensions such as **6500** in the new **features** context. When calls come into the users context and doesn't find a matching extension, the include statement tells Asterisk to also look in the new **features** context.

The syntax for an include statement is very simple. You simply write **include =>** and then the name of the context you'd like to include from the existing context. If we reorganize our dialplan to add a **features** context, it might look something like this:

[users]
include => features

exten => 6001,1,Dial(SIP/demo-alice,20)
 same => n,VoiceMail(6001@vm-demo,u)

exten => 6002,1,Dial(SIP/demo-bob,20)
 same => n,VoiceMail(6002@vm-demo,u)

[features]
exten => 6000,1,Answer(500)
 same => n,Playback(hello-world)
 same => n,Hangup()

exten => 6500,1,Answer(500)
 same => n,VoiceMailMain(@vm-demo)Location of Include StatementsPlease note that in the example above, we placed the include statement before extensions **6001** and **6002**. It could have just as well come after.  
The order in which Asterisk tries to find a matching extension is always current context first, then all the include statements.

Be careful with overlapping patterns/extensionsBecause Asterisk doesn't stop processing the dialplan after the first matching extension is found, always ensure that you don't have overlapping patterns or duplicate extensions among included contexts, or else you'll get an unexpected behavior.  
To prevent convoluted bugs it's recommended to end each extension with a Hangup call to explicitly exit the dialplan.

How calling 6001 may go wrong[users]
include => features
include => catchall

exten => 6001,1,Dial(SIP/demo-alice,20) ; <- Priority 1
 same => n,VoiceMail(6001@vm-demo,u) ; <- Priority 2

exten => 6002,1,Dial(SIP/demo-bob,20)
 same => n,VoiceMail(6002@vm-demo,u)

[features]
exten => 6000,1,Answer(500)
 same => n,Playback(hello-world)
 same => n,Hangup()
exten => 6500,1,Answer(500)
 same => n,VoiceMailMain(@vm-demo)
 
[catchall]
exten => \_.,1,NoOp();
exten => \_.,2,NoOp();
exten => \_.,3,NoOp(); ; <- Priority 3 ends up being here, which is NOT what you want