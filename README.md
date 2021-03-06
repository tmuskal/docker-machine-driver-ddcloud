# Docker Machine driver for Dimension Data CloudControl

If you want detailed instructions, click [here](docs/setup.md). Otherwise, keep reading for quickstart documentation.

## Usage

You will need:

* A network domain
* A VLAN in that network domain (servers will be attached to this VLAN)
* If using public IP addresses, a firewall rule that permits SSH traffic from your (local) public IPv4 address to the VLAN's IPv4 network
Alternatively, you can use the `--ddcloud-create-ssh-firewall-rule` flag when creating your machine if you have permissions in CloudControl to create firewall and NAT rules
* If using private IP addresses, you will need to be connected to the CloudControl VPN for the target data centre

### Example

```bash
docker-machine create --driver ddcloud \
	--ddcloud-region AU \
	--ddcloud-datacenter AU9 \
	--ddcloud-networkdomain 'my-docker-domain' \
	--ddcloud-vlan 'my-docker-vlan' \
	--ddcloud-ssh-key ~/.ssh/id_rsa \
	--ddcloud-memorygb 8 \
	--ddcloud-cpucount 4 \
	--ddcloud-corespersocket 4 \
	--ddcloud-ssh-bootstrap-password 'throw-away-password' \
	mydockermachine
```

If you're running on Windows, just remove the backslashes so the whole command is on a single line.

### Options

The driver supports all Docker Machine commands, and can be configured using the following command-line arguments (or environment variables):

* `ddcloud-user` - The user name used to authenticate to the CloudControl API.
Environment: `MCP_USER`
* `ddcloud-password` - The password used to authenticate to the CloudControl API.
Environment: `MCP_PASSWORD`.
* `ddcloud-region` - The CloudControl region name (e.g. AU, NA, EU, etc).
Environment: `MCP_REGION`.
* `ddcloud-networkdomain` - The name of the target CloudControl network domain.
* `ddcloud-datacenter` - The name of the CloudControl datacenter (e.g. NA1, AU9) in which the network domain is located.
* `ddcloud-vlan` - The name of the target CloudControl VLAN.
* `ddcloud-memorygb` - The amount of RAM in GB for the target machine. (Default: taken from image)
* `ddcloud-cpucount` - The amount of CPUs for the target machine. (Default: taken from image)
* `ddcloud-corespersocket` - The amount of cores per socket for the target machine. (Default: taken from image)
* `ddcloud-image-name` - The name of the image used to create the target machine.
Additionally, the OS must be a Linux distribution supported by docker-machine (Ubuntu 12.04 and above are supported, but RedHat 6 and 7 are not supported due to iptables configuration issues).
* `ddcloud-ssh-user` - The SSH username to use.
Default: "root".
Environment: `MCP_SSH_USER`.
* `ddcloud-ssh-key` - The SSH key file to use.
Environment: `MCP_SSH_KEY`.
* `ddcloud-ssh-port` - The SSH port to use.
Default: 22.
Environment: `MCP_SSH_PORT`.
* `ddcloud-ssh-bootstrap-password` - The initial SSH password used to bootstrap SSH key authentication.
This password is removed once the SSH key has been installed
Environment: `MCP_SSH_BOOTSTRAP_PASSWORD`
* `ddcloud-create-ssh-firewall-rule` - Automatically create a firewall rule to enable inbound SSH to the target server?
* `ddcloud-client-public-ip` - Use the specified IPv4 address as the client's public IP address (don't auto-detect).
Environment: `MCP_CLIENT_PUBLIC_IP`.
* `ddcloud-use-private-ip` - Don't create NAT and firewall rules for target server (you will need to be connected to the VPN for your target data centre).

## Installing the driver

Download the [latest release](https://github.com/DimensionDataResearch/docker-machine-driver-ddcloud/releases) and place the provider executable in the same directory as `docker-machine` executable (or somewhere on your `PATH`).

### UI plugin for Rancher

Rancher can automatically generate a UI for calling the `ddcloud` driver for Docker Machine, but we also have a UI plugin for Rancher (it's not yet as mature as the Docker Machine driver):

https://github.com/DimensionDataResearch/rancher-ui-driver-ddcloud#using

## Building the driver

If you'd rather run from source, simply run `make install` and you're good to go.
