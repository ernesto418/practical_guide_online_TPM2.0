# Sign and verify

In this course, we will learn how to sign a message or a digest and verify it.

From the last course, we should have created our key with the [public portion](include_reference) obj.pub and the [private portion](include_reference) obj.priv.

To load a key into the TPM we need to provide the the parent key (the key that wrapped our key), the [public portion](include_reference) and the [private portion](include_reference) to the TPM.
then The TPM will be able to decrypted and load the private key in the TPM's volatile memory.

```
tpm2_load  -C primary.ctx -u obj.pub -r obj.priv -c key.ctx
```

If some time passed since you did the last course, may you need to regenerate the primary key before load the ECC-256 key. If you get an error, try with:


```
tpm2_createprimary -c primary.ctx
```

