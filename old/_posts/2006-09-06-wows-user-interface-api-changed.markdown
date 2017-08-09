---
author: admin
comments: true
date: 2006-09-06 15:00:52+00:00
layout: post
link: http://www.seerhut.cn/2006/09/06/wows-user-interface-api-changed/
slug: wows-user-interface-api-changed
title: WoW's User Interface API changed
wordpress_id: 54
---

from  **[2.0.0 Changes - Concise List](http://forums.worldofwarcraft.com/thread.html?topicId=15401595&sid=1)**









  *  09/02/2006 03:12:11 AM UTC





          














This is a consolidated list of the announced (and sometimes observed) changes in the User Interface API's and functionality for the Burning Crusade expansion (2.0) release. Please note that this thread is to discuss the upcoming changes and any clarifications or features that are a direct result of those changes, or things which we've been asked to remind slouken of.





<!-- more -->
IMPORTANT: Off-topic or entirely redundant posts are liable to get deleted. It is however in everyone's best interest to not post them in the first place - We'd rather slouken could spend his time coding us cool things than moderating this thread!

**Significant Changes**
* The expansion will be using Lua version 5.1.1, which provides a number of useful features, most notably incremental garbage collection and memory-efficient variable arguments. There ARE some incompatible changes with Lua 5.0 and authors are advised to familiarize themselves with [http://www.lua.org/manual/5.1](http://www.lua.org/manual/5.1)

**Slash Commands**
The following new slash commands will be available:
* Targetting: /targetlasttarget
* Items and equipment: /use , /equip , /equipslot  
* Pet control: /petattack, /petfollow, /petstay, /petpassive, /petdefensive, /petaggressive

**Spell Casting**
* When casting a ranked buff spell that's too high a level for a friendly target the game will automatically use the highest appropriate rank of the spell instead.

**Actions**
* The old CURRENT_ACTIONBAR_PAGE variable is no longer used, the ChangeActionBarPage(page) function now takes the new page as its argument, and GetActionBarPage() returns the current page number.

**Unit Function Changes**
* The GetSlashCmdTarget() UI function is no longer necessary. All the functions that required it now properly interpret the unit tokens ("player", "pet", etc.) in addition to names, and an empty string defaults to your current target.

**Key Bindings**
* You will be able to bind keys directly to spells, using the scripting interface, e.g. /script SetBinding("[", "Holy Light")

**Bug Fixes**
* Fixed missing CHAT_MSG_SPELL_SELF_BUFF messages (Opening, Mining, Gathering)

_Last updated 2006-09-04 19:11 pacific_
[ Post edited by Iriel ]




* * *

**UI and Macros Forum MVP** - Understand GC!



