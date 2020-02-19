# Using Symmetric Key Encryption

## Blowfish Example

### Decide On A Key

First we need to decide on a key

Any string will do, as long as it is between 4 and 56 characters, e.g. "mysecretkey", "slartibartfast", "ThisIsSecure1"

Then pick a file on your laptop, and open a terminal session in the folder

Now we can encrypt the file:

`docker run josharmi/blowfish:stable -v ${PWD}:. encrypt <key> <file>`

For example: `docker run josharmi/blowfish:stable -v ${PWD}:. encrypt mysecretkey secret.txt`

Now if you look in the folder, we can see a new file called **encrypted**

Now we can decrypt the file:

`docker run josharmi/blowfish:stable -v ${PWD}:. decrypt <key>`

For example: `docker run josharmi/blowfish:stable -v ${PWD}:. encrypt mysecretkey`

Now you should have a file called **decrypted** in the folder, and if you open it, the contents should be the same as the file you encrypted in the first place

## Questions

Do you think symmetric key has value?

What are the good points of symmetric keys?

What do you think the main issues with symmetric keys are?
