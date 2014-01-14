# SM Framework - PostgreSQL High Availability Extension

# Installation

First be sure you have installed and setup etcd,

    root# sm set install servers
		root# sm etcd install 
		root# sm etcd configure client_ip {ip of master server} peer_ip {ip of master server}

    root# sm pgha nginx configure etcd_ip {etcd ip}

Note that {etcd ip} should be the locally exposed public IP address (not 127.0.0.1).
This is usually a 10. or 192. network address.

# Configuration

# Using
