# Sign and verify

In this course, we will learn how to sign a message or a digest and verify it.

From the last course, we should have created our key with the [public portion](include_reference) obj.pub and the [private portion](include_reference) obj.priv.

To load a key into the TPM we need to provide the parent key (the key that wrapped our key), the [public portion](include_reference), and the [private portion](include_reference) to the TPM.
then The TPM will be able to decrypt and load the private key in the TPM's volatile memory.

```
tpm2_load  -C primary.ctx -u obj.pub -r obj.priv -c ECC-256.ctx
```

If some time passed since you did the last course, you may need to regenerate the primary key before load the ECC-256 key. If you get an error, try with this command before load the key:

```
tpm2_createprimary -c primary.ctx
```

Before signing a message, we need to create a message:
```
echo "Hello world!, signed my private ECC-256 key" > message
```

Now finally is time to sign:

```
tpm2_sign -c ECC-256.ctx -g sha256 -o signature.tss message
```

Now is time to know what we exactly did with this signature:

**-c** The context of the key that we are going to use for signing. As it is already loaded in the TPM's V-memory we don't have to provide the private or the public portion of the key 

**-g** It is the algorithm used for hashing the data, [here](https://github.com/tpm2-software/tpm2-tools/blob/master/man/common/alg.md#hashing-algorithms) you can find the different specifiers.

**-o** path where store the signature.

**Argument** Path with the data to sign.

## Now let's verify it:

To verify it we just need to provide the key ctx used (**-c**), the hash used (**-g**), the signature (**-s**) and the message (**-m**).

```
tpm2_verifysignature -c ECC-256.ctx -g sha256 -s signature.tss -m message
```

If you do not recieve a error, the verification was great!. Now we are going to edit the message and try again the verrification.

```
echo "I hate you, world!, signed my private ECC-256 key" > message_fake
tpm2_verifysignature -c ECC-256.ctx -g sha256 -s signature.tss -m message_fake
```

You should recieved (beetwen other things):

```
ERROR: Esys_VerifySignature(0x2DB) - tpm:parameter(2):the signature is not valid
ERROR: Verify signature failed!
```

Nice!, but... what if I do not want to verify it in a TPM? maybe I want to use a commun sofware like Openssl? 

## Verify with Openssl

There is no much complication to verify a signature with Openssl, just type "verify signature with Openssl". The problem is that the output we have from the TPM taht are required to verify the signatures are not in the correct format to be used in Openssl. Becasue of that we need to aks the TPM to provide as the public key of ECC-256 in a PEM format and the signature in DER format, and then Openssl will be able to verify it.

Let's start with the public key:

```
tpm2_print -t TPM2B_PUBLIC -f pem obj.pub >> ECC-256_PuB.pem
```

[Note: current command does not work properly, issue open [here](https://github.com/tpm2-software/tpm2-tools/issues/2840)]

And now, get our signature in der format, we can specify the format of the public key with **-f** and select the [specifier](https://github.com/tpm2-software/tpm2-tools/blob/master/man/common/signature.md) we prefer:

```
tpm2_sign -c ECC-256.ctx -g sha256 -f plain -o signature.der message
```

And now, we can verify it wit Openssl:

```
openssl dgst -verify ECC-256_PuB.pem -keyform pem -sha256 -signature signature.der message
```


In the next [course](insert_reference) we will sign data providing just the digest and we will introduce the first attribute, [restricted](insert_reference)
