--- 
layout: post
title: Folder and Filter TestSuite complete
published: true
meta: {}

tags: 
- Columba
type: post
status: publish
---
My exams currently keep me pretty busy until next week. Right now, I'm totally into DB2 - the second lecture on database, not IBM's database - focusing on Object-Oriented and XML databases. Not very exciting, though...So, finally some news on Columba development. After starting a bigger refactoring of the folder packages in the mail module, I've finished with a testsuite for all types of folders. You can add every foldertype you want and the testcases run over it showing you problems. This should become pretty handy when adding new folders, including new local mailbox formats or even database backed folder implementations.The Filter testsuite covers all plugin-based filters which are shipped by default with Columba. This is more important than you would actually think because these are also the default fallbacks, used in case a search-engine doesn't implement all operations. So, implementors of search-engines can just start hacking on their search-engine adding more optimized operations, and falling back to the default engine in case.
