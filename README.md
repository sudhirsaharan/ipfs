# **ipfs**

**What Is IPFS ?**
![image representing ipfs](https://camo.githubusercontent.com/140c9a1fac2bafdc2f2a34208b5639edb6fe08e8/68747470733a2f2f697066732e696f2f697066732f516d566b37737272776168584c4e6d634459767955454a7074796f78706e646e52613537594a31314c346a5632362f697066732e676f2e706e67)
IPFS is a peer-to-peer hypermedia protocol to make the web faster, safer, and more open. It combines good ideas from Git, BitTorrent,
Kademlia, SFS, and the Web. It is like a single bittorrent swarm, exchanging git objects. IPFS provides an interface as simple as the 
HTTP web, but with permanence built in. You can also mount the world at /ipfs.

For more info see: https://github.com/ipfs/ipfs.

**System Requirements**

IPFS can run on most Linux, macOS, and Windows systems. We recommend running it on a machine with at least 2 GB of RAM
(it’ll do fine with only one CPU core), but it should run fine with as little as 1 GB of RAM. On systems with less memory, it may not
be completely stable.

**npm**
This project is available through npm. To install run

```> npm install ipfs --save```
We support both the Current and Active LTS versions of Node.js. Please see nodejs.org for what these currently are.

This project is tested on OSX & Linux, expected to work on Windows.

**Use in Node.js**
To create an IPFS node programmatically:

```const IPFS = require('ipfs')
const node = new IPFS()

node.on('ready', () => {
  // Ready to use!
  // See https://github.com/ipfs/js-ipfs#core-api
})
```

Through command line tool
In order to use js-ipfs as a CLI, you must install it with the global flag. Run the following (even if you have ipfs installed locally):

```npm install ipfs --global```

The CLI is available by using the command jsipfs in your terminal. This is aliased, instead of using ipfs, to make sure it does not conflict with the Go implementation.

Use in the browser
Learn how to bundle with browserify and webpack in the examples folder.

You can also load it using a <script> using the unpkg CDN or the jsDelivr CDN. Inserting one of the following lines will make a Ipfs object available in the global namespace.

```<!-- loading the minified version -->
<script src="https://unpkg.com/ipfs/dist/index.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/ipfs/dist/index.min.js"></script>

<!-- loading the human-readable (not minified) version -->
<script src="https://unpkg.com/ipfs/dist/index.js"></script>
<script src="https://cdn.jsdelivr.net/npm/ipfs/dist/index.js"></script>
Inserting one of the above lines will make an Ipfs object available in the global namespace:

<script>
const node = new window.Ipfs()

node.on('ready', () => {
  // Ready to use!
  // See https://github.com/ipfs/js-ipfs#core-api
})
</script>
```
**IPFS Daemon**
The IPFS Daemon exposes the API defined http-api-spec. You can use any of the IPFS HTTP-API client libraries with it, such as:
js-ipfs-api.

If you want a programmatic way to spawn a IPFS Daemon using JavaScript, check out ipfsd-ctl module

**IPFS Module**
Use the IPFS Module as a dependency of a project to spawn in process instances of IPFS. Create an instance by calling new IPFS() and
waiting for its ready event:

```// Create the IPFS node instance
const node = new IPFS()

node.on('ready', () => {
  // Your node is now ready to use \o/

  // stopping a node
  node.stop(() => {
    // node is now 'offline'
  })
})
```
 
 
 **files.add**
 
####Add files and data to IPFS####

Go WIP
```JavaScript - ipfs.files.add(data, [options], [callback])```
Where data may be:

a Buffer instance
a Readable Stream
a Pull Stream
```an array of objects, each of the form:
{
  path: '/tmp/myfile.txt', // The file path
  content: <data> // A Buffer, Readable Stream or Pull Stream with the contents of the file
}
```
If no content is passed, then the path is treated as an empty directory

```options is an optional object argument that might include the following keys:

cid-version (integer, default 0): the CID version to use when storing the data (storage keys are based on the CID, including it's version)
progress (function): a function that will be called with the byte length of chunks as a file is added to ipfs.
recursive (boolean): for when a Path is passed, this option can be enabled to add recursively all the files.
hashAlg || hash (string): multihash hashing algorithm to use. (default: sha2-256) The list of all possible values
wrapWithDirectory (boolean): adds a wrapping node around the content.
onlyHash (boolean): doesn't actually add the file to IPFS, but rather calculates its hash.
pin (boolean, default true): pin this object when adding.
raw-leaves (boolean, default false): if true, DAG leaves will contain raw file data and not be wrapped in a protobuf
chunker (string, default size-262144): chunking algorithm used to build ipfs DAGs. Available formats:
size-{size}
rabin
rabin-{avg}
rabin-{min}-{avg}-{max}
```
callback must follow function (err, res) {} signature, where err is an error if the operation was not successful. res will be an array of:
```
{
  path: '/tmp/myfile.txt',
  hash: 'QmHash', // base58 encoded multihash
  size: 123
}
```
If no callback is passed, a promise is returned.

Example:
```
const files = [
  {
    path: '/tmp/myfile.txt',
    content: (Buffer or Readable stream)
  }
]

ipfs.files.add(files, function (err, files) {
  // 'files' will be an array of objects containing paths and the multihashes of the files added
})
 ```
 
 **files.read**
Read a file into a Buffer.

Go WIP
```JavaScript - ipfs.files.read(path, [options], [callback])```
Where:

path is the path of the file to read and must point to a file (and not a directory)
options is an optional Object that might contain the following keys:
offset is an Integer with the byte offset to begin reading from (default: 0)
length is an Integer with the maximum number of bytes to read (default: Read to end of stream)
callback is an optional function with the signature function (error, buffer) {}, where error may be an Error that occured if the operation was not successful and buffer is a Buffer with the contents of path
If no callback is passed, a promise is returned.

N.b. this method is likely to result in high memory usage, you should use files.readReadableStream or files.readPullStream instead where possible.

Example:

```ipfs.files.read('/hello-world', (error, buf) => {
  console.log(buf.toString('utf8'))
})

// Hello, World! 
```

Similarly you can do it for images,multiple files etc. 




