# Elixir Nerves - VSCode devcontainer

Use with the VSCode **Remote - Containers** extension.

### Image
* Debian bookworm (12)
* All [nerves install steps](https://hexdocs.pm/nerves/installation.html#linux) are in place
  * [asdf](https://asdf-vm.com/guide/introduction.html)
  * [fwup](https://github.com/fwup-home/fwup)

### Other goodies
* zsh preconfigured with p10k
  * To customize prompt, run `p10k configure` or edit `~/.p10k.zsh`.
  * and copy new one to `.devcontainer/config/.p10k.zsh` to preserve it.
* with command history preserved

### Other
* uses docker-compose, you can easily add extra services
* mounts under `/workspaces/project`.
  * To change this, change `/workspaces/project` in all files in `.devcontainer`
