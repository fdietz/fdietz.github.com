--- 
layout: post
title: Interesting new stuff in upcoming RC2
published: true
meta: {}

tags: 
- Columba
type: post
status: publish
---
New Features:- international IMAP mailbox naming using UTF-7 encoding/decoding (tstich)
- added show next/previous message and unread message actions (fdietz)
- #999257, show/hide message preview panel (fdietz)
- offer three choices for viewing email headers: default, custom, all (fdietz)
- #999871, "subject or sender contains" should do filtering on typing (fdietz)
- show recent message count in folder tree using blue font (fdietz)
- #999748, in preview panel,full emailadress should be displayed beside (fdietz)
- added complete sorting and filtering support for contact JTable (fdietz)
- speedup of IMAP mailbox selection and auto-checking (tstich)
- set Columba to default-email client on win32 using jniwrapper (tstich) 
- added jpim library for vCard import/export, not complete yet (fdietz)
The #numbers are actually Feature Requests which can be found in our [tracker](http://sourceforge.net/tracker/?atid=371217&group_id=21217&func=browse). We try hard to start using this tool as an official feature plan for developers.  New developers should also look into our [wiki](http://columba.sourceforge.net/phpwiki/index.php/UnassignedProgrammingTasks).What's most notably for developers is a much cleaner separation of mail and addressbook code, introducing an interface-only API. This way components can use each others functionality but can be compiled against the API safely.Another bigger change is the removal of all interfaces like MainInterface, MailInterface and AddressbookInterface. We now use singleton classes instead which are instanciated lazy on demand. This makes things a lot easier because you don't have to manually assure that classes are instanciated in the correct order. This further simplifies writing testcases.Also, we introduced a clean concept of a container which embeeds a component. This way you can switch between mail and addressbook components  in the same JFrame.
