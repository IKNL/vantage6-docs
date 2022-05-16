---
description: In this section, you will learn how to use the client to create a new organization on the server.
---
# Creating an organization

Here, we assume that you have a Python session with an authenticated Client object, as created in [authentication](authentication.md). We also assume that you have a login on the Vantage6 server that has administrative rights, otherwise, you will not be able to create a new organization.

The first, optional step, is to create an RSA keypair. A keypair, consisting of a private and a public key, can be used to encrypt data transfers. Users from the organization you are about to create will only be able to use encryption if such a keypair has been set up and if they have access to the private key.

```python
from vantage6.common import (warning, error, info, debug, bytes_to_base64s, check_config_write_permissions)
from vantage6.client.encryption import RSACryptor
from pathlib import Path

# Generated a new private key
private_key_filepath = r'/path/to/private/key'
private_key = RSACryptor.create_new_rsa_key(Path(private_key_filepath)

# Generate the public key based on the private one
public_key_bytes = RSACryptor.create_public_key_bytes(private_key)
public_key = bytes_to_base64s(public_key_bytes)
```

Now, we can create an organization

```python
client.organization.create(
    name = 'The_Shire',
    address1 = '501 Buckland Road',
    address2 = 'Matamata',
    zipcode = '3472',
    country = 'New Zealand',
    domain = '',
    public_key = public_key
)
```

Users can now be created for this organization. Any users that are created and who have access to the private key we generated above can now use encryption by running

```python
client.setup_encryption('/path/to/private/key')
```

after they authenticate. 
