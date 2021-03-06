# Using a Mac & Packet

## One time environment setup

1. Make a [Packet account](https://app.packet.net/#/registration/) (or reuse an existing one)
2. Make a [new Project](https://app.packet.net/portal#/projects/new).
   * Projects are tied to a payment method, so you may choose to use an existing one, just be aware that you shouldn't use the convenient `./provision packet delete --all` to decommission a cluster as it may remove other instances you care about it.
3. Open a Terminal session on your mac
4. <table><tr><td>`mkdir ~/kismatic` <br/>
   `cd ~/kismatic`</td> 
   <td>*Make a new directory for Kismatic (~/kismatic would work) and make it the working directory*</td></tr></table>
5. <table><tr><td>`wget -O - https://kismatic-installer.s3-accelerate.amazonaws.com/latest-darwin/kismatic.tar.gz | tar -zx`</td> 
   <td> *Download & unpack Kismatic*</td></tr></table>
6. Make (or re-use) a Packet API key https://app.packet.net/portal#/api-keys
7. <table><tr><td>`export PACKET_API_KEY=YOURAPIKEY`</td><td> *Set your new API key in an environment variable*</td></tr></table>
8. You need to grab your project's ID. It will be part of the URL when you browse your project, and look like this: ```12345678-1234-5678-90ab-cdef12345678```
9. `export PACKET_PROJECT_ID=YOURPROJECTID`
10. <table><tr><td>`ssh-keygen -t rsa -f kismatic-packet.pem -N ""`</td><td> *Build a new keypair for use in connecting to machines. You may also use an existing key pair by exporting PACKET_SSH_KEY_PATH=the absolute path to your private key*</td></tr></table>
11. `cat kismatic-packet.pem.pub`
12. Select your entire public key into your clipboard. A key will look like this: ```ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDRrxKFLVQSLNTsjrtjhmLqehZ1zF1wi9hsdbL8WULVZqWM45Wx3UffM9vJUEYgRywOzBKxDjFHyqPMCOHmp6rH4unClUCCMQyaBmGyHxNKad5RJPNVzAZl7YG0a2Ph+dFJ8impBZhBVgF+/diXQ2ogeXx8b3hLylXxCa4AdlkB6yC8Gt/H22oWYcS2CN1NM8KFvIvWzYg0aHHVmPJ8IbqHgO/wncgy369McwtOlJ7ngVzQwEmTt50dlmXM6Gm8DCZNnmUFt4qydOo6RzRTtmfi0YNGtyaUhBxnO9x5kmfgjd88nNQf6bWAYg+P8bNrBJkWLVbluhh7i+vN+HFaFMQP mmiller@m3s-MacBook-Pro.local```
14. [Visit the Packet new ssh key page](https://app.packet.net/portal#/ssh-keys/new)
16. Paste your key into the "new key" field, give it a memorable name and assign it to your project.

## Make a new cluster

17. <table><tr><td>`./provision packet create`</td><td> *create 3 Type 0 instances.*</td></tr></table>
18. <table><tr><td>`./kismatic install apply -f kismatic-cluster.yaml`</td><td> *install Kubernetes*</td></tr></table>

## Tear it down when you're done with it

9. <table><tr><td>`./provision packet delete --all`</td><td> **This will delete every machine in your packet project, even ones not built by this tool!**</td></tr></table>