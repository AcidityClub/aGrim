# aGrim
aGrim (AcidityGrim) is a modified version of the open-source, high accuracy anti-cheat,
"Grim". It is designed to add additional features not found in Grim itself.

Note: Everything in this project is on-premise, and does
not require any sort of cloud functionality. Privacy concerns are a huge
issue for many anti-cheats, so...

## aGrim Features
### Mitigations
In other high-quality anti-cheats, they use a "mitigation" system
to prevent cheaters from gaining a major unfair advantage. 

In these anti-cheats, most mitigations that non-movement related (combat, ...) are silent,
essentially wasting the cheater's time (it usually takes a while to notice a mitigation, meaning it takes longer
to return on a new account or try a different bypass method).

aGrim has implemented (or will implement) similar style mitigations.

### Local Heuristics (Basic)
aGrim comes with a few basic locally-held heuristics
to detect cheats and exploits like Kill Aura and Scaffold.

### Additional, very COOL stuff
aGrim has added/will add various features to mitigate
and make cheats almost completely ineffective. This includes:

- Filter Checks (Removes/Obfuscates sensitive information from packets, like health)

- Latency-Spoof and Blink mitigations (Stops outgoing packets if blink is being used, lags them
if suspected of fake-lag)

- Transaction Obfuscation

- Different movement mitigation modes (Want Polar-style setbacks? You can set it in 
the config!)


## Grim Supremacy

What makes Grim stand out against other anticheats?

### Movement Simulation Engine

* We have a 1:1 replication of the player's possible movements
    * This covers everything from basic walking, swimming, knockback, cobwebs, to bubble columns
    * It even covers riding entities from boats to pigs to striders
* Built upon covering edge cases to confirm accuracy
* 1.13+ clients on 1.13+ servers, 1.12- clients on 1.13+ servers, 1.13+ clients on 1.12- servers,
  and 1.12- clients on 1.12- servers are all supported regardless of the large technical changes
  between these versions.
* The order of collisions depends on the client version and is correct
* Accounts for minor bounding box differences between versions, for example:
    * Single glass panes will be a + shape for 1.7-1.8 players and * for 1.9+ players
    * 1.13+ clients on 1.8 servers see the + glass pane hitbox due to ViaVersion
    * Many other blocks have this extreme attention to detail.
    * Waterlogged blocks do not exist for 1.12 or below players
    * Blocks that do not exist in the client's version use ViaVersion's replacement block
    * Block data that cannot be translated to previous versions is replaced correctly
    * All vanilla collision boxes have been implemented

### Fully asynchronous and multithreaded design

* All movement checks and the overwhelming majority of listeners run on the netty thread
* The anticheat can scale to many hundreds of players, if not more
* Thread safety is carefully thought out
* The next core allows for this design

### Full world replication

* The anticheat keeps a replica of the world for each player
* The replica is created by listening to chunk data packets, block places, and block changes
* On all versions, chunks are compressed to 16-64 kb per chunk using palettes
* Using this cache, the anticheat can safely access the world state
* Per player, the cache allows for multithreaded design
* Sending players fake blocks with packets is safe and does not lead to falses
* The world is recreated for each player to allow lag compensation
* Client sided blocks cause no issues with packet based blocks. Block glitching does not false the
  anticheat.

### Latency compensation

* World changes are queued until they reach the player
* This means breaking blocks under a player does not false the anticheat
* Everything from flying status to movement speed will be latency compensated

### Inventory compensation

* The player's inventory is tracked to prevent ghost blocks at high latency, and other errors

### Secure by design, not obscurity

* All systems are designed to be highly secure and mathematically impossible to bypass
* For example, the prediction engine knows all possible movements and cannot be bypassed
