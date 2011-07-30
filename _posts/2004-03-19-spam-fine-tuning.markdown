--- 
layout: post
title: Spam fine tuning
published: true
meta: {}

tags: 
- Columba
type: post
status: publish
---
Waking up this morning I recognized that I've forgotten something really important for the token database.I'm using MD5 sums to detect already learned messages. These shouldn't be added to the token database twice or more times - just once. When marking messages as spam, which are non spam, Columba should use this MD5 sum to correct the value, if the user marks the message as non spam.You can imagine that this MD5 message list is getting hugh over time. So, I've added a timestamp for every MD5 sum. This way, cleaning up the token database is very easy because I can just remove old messages.Sadly, this means that the spam.db file format has changed a bit.Another new thing are handcrafted rules. This is pretty new, I've got it from a paper "A bayesian approach to filtering junk email" from Sahami et. al.In Columba I've basically eliminated the need for something like a training mode with these rules. The idea is pretty easy. The bayesian classifier enables us to add handcrafted rules, which are handled equally to other tokens. So, a rule just adds another probability additionally to the word probabilities.When starting from scratch you don't have any words in your token database. The spam engine can't detect spam messages until you trained it for some time. But the handcrafted rules are in place, giving you a good start to determine spam messages, until you have collected enough tokens using the message contents.One such rule would be for example checking if the Subject is of capital letters only, or if the Subject contains many whitespaces. You can think if many more rules. Spamassassin has a very big list of rules which can be found [here](http://www.spamassassin.org/tests.html).   Adding new rules is very easy. Just subclass AbstractRule in org.columba.mail.spam.rules.
