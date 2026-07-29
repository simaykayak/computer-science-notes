# File Systems and Storage

## 1. File System

A file system is a logical structure that determines how data is organized, stored, and accessed on a storage device. The operating system uses the file system to manage the physical location of files, their size, permissions, and other metadata.

The main responsibilities of a file system are:

- Creating the file and directory structure
- Storing file metadata
- Managing free disk space
- Controlling file access permissions
- Ensuring data integrity

## Metadata

A file consists of more than just its content. Additional information about the file is also stored. This information is called metadata.

Examples of metadata include:

- File name
- File size
- Creation date
- Last modified date
- File owner
- Access permissions
- Block addresses on the disk


## 2. Common File Systems

Different operating systems use different file systems.

### *NTFS (New Technology File System)*

NTFS is the default file system developed by Microsoft for Windows operating systems.

**Features:**

- Journaling support
- File permissions (ACL)
- File compression
- Encryption (EFS)
- Support for large files and partitions

### *ext4 (Fourth Extended File System)*

ext4 is one of the most widely used file systems in Linux distributions.

**Features:**

- Open source
- Supports journaling
- Supports large file systems
- High performance
- Low fragmentation rate

### *APFS (Apple File System)*

APFS was developed by Apple for macOS, iOS, iPadOS, and other Apple operating systems.

**Features:**

- Optimized for SSDs
- Built-in encryption support
- Supports snapshots
- Uses Space Sharing technology


## 3. Journaling

Journaling is a mechanism used in file systems to maintain data integrity.

Before any modification is made to a file, the operations to be performed are first written to a special log called the journal.

If the operation completes successfully, the journal entry is removed.

In the event of a power failure or system crash, the operating system can use the journal to restore the file system to a consistent state.

This method significantly reduces the risk of data loss.

## 4. Block

Data on storage devices is stored in fixed-size units called blocks.

The size of a block depends on the file system.

Common block sizes include:

- 1 KB
- 2 KB
- 4 KB
- 8 KB

The most common block size in modern operating systems is 4 KB.

Example:

File size: 9000 bytes

Block size: 4096 bytes

Required number of blocks:

9000 / 4096 ≈ 2.2 → 3 blocks are required.

Since the third block is not completely filled, some space remains unused. This is called internal fragmentation.


## 5. HDD (Hard Disk Drive)

An HDD (Hard Disk Drive) is an electromechanical storage device that stores data on rotating magnetic platters.

### Main Components

- Magnetic platter
- Read/Write head
- Motor
- Controller

When data is read or written, the disk rotates and the read/write head moves to the appropriate sector.

This physical movement increases the access time.

### *Advantages of HDD*

- Low cost per gigabyte
- Large storage capacity

### *Disadvantages of HDD*

- Contains mechanical parts
- Produces noise
- Slower than SSDs
- Sensitive to physical shocks


## 6. SSD (Solid State Drive)

An SSD (Solid State Drive) is a semiconductor-based storage device that stores data in NAND flash memory cells.

It contains no moving parts.

Since data is accessed electronically, the access time is much lower than that of an HDD.

### *Advantages of SSD*

- High read/write speed
- Low power consumption
- Silent operation
- Shock resistant
- Low access latency

### *Disadvantages of SSD*

- Higher cost per gigabyte
- Flash memory cells have a limited write lifespan


## 7. Defragmentation and TRIM

### *Defragmentation*

Over time, files on an HDD may become scattered across different areas of the disk. This is known as fragmentation.

Fragmentation causes the read/write head to move more frequently, reducing performance.

Defragmentation rearranges fragmented files into contiguous blocks, reducing access time and improving performance.

### *TRIM*

Since SSDs have no moving parts, defragmentation is not necessary.

Instead, the operating system uses the TRIM command.

TRIM informs the SSD controller which data blocks are no longer in use, allowing them to be cleaned in advance.

As a result, write performance is maintained and the lifespan of the SSD is extended.