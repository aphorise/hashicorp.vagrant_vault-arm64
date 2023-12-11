# HashiCorp `vagrant` example of **`vault`** on ARM64
This repo contains a `Vagrantfile` mocking an example [Vault](https://www.vaultproject.io/) setup using Integrated Storage (aka Raft).

Built on :apple: MacOS Apple Silicon (with M3) and substitute for former VirtualBox based image that will not work on Apple's Arm based architecture.


### Prerequisites
 - [:apple: **macOS** (aka OSX) Fusion 13](https://www.vmware.com/products/fusion.html) for Apple Silicon (M1, M2 / M3).
 - [**Vagrant**](https://www.vagrantup.com/)
 - :lock: **NOTE**: An [enterprise license](https://www.hashicorp.com/products/vault/pricing/) will be required for enterprise features like: [performance standbys](https://www.vaultproject.io/docs/enterprise/performance-standby/), [replication](https://www.vaultproject.io/docs/enterprise/replication/) & more. By Default it install OSS version not requiring a license.


## Usage & Workflow
Refer to the contents of **`Vagrantfile`** for version and details of provisioning steps. Set the `VV1` in Vagrant variable with exact version you wish to install (if not latest).

If a license is required thenb place the complete license in the directory `vault_files` in the content of the file: `vault_license.txt`. A custom auto-unseal can also be specified via the file  `vault_seal.hcl.commented` which should be renamed to `vault_seal.hcl` where auto-unseals may be applicable.

```bash
vagrant up --provider=vmware_desktop ;
# // ... output of provisioning steps.

vagrant global-status --prune ; # should show running nodes
# id       name   provider       state   directory
# ------------------------------------------------------------------------------
# b87e2f3  vault1 vmware_desktop running /Users/me/hashicorp.vagrant-vault-arm64

# // On a separate Terminal session check status of consul1 & its members.
vagrant ssh vault1
# // ...

# // ---------------------------------------------------------------------------
# when completely done:
vagrant destroy -f ;
vagrant box remove -f aphorise/debian12-arm64 --provider=vmware_desktop ; # ... delete box images
```


## Notes
Ported from original X86 / AMD64 reproduction:

 - [aphorise/hashicorp.vagrant_vault-dr](https://github.com/aphorise/hashicorp.vagrant_vault-dr)
 - [aphorise/vagrant_haproxy_vault-dr-clusters](https://github.com/aphorise/vagrant_haproxy_vault-dr-clusters)

------
