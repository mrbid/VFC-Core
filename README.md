# VF Cash
VF Cash is the most efficient IoT Cryptocurrency project, at the current point in time it uses only 1.8 MB of memory, a maximum of 8 threads or 2 threads idle, no proof of work (PoW) and blockchain file that grows at a rate 76% less than Bitcoin. [Whitepaper.pdf](https://github.com/vfcash/RELEASES/blob/master/vfcash.pdf)

The project started on the 23rd of April 2019. It has no Proof-of-Work (POW) rather it has a transaction rate limit per address / public key of three seconds. This prevents the sender or the receiver of a transaction from making any further transactions during this period. The chain is unordered, and the networking uses UDP. The Digital Signature algorithm uses secp256r1.

Divisible to three decimal places, mineable, written in C, compiled with GCC, 256-bit key length, Transactions are 76.16% smaller than an average Bitcoin-Core transaction, relating to total blockchain size.

Transactions are truly free, there is no charge for making a transaction on the network.
However transactions can optionally create inflation of the currency which is partly paid back to the miners in rewards.

This is a private decentralised network, as-in you will need control of some currency before the rest of the network considers your dedicated node viable for indexing.

Join us on Telegram [@vfcash](https://t.me/vfcash)

# Installation & Running a Full Node

**Linux Compile & Install Instructions (Full Node & Client Wallet):**
```
git clone https://github.com/vfcash/VFC-Core && cd VFC-Core
sudo chmod 0777 compile.sh
./compile.sh
```
Then use the `coin help` command in the console for a full command list.

If you wish the full node to launch on startup of the server, `edit /etc/rc.local` and add the command `coin` at the end of the file. Alternatively you can SSH in and launch an instance of coin in a screen or tmux, or if running a crontab add `@restart coin`.

**Windows Install Instructions (Full Node & Client Wallet):**

For a Windows installation you can follow the steps above but first install the Ubuntu Terminal software on Windows first: https://www.microsoft.com/en-us/p/ubuntu/9nblggh4msv6

**To become an active part of the network leave the coin program running in a screen and make sure any necessary ports are forwarded, VFC uses UDP Port 8787.** You will need to make atleast one valid transaction on the network before the mainnet indexes you as a peer and will commuicate with your node. We recommend that you send 1 VFC to yourself, to and from the same address. If you are sent VFC your balance will still show 0 until you perform this operation from your network node. Alternatively you can mine VFC with `coin mine` and your first successful mined VFC will register you on the network. 

Each address is limited to one transaction every three seconds, once a transaction is made the sender cannot make another transaction to a different address for three seconds.

# Denial-of-service Protection

We recommend configuring iptables to throttle incoming UDP packets on port 8787 to 7,133 every minute [~119 packets a second]. 

This should be adequate for the maximum throughput of the entire network allowing 'leeway'.

- **Block Replay:** 10 tps [max]
- **Valid Transactions:** 85 tps [max]
- **Remainder / Utility Requests:** 24 tps

Utility requests such as balance checks, pings, reward payments and user-agent requests have no defined limit like how the Block Replay is always limited to 10 tps and the valid processable transactions of each client is limited to 85 tps. Thus, when both of these services are at their max throughput remainder services will only get 24 tps when using the following iptables configuration;

```
iptables -I INPUT -p udp -i eth0 --dport 8787 -m state --state NEW -m recent --update --seconds 2 --hitcount 255 -j DROP
```

# Additional Software

- [Supervisord](http://supervisord.org/) - Supervisor is a client/server system that allows its users to monitor and control a number of processes on UNIX-like operating systems. A good tool that can relaunch important processes if they happen to crash.


# Third-Party Dependencies

**CRYPTO:**
- https://github.com/brainhub/SHA3IUF   [SHA3]
- https://github.com/esxgx/easy-ecc     [ECDSA]

**Additional Dependencies:**
- CRC64.c - Salvatore Sanfilippo
- Base58.c - Luke Dashjr
