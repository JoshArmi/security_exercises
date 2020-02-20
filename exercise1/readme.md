# Using Symmetric Key Encryption

## Blowfish Example

### Decide On A Key

First we need to decide on a key

Any string will do, as long as it is between 4 and 56 characters, e.g. "mysecretkey", "slartibartfast", "ThisIsSecure1"

Then pick a file on your laptop, and open a terminal session in the folder

Copy the `Dockerfile` file down to the folder

Run `docker build -t exercise1 .`

Now we can encrypt the file:

Run `docker run -it exercise1`

Let's create a file by `echo <your-file-content> >> file`

`aescrypt -e -p <password> file` (Pick any password you want)

Now if you look in the folder, we can see a new file called `file.aes`

If you look at the contents by running `cat file.aes` it should be garbage

Now let's delete the original file

`rm file`

Now we can decrypt the file:

`aescrypt -d -p <password> file`

Now if we run `cat file` we should see the original file

## Questions

Do you think symmetric key has value?

What are the good points of symmetric keys?

What do you think the main issues with symmetric keys are?
