# Cassiar

Cassiar backup utility

### Theory

Three major components/things

#### catalogue - Where files are for each thing

- Basically a key value kinda thing, hash (blake3, https://crates.io/crates/blake3 / https://github.com/BLAKE3-team/BLAKE3), and where the actual file is stored
  - What file storage file, and what bit it starts at
  - Seems to get really good accel, so hopefully wont be cpu limited and just the disk or whatever
  - Ideally can scan an entire ssd on my framework within a minute or so, mostly limited by the disk speed.
- Limit each file to say 10mb or so
- Example file layouts:
  - 00001
  - 00050
  - 05000
- Can generate new files in between as needed when get too large, so easily split up and stuff
- Probably want to let it burst to say 15mb or so, so every time they add a new hash to an area, it has to resplit the files again
- Should handle billions of files well? trillions? i hope at least
- Hash each file so it's not replicated - kinda important

#### Snapshots

- A record of how files are originized and what are in each snapshot
- Structure it so it can do a partial snapshot while not being written (like a partial snapshot) that's not fully counted

#### File storage

- 32mb blocks or so for storing actual files
  - uuid kinda setup, maybe nested folders so there's not a bajaillion files in a dir slowing down indices? though idk if that is an issue if i already have the file setup

pq encryption?

- https://aws.amazon.com/blogs/opensource/introducing-aws-libcrypto-for-rust-an-open-source-cryptographic-library-for-rust/
- Do some sort of pq thing? ideally accelerated as well, like how the blake3 thing is being accelerated
- Two methods of encrypting the key hopefully, something like a stored key that can be easily automated on a system to back up, and a password auth method for later logins stuff
