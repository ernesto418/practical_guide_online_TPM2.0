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

Before sing a message, we need to create a message:
```
echo "Hello world!, signed my private ECC-256 key" > message.dat
```

Now finally is time to sign:

```
tpm2_sign -c ECC-256.ctx -g sha256 -o signature.der message.dat
```

Now is time to know what we exactly did with this signature:

**-c** The context of the key that we are going to use for signing. As it is already loaded in the TPM's V-memory we don't have to provide the private or the public portion of the key 

**-g** It is the algorithm used for hashing the data, [here](https://github.com/tpm2-software/tpm2-tools/blob/master/man/common/alg.md#hashing-algorithms) you can find the different specifiers.

**-o** path where store the signature.

**Argument** Path with the data to sing

