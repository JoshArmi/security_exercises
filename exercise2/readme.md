# Setting up MFA

Go to `console.aws.amazon.com` and register for a new account

Once you're in the console, click on your name in the top right and click on `My Security Credentials`

Expand the `Multi-Factor authentication` section

Click `Activate MFA`

Pick `Virtual MFA Device`

If you don't have it already, install either `Google Authenticator` or `Microsoft Authenticator` on your phone

Click `Show QR Code`

Open your authenticator on your phone and scan the barcode

## Create a New User

It is not recommended practice to create access keys for the root user on your account, so we should create a less privileged user.

In the left menu, click `Users`

Then hit `Add User` and follow the wizard

Give the user `Administrator Access` so it will have all the powers we need for the rest of the course

## Log in as the New User

Click on your new user and click `Security Credentials`

At the top there will be a `console sign-in link`, copy that and log out of the account

Now browse to the url you copied

You should be able to log in with the user you just created

## Set MFA on the New User

Go through the same process as before to assign your phone as an MFA token against the user

## Get Access Keys for the New User

Under `My Security Credentials` hit `Create Access Key`

Download and open the CSV file

Install the AWS CLI for your system, follow the instructions [here](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)

Once installed, run: `aws configure`

Copy the access key and secret access key from the CSV as required by the prompts

For default region put `ap-southeast-2` and for default output format put `json`

Now open `~/.aws/config` and under the `[default]` block add a line
`mfa_serial=arn:aws:iam::<account-number>:mfa/<new-user-name>`

E.g. `mfa_serial=arn:aws:iam::613576916451:mfa/josh.armitage`

Now if you run `aws s3 ls` it should return without error
