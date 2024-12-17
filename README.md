# Elixir Nerves - VSCode devcontainer
![elixir logo](https://elixir-lang.org/images/logo/logo.png)

Use with the VSCode **Remote - Containers** extension.

* zsh preconfigured, remove `.devcontainer/config/.p10k.zsh` to reset

* mounts under `/workspaces/project`. 
  * To change this, change `"/workspaces/project` in all files in `.devcontainer`

* uses docker-compose, you can easily add extra services

* All nerves install steps are in place
  * [asdf](https://asdf-vm.com/guide/introduction.html)

