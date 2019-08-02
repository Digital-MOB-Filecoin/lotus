# project lotus - 莲

Lotus is an experimental implementation of the Filecoin Distributed Storage
Network. For more details, check out the
[spec](https://github.com/filecoin-project/spec).

## Development

All work is tracked via issues, and an attempt to keep an up to date view on
this exists in the [lotus testnet github project
board](https://github.com/filecoin-project/go-lotus/projects/1).


## Building

*Dependencies:*
- go1.12 or higher
- rust (version?)
- git
- bzr (some go dependency needs this)

*Building:*
```
$ make
```

## Architecture
Lotus is architected modularly, and aims to keep clean api boundaries between
everything, even if they are in the same process. Notably, the 'lotus full node'
software, and the 'lotus storage miner' software are two separate programs.

The lotus storage miner is intended to be run on the machine that manages a
single storage miner instance, and is meant to communicate with the full node
via the websockets api for all of its chain interaction needs. This way, a
mining operation may easily run one or many storage miners, connected to one or
many full node instances.

## Notable Modules

### Api
The systems api is defined in here. The rpc maps directly to the api defined
here using the jsonrpc package in `lib/jsonrpc`.

### Chain/Types
Implementation of data structures used by Filecoin and their serializations.

### Chain/Store
The chainstore manages all local chain state, including block headers,
messages, and state.

### Chain/State
A package for dealing with the filecoin state tree. Wraps the
[HAMT](https://github.com/ipfs/go-hamt-ipld).

### Chain/Actors
Implementations of the builtin Filecoin network actors.

### Chain/Vm
The filecoin state machine 'vm'. Implemented here are utilities to invoke
filecoin actor methods.


### Miner
The block producer logic. This package interfaces with the full node through
the api, despite currently being implented in the same process (very likely to
be extracted as its own separate process in the near future).

### Storage
The storage miner logic. This package also interfaces with the full node
through a subset of the api. This code is used to implement the
lotus-storage-miner process.

## License
MIT + Apache
