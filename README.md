# plexmap
IMAP Multiplexing Proxy (POC Python)

The goal of this project is to create a solution where distributed IMAP servers are merged into a single virtual IMAP mailbox - also distributed.

Example scenario:

Mail comes into three SMTP servers and stored in local IMAP mailboxes for a specific user:

```
                      /-> (SMTP/IMAP) mail-a.stitch.com 42 messages
(SMTP) lilo@stitch.com -> (SMTP/IMAP) mail-b.stitch.com 18 messages
                      \-> (SMTP/IMAP) mail-c.stitch.com 25 messages
                      
```

When user lilo@stitch.com wants to check their mail using IMAP and the system administrator of their mail system is super cheap and super lazy they can take advantage of plexmap.  Thankfully the system administrator has already installed plexmap on each of the mail servers.

```
(IMAP) lilo@stitch.com -> (PLEXMAP/IMAP) mail-?.stitch.com 85 messages
                                          \-> (IMAP) mail-a.stitch.com 42 messages 
                                          \-> (IMAP) mail-b.stitch.com 18 messages
                                          \-> (IMAP) mail-c.stitch.com 25 messages                                                      
```

Through the magic of IMAP client IDs lilo should see a merged and sorted IMAPv4 connected mailbox containing all messages and folders from all backend IMAP servers.

Connecting to any of the mail servers above will always result in the same virtual multiplexed mailbox.

What if one of the servers goes down?  Better get it back up.  Ideally mail-a/mail-b/mail-c will have a hot backup that will take over.

I could be coerced into forcing some sort of mirroring that will attempt to always make each server hold the same message set... but I don't want to.

I'm sure you know of some gotchas.  Let me know.  I know quite a few in my way already.
