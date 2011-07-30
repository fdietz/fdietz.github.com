--- 
layout: post
title: Spam filter integrated in Columba
published: true
meta: {}

tags: 
- Columba
type: post
status: publish
---
Checking the latest CVS sources from [Columba](http://columba.sf.net) you will notice the new integrated spam solution. This work is based on my bachelor dissertation. An introduction can be read at [http://frederikdietz.de](http://frederikdietz.de/jbayesian.php). The thesis paper can be downloaded as [pdf file](http://frederikdietz.de/files/thesis.pdf).The spam filter can be enabled in the account configuration. Note, that it takes some time to train this filter. This is done in marking messages as spam or not spam.In the first few days its recommended to just mark the messages as spam. Do not make them move to trash automatically, as its high likely that you will get some false-positives. Also, use the addressbook checking option, which prevents marking messages as spam from people which are already in your addressbook.Its recommended to delete:<pre>[your-config-folder]/mail/main_toolbar.xml</pre>This will create a new toolbar for you, with a mark message as spam button added.In my personal experience, learning around 1000 messages should be enough to make it perform well.There's going to be some more fine-tuning needed, especially to make it perform better in the beginning - you have been warned!So, please help me test this beast ;-)
