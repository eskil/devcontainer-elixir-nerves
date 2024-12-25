# Elixir Nerves - VSCode devcontainer

Use with the VSCode **Remote - Containers** extension.

### Image
* Debian bookworm (12)
* All [nerves install steps](https://hexdocs.pm/nerves/installation.html#linux) are in place
  * [asdf](https://asdf-vm.com/guide/introduction.html)
* Uses docker-compose, you can easily add extra services

### Other goodies
* zsh preconfigured with p10k
  * To customize prompt, run `p10k configure` or edit `~/.p10k.zsh`.
  * and copy new one to `.devcontainer/config/.p10k.zsh` to preserve it.
* With command history preserved.
* Mounts under `/workspaces/project`.
  * To change this, change `/workspaces/project` in all files in `.devcontainer`
* `MIX_TARGET=rpi0` set, change in `post-create`
* _No_ ssh key.


### Does not...

It installs;
* [fwup](https://github.com/fwup-home/fwup)

But since docker (MacOS) cannot access usb devices, you cannot burn
firmware from within the devcontainer. You need to build `fwup`
locally and do the burns locally.

See also
* [This elixir post discussion](https://elixirforum.com/t/nerves-development-environment-with-docker-and-vs-code/35973) reg. nerves devcontainer
*
  [Docker FAQ](https://docs.docker.com/desktop/troubleshoot-and-support/faqs/general/#can-i-pass-through-a-usb-device-to-a-container)


### Get started

```
mix nerves.new hello_nerves
```

You'll need a ssh key in `~/.ssh` that matches what's in `config/target.exs`;

```
Path.join(".ssh/id_{rsa,ecdsa,ed25519}.pub")
```

This is not created as part of the image.

See also [nerves ssh key setup](https://hexdocs.pm/nerves_firmware_ssh/readme.html#device-keys)
