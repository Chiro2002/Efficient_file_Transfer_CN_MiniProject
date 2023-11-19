# Efficient_file_Transfer_CN_MiniProject
This repository contains the implementation of a bittorrent-like P2P network efficient file transfer method written in python Each node in the network first contacts the tracker to transfer the essential information and then acts based on that.

## General Explanation
As said before, nodes need to communicate with the tracker to send or receive
the information required for their action, which is either uploading a file or
downloading one. Note that sending/receiving multiple files do not create an
issue since every action is done via a corresponding thread.
Each node's files are placed in a separate folder to simulate the real world.

Possible actions are:
- Upload: The node first tells the tracker it wants to upload a file and the
tracker adds that node to a list holding the `owners` of a file. Then, it 
waits for download requests from other nodes.
- Download: After telling the tracker, the node receives the list of `owners`
of that requested file. Then, asks the file size from one of the owners and
divides that size by the `owners` length. After that,
each uploader sends its portion of the file and the whole process is done via
a thread for each part. After all threads are done, the file is glued together.
- Exit: Exits the network while de-allocating the previously held socket. The
tracker then removes this node from the `owners` list.

## Communication Messages
There are **four** distinct types of message that is transferred between objects
in the network:
1. NodeToTracker: Tells the tracker whether it has a file and wants to upload 
it (mode `have`), wants to receive (download) a file (mode `need`), or wants
to exit the network (mode `exit`).
2. TrackerToNode: Responds to a node who wants to download a file and transfers
the `owner` list of that file to them.
3. SizeInformation: Sent when a node starts downloading a file and wants to 
know the size of that. `size = -1` when the downloader asks the size. 
4. FileCommunication: The most crucial message type which contains the actual
parts of data sent by different nodes.

We tried to make the code self-explanatory, so everything else should be easy
to understand by just looking at the code.

## Usage (IMP)
First, you need to run the tracker; To do that, simply run `python3 tracker.py`. 
Then you need to run some nodes using the following line:
```commandline
python3 node.py -n <node name> -p <input port> <output port>
```
e.g. python3 node.py -n NodeA -p 8080 9090
Now the `node.py` waits for your command to be entered. Possible commands are:
- `torrent -setMode upload <filename>` to upload a file and wait for requests.
- `torrent -setMode download <filename>` to start downloading a file.
- `torrent exit` to exit the network.
- e.g. torrent -m upload avengers.png



