Keypairs are a way for you to automatically provision linux machines with SSH
public keys to use for Public-Key authentication with SSHv2.

Public-Key authentication is both:

* Secure: breaking an SSH key requires so much time and computational power
  that these sorts of attacks are not practical in the real world. SSH keys
  are much, much secure than even very strong passwords.

* Convenient: instead of managing per-machine passwords or sharing them across
  your company, every physical person who needs access to your servers give
  you their public key. You can then setup granular access control by adding
  those keys only to the relevant machines. If you need to revoke someone's
  access, simply revoking his key prevents him from logging in without
  altering other people's workflow.

Note that while you can have multiple keypairs in your account, the
instance creation dialog only allows you to select one keypair. Fine key-based
access control is the job of configuration management tools.

Keypairs can be managed in two ways from the [keypairs view](/keypairs).

### Importing a keypair

If you aleady have an SSH key, select the "Import" tab on the keypair creation
view. Paste the content of the **public** key in the box.

### Creating a keypair

If you do not have a keypair, you can let exoscale create one for you. On the
creation view, select the "Create" tab and set the keypair name.

Save the private key that appears on the screen. That private key is also
emailed to you. If you don't want your private key to transit via email we
recommend creating an RSA key on your machine and importing it instead.

### Provisioning an instance with a keypair

When creating instances, simply select the keypair you want to be associated
to this instance and the person holding the corresponding private key will be
able to log in.

Deleting a keypair does not automatically remove the authorized public key
from already created instances. If you want to completely revoke a key you
need to do so on every instance holding this key.
