# Contributing

## Running the dev environemnt

### Get Nix

This project is nixified, so first make sure you have [nix installed](https://nixos.org/download) and [experimental features](https://nixos.wiki/wiki/Nix_command) turned on so that you can use the `nix develop` command.

Once nix is installed, clone the repo and inside the project directory run:

```
nix develop
```

The first time you run this expect to wait for everthing to download/build.

Now, you have **bitcoin and lightning nodes**, all the **required packages**, and **shell variables** defined!

### Start, fund, and connect your nodes

#### Start nodes in regtest

Source the [`startup_regtest.sh`](./startup_regtest.sh) script and then start 2 nodes.

```
source ./startup_regtest.sh
```

That will give you access to a series of handy scripts. One starts your nodes:

**NOTE**: Most of these scripts are hardcoded to work with prisms in a 5 node network

```
start_ln 5 #creates a 5 node network
```

## Prism network configuration

Make sure your bitcoind node is funded, and then run `./bootstrap_network.sh`

That will fund all the nodes, connect them, and then open channels in the configuration necessary for creating prisms with 3 members payable by `l1`.

## Start/restart plugin

`./restart.sh [NODE_NUM]`

NODE_NUM is an optional argument that specifies which node to start the prism. Defaults to `NODE_NUM=2`.

Make sure the plugin path is correctly defined in the .testing.env file.

#### NOTE: A handy trick is to use:

`ls | entr -s './testing/restart.sh'`

This will restart the plugin every time a file in the root dir changes

## Create prisms

`./create_prism.sh`

This just creates a prism where `l1` can pay `l2`'s prism which pays out to `l3`, `l4`, and `l5`.

## Paying a prism

"Paying a prism" is equivalent to paying a BOLT 12 offer.

1. `l1` has to fetch an invoice from `l2`. See `fetchinvoice` in the CLN docs
2. `l1` pays that invoice. See `pay` in the CLN docs

When a payment is received, the node looks up in the datastore to see if a prism exists for that offer. Then it iterates through each member paying out their deserved amount.

## Updating a prism

The plugin has a method called `updateprism` (see docs)

I made the `update_prism.sh` script to more easily pass updates. You will need to paste in the `offer_id` of an existing prism. This script is primarily for testing the method, but could be improved to be more functional.

## Debugging

There is a function in `bolt12-prism.py` called `printout`.

Use this to log anything to a file on your machine.

You can use to `watch -n 1 cat /tmp/plugin_out` to hot reload the file

## Help

Reach out if you have any questions. This project has been a learning experience for me. Feedback is appreciated.
