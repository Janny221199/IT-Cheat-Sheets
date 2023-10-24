- Redundant Array of Independent Disks
- combines multiple physical disk drive components into one or more logical units for the purposes of data redundancy, performance improvement, or both

# RAID 0
- 2 or more separate disks, but no mirroring, both keep separate data (called striping)
- problem: if one disk fails, data can be loss
- advantage: good for performance because access to two independent disk controller is faster
- often in combination with other RAIDs


# RAID 1
- 2 or more separate disks, but mirroring, both keep the same data
- advantage: redundancy and prevent data loss if one disk fails
- problem: needs more storage for duplicate or times n replication

# RAID 5
- 3 ore more separate disks, data is distributed across multiple disks but not mirrored / duplicated, but uses "parity" to be able to reconstruct data with n-1 disks, so one can fail
- advantage: one disk can fail without data loss and good performance because access to two independent disk controller is faster
- problem: parity information takes storage space and it is difficult to add more disks (due to parity information must be restructured across more disks)

# RAID 10
- combination of RAID 1 and RAID 0
- 4 or more separate disks, where data is separated in a set of two disks where one of it mirrors the other one
- advantage: combination of redundancy and performance of RAID 1 and RAID 0
- problem: needs more storage for duplicate or times n replication