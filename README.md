# SYNOPSIS

[![js-standard-style](https://cdn.rawgit.com/feross/standard/master/badge.svg)](https://github.com/feross/standard)  

This is an implementation of the modified merkle patricia tree.

> The modified Merkle Patricia tree (trie) provides a persistent data structure to map between arbitrary-length binary data (byte arrays). It is defined in terms of a mutable data structure to map between 256-bit binary fragments and arbitrary-length binary data. The core of the trie, and its sole requirement in terms of the protocol specification is to provide a single 32-byte value that identifies a given set of key-value pairs.   

The only backing store supported is LevelDB through the ```levelup``` module.

# INSTALL
 `npm install merkle-patricia-tree`

# USAGE

## Initialization and Basic Usage

```javascript
var Trie = require('merkle-patricia-tree'),
level = require('level'),
db = level('./testdb'),
trie = new Trie(db);

trie.put('test', 'one', function () {
  trie.get('test', function (err, value) {
    if(value) console.log(value.toString())
  });
});
```

## Merkle Proofs

```javascript
Trie.prove(trie, 'test', function (err, prove) {
  if (err) return cb(err)
  Trie.verifyProof(trie.root, 'test', prove, function (err, value) {
    if (err) return cb(err)
    console.log(value.toString())
    cb()
  })
})
```

## Read stream on Gpuffs DB

```javascript
var level = require('level')
var Trie = require('./secure')

var stateRoot = "0xb4973da140b05bfffb1cd734ed871f888e71cf563a4218f82a092fc4540f6c03" // Block #0

var db = level('YOUR_PATH_TO_THE_GPUFFS_CHAIN_DB')
var trie = new Trie(db, stateRoot)

trie.createReadStream()
  .on('data', function (data) {
    console.log(data)
  })
  .on('end', function() {
    console.log('End.')
  })
```

# API
[./docs/](./docs/index.md)

# TESTING
`npm test`

# PuffscoinJS

See our organizational [documentation](http://puffscoin.leafycauldronapothecary.com/puffwiki/puffscoinjs-user-guide/) for an introduction to `PuffscoinJS`.

# LICENSE
MPL-2.0
