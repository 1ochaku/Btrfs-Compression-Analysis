# Btrfs Compression Analysis

## Purpose

This project aims to understand the purpose of file systems, focusing specifically on the compression feature of the Btrfs file system. The analysis was conducted to evaluate the impact of compression on storage efficiency and performance.

## Setup

The experiment was carried out using Ubuntu with a single partition as the base operating system. The following steps outline the setup process:

1. **Install Btrfs**:  
   To install Btrfs, use the command:
   ```bash
   sudo apt install btrfs-progs
   ```
   You can verify the installation by checking the version:
   ```bash
   btrfs --version
   ```

2. **Create Virtual Disks**:  
   Two virtual disks of 10GB size were created and the file system was loaded onto them. The steps for each are detailed below.

### Compression Enabled

```bash
sudo dd if=/dev/zero of=compressed.img bs=1G count=10
sudo losetup -fP compressed.img
sudo mkfs.btrfs -f /dev/loop27
sudo mkdir /mnt/btrfs_compressed
sudo mount /dev/loop27 /mnt/btrfs_compressed
sudo btrfs property set /mnt/btrfs_compressed compression zlib
```

*(To find the loop number, execute `losetup -a` to check the associated virtual disk name.)*

### Compression Disabled

```bash
sudo dd if=/dev/zero of=uncompressed.img bs=1G count=10
sudo losetup -fP uncompressed.img
sudo mkfs.btrfs -f /dev/loop28
sudo mkdir /mnt/btrfs_uncompressed
sudo mount /dev/loop28 /mnt/btrfs_uncompressed
```

3. **Verify Mounted Disks**:  
   Ensure both disks are mounted correctly by executing:
   ```bash
   df -h
   ```

## Creating Workloads

Next, we created two 1GB files in each of the folders created in the virtual disks.

### For Compressed Folder

```bash
sudo dd if=/dev/zero of=/mnt/btrfs_compressed/compressed_file bs=1M count=1024
nano vdbench_compressed.txt
sudo ./vdbench -f vdbench_compressed.txt -o output_compressed
```

### For Uncompressed Folder

```bash
sudo dd if=/dev/zero of=/mnt/btrfs_uncompressed/uncompressed_file bs=1M count=1024
nano vdbench_uncompressed.txt
sudo ./vdbench -f vdbench_uncompressed.txt -o output_uncompressed
```

## Results

After running the tests, we evaluated space usage, performance, and CPU usage. The following results were observed:  
*(Insert your results and any figures here)*

## Conclusion

This analysis provides insights into the benefits and drawbacks of enabling compression in the Btrfs file system, demonstrating significant space savings with associated trade-offs in CPU usage and write speed.
