# angelFarmer

## Description
A project to create a cable-driven agricultural robot.

## Justification
Why this project?

## Phases
1. Farmbot Simulator
2. Mathematica model
3. Hardware design
4. Hardware Fabrication
5. Electrical design
6. Putting all things together
7. Testing
8. Website

## Farmbot Simulator

### Installing FarmbotOs in Ubuntu Server


### Erlang Dependencies

```bash
sudo apt-get install automake autoconf libreadline-dev libncurses-dev libssl-dev libyaml-dev libxslt-dev libffi-dev libtool unixodbc-dev  libgl1-mesa-dev  libglu1-mesa-dev libssh-dev xsltproc fop libxml2-utils
```

For Ubuntu 20.4
```
sudo apt install libwxgtk3.0-gtk3-dev
```

Ubuntu Versions < 20.4
```
sudo apt install libwxgtk3.0-dev
```

In case you are installing on small server like 1GB memory, compiling erlang may fail. If this happens, [increasing swap memory will help](https://linuxhandbook.com/increase-swap-ubuntu/) But it will still take a very long time to compile.

Begin by installing the requirements(erlang and elixir) as `asdf plugins` by following these [instructions](https://hexdocs.pm/nerves/installation.html#Linux). The steps in the page (March 2021) are given below 



```bash
git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.8.0

# The following steps are for bash. If you’re using something else, do the
# equivalent for your shell.
echo -e '\n. $HOME/.asdf/asdf.sh' >> ~/.bashrc
echo -e '\n. $HOME/.asdf/completions/asdf.bash' >> ~/.bashrc # optional
source ~/.bashrc
# for zsh based systems run the following
echo -e '\n. $HOME/.asdf/asdf.sh' >> ~/.zshrc
echo -e '\n. $HOME/.asdf/completions/asdf.bash' >> ~/.zshrc
source ~/.zshrc

asdf plugin-add erlang
asdf plugin-add elixir

# Note #1:
# If on Debian or Ubuntu, you'll want to install wx before running the next line:
# sudo apt install libwxgtk3.0-dev
# for arch based systems run the next line:
# yay -S wxgtk2 fop jdk-openjdk unzip


# Note #2:
# It's possible to use different Erlang and Elixir versions with Nerves. The
# latest official Nerves systems are compatible with the versions below. In
# general, differences in patch releases are harmless. Nerves detects
# configurations that might not work at compile time.
asdf install erlang 23.1.4
asdf install elixir 1.11.2-otp-23
asdf global erlang 23.1.4
asdf global elixir 1.11.2-otp-23
```

Clone the farmbotOs repo:
```bash
git clone https://github.com/FarmBot/farmbot_os.git --recursive
```

If it fails to clone the inner repos, then you may need to add your ssh key to the command. Replace `/tmp/gitkey` with the path to your github ssh key.
```bash
GIT_SSH_COMMAND='ssh -i /tmp/gitkey -o IdentitiesOnly=yes' git clone https://github.com/FarmBot/farmbot_os.git --recursive
```

Install the dependencies
```bash
cd farmbot_os
mix deps.get
cd farmbot_os
mix archive.install hex nerves_bootstrap
mix deps.get
```

Test if all things are fine:
```bash
FARMBOT_EMAIL="farmbotEmail" FARMBOT_PASSWORD="farmbotPassword" FARMBOT_SERVER="https://my.farm.bot" iex -S mix
```

### Cloning FarmbotOS operations

The farmbot WebApp will not accept mqtt connections for a device that has not been created. This process will assist in seeing the request to farmbot webApp from the OS that help with creating the device.

#### Deleting a device from farmboWeb App
You will require to delete an existing device in a farmbot account for all the requests to be seen in their order.

***This step is really not necessary. But just incase it is needed, goto [delete](http://your-simulator/#/delete)***

#### Installing the proxy
The proxy is a cors-anywhere proxy modified to redirect requests received at `/path` to `http:/my.farmbot.io/path` and log both the request and the response.

See [how to install the proxy](https://github.com/AngelFarmer/cors-escape).

#### Checking the requests

```bash
cd farmbot_os/farmbot_os/
FARMBOT_EMAIL="farmbotEmail" FARMBOT_PASSWORD="farmbotPassword" FARMBOT_SERVER="http://localhost:2000" iex -S mix
```

## WebSocket

Is this the connection to the webapp?
The above not helpful.Try connecting to websocket

Push communication with broker to the future...
