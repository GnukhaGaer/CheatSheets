# Using SourceTree to Access Gitlab

If you want to make the switch to GitLab from GitHub, or BitBucket Cloud or Server,
the way you need to use SourceTree is a little unintuitive as I discovered the other day.

There is no Account profile you can use to specify your GitLab credentials, so how do you connect?

## How To Make The Switch?

The basic process to follow is outlined below:
- Generate an SSH Public Key.
- Apply the key to your GitLab user profile.
- Use the **Clone from URL** SourceTree feature to check out repositories from SourceTree using SSH.

## Generate an SSH Key

GitLab offers SSH as a method to handle authentication between the master and remote repositories. 
The advantage of SSH is that you do not need to provide a User Name or Password when you commit work.

To push content to GitLab using SourceTree, you will need to generate a SSH Public Key using the command-line 
on your Windows, Mac, or Linux PC. The following procedures and references will help you achieve this.

1. Log onto GitLab.
2. In the left pane, click **Profile Settings**.
3. Click **SSH Keys**.

## Retrieve an Existing Public Key

You’ll notice that the **SSH Keys** page displays a banner suggesting you create a SSH Key Pair.

The SSH protocol handles all aspects of verifying who you are using Public Key Cryptography. 
It is a very secure method of authentication, which is why many folks choose it as the only 
way they interact with GitLab, and other Version Control Systems (VCS).

If you have set up SSH for GitHub or another repository before, you can run either of these commands 
in a terminal to retrieve your public key and enter this into the **SSH Keys** screen:

- Windows

```bash
type %userprofile%\.ssh\id_rsa.pub`
```

- Mac or Linux

```bash
cat ~/.ssh/id_rsa.pub`
```

## Generate a SSH Key Pair

If you do not get a key after issuing either of these commands, read on for instructions.

Create a SSH Key Pair:

1. Load http://doc.gitlab.com/ce/ssh/README.html in your browser.
2. Read the instructions described on this page, and use the commands specified to generate the required key pair.
	
## Apply the Key Pair to your User Profile

Apply the Key to Your Profile:

1. Once you have a public key, click `Add SSH Key`.
2. Paste the key into the Key field.
3. Click `Add key` to set the SSH key.

## Clone GitLab Repositories in SourceTree

Because SourceTree does not have a specific protocol defined for GitLab in it’s Accounts tab, 
clicking Remotes and selecting from the list of known repositories will not work.

You need to change your approach when cloning repositories from GitLab to the **Clone from URL** method.

How to Clone a GitLab Repository using SourceTree

1. Load https://gitlab.com in your browser.
2. Navigate to the GitLab repository you want to clone.
3. Copy the **SSH** path shown in the screen.
4. In SourceTree, click menu:New Repository[Clone from URL].
5. Paste the copied SSH path into the **Source URL** field.
6. Specify the **Destination Path** and **Name** as you have done previously.
7. Click **Clone**.
