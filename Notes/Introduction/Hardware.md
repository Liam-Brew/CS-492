# Hardware

## Table of Contents

- [Hardware](#hardware)
  - [Table of Contents](#table-of-contents)
  - [Memory](#memory)
  - [Disks](#disks)
  - [IO Devices](#io-devices)
  - [Buses](#buses)

## Memory

![memory](/notes/assets/introduction/memory.PNG)

![memory2](/notes/assets/introduction/memory2.PNG)

**cache**: temporarily holds a piece of data from a slower memory into a faster memory. There are hardware and software caches. Also an interface (when not transparent):

- Get(), returns:
  - hit
  - miss
- Set()

Questions when dealing with caches:

- when to put a new item into the cache?
- which cache slot to put the new item in?
- which item to remove from the cache when a slot is needed?
- where to put a newly evicted item in the larger memory?

![chips](/notes/assets/introduction/chips.PNG)

Advantages: shared memory (a), more independent tasks (b)

## Disks

Mechanical hard drive:

![hdd](/notes/assets/introduction/hdd.PNG)

Solid state drive:

![ssd](/notes/assets/introduction/ssd.PNG)

## IO Devices

![io](/notes/assets/introduction/io.PNG)

## Buses

![bus](/notes/assets/introduction/bus.PNG)

![bus2](/notes/assets/introduction/bus2.PNG)