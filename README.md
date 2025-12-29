# Born2beroot

_This project has been created as part of the 42 curriculum by egaziogl._

## Description

Born2beRoot is an exercise 

### Requirements

_(More on this later.)_

Bonus requirements:
_(More on this later.)_

### The challenge

_(More on this later.)_

### Implementation

**VM initial setup:**
- I used a SanDisk Usb 3.0 for my virtual machine. It's not the best but I had it lying around and it worked out fine.
- I allocated 32gb to have plenty of room in case I do the bonus part & need more space than expected.

**Disk partitioning:**
- `/boot` gets its own physical volume, whereas root (`/`) and other directories will live under the same LV (`sda5`).
- After reading so many forum discussions and blog posts ([1](https://www.reddit.com/r/linuxquestions/comments/w2vsgs/what_space_should_i_allocate_to_root_and_home/), [2](https://dev.to/jabulani_meki_a537563a784/i-just-did-manual-partitioning-in-linux-and-it-wasnt-scary-at-all-36ne), [3](https://www.reddit.com/r/linux4noobs/comments/jeo8dd/how_much_space_should_i_allocate_for_each/), [4](https://www.reddit.com/r/linux4noobs/comments/mqf7kp/what_is_the_difference_between_boot_and_bootefi/), [5](https://www.reddit.com/r/linuxquestions/comments/17bon4l/separate_partitions_for_var_tmp_and_home_why/), [6]()), the general consensus for the initial partitioning seems to be:
    - No separate partitions for `/bin`, `/usr`, `/dev` or `/media`, `/lib`.
    - Yes for `/swap`, `/home`, `/var`, `/srv`, yes `/tmp`.
    - a separate LV for `/boot` (EFI partition),
    - 2Gs for `/swap` (4 if extra performance is needed), 
    - a set amount of Gs for `/` (mileage may vary depending on necessity, but 10G seems enough), 
    - and the rest for `/home`.


## Instructions

### Compilation

_(More on this later.)_

### Integration

_(More on this later.)_

### Testing

_(More on this later.)_

## Resources

- [This post (AskUbuntu)](https://askubuntu.com/questions/3596/what-is-lvm-and-what-is-it-used-for)
- [Followed by this LVM Howto (The Linux Documentation Project)](https://tldp.org/HOWTO/LVM-HOWTO/)
- [The Linux Filesystem Explained (The Linux Foundation)](https://www.linuxfoundation.org/blog/blog/classic-sysadmin-the-linux-filesystem-explained)
- [Swap space (Red Hat docs)](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/storage_administration_guide/ch-swapspace#tb-recommended-system-swap-space)
- [FHS (Wikipedia)](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)


## Project Description

[See the "Implementation" section above.](#implementation)