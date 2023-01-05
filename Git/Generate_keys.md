# Steps to Generate Ssh keys for different platforms and to your Github account
 
## For Windows

* Step 1: Open "cmd" and then execute the below command

```bash
ssh-keygen -o -t rsa -C "<email-id>"
```

* Step 2 : Go to your Github account and go to settings , select the "SSH and GPG keys" option and Click on the "New SSH key" button.

* Step 3: In the new window which pops up give an appropriate title for ssh key such as "My IdeaPad 5 Laptop" so that it is easier to identify it in the future in case we want to update/delete it.

* Step 4: Copy the public key generated from step 1 from the location it was generated to and paste it to the "Key" Text area and Click on the "Add SSH key" button