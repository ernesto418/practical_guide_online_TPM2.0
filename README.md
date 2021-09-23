# Hello student!

In this set of tutorials, we will focus just on "doing" things with the TPM, explaining all the theory found in the way. But remember,  never try to do some real application until you completely know what you are doing.


In this series of practical and explained exercises, you will learn how to use the TPM from creating private keys to create some complex policies like getting access to your secret word just if you have a smartcard, you know your password, and you were using you computer for less than 2 hours.

The first thing you have to do is to set up your work environment. We will use this library as reference https://github.com/tpm2-software/tpm2-tools, with the commit 4bcbebc852d9849c9f1784880db2accc2eb1a3bf

Follow the next guide, https://github.com/tpm2dev/tpm.dev.docker and when you are able to execute the next command, come back here:

```
tpm2_getrandom 8
```

You did it? Nice! Then let' start!

Start with the first [course](Create_keys.md)!
