# RemoteContiguousMappings (RCM)

RCM is a minor experimental extension of the CAPaging Linux kernel ([paper](https://www.cslab.ece.ntua.gr/~xalverti/papers/isca20_enhancing_and_exploiting_contiguity.pdf), [original source](https://github.com/cslab-ntua/contiguity-isca2020)) that tries to address the tradeoff between data locality and memory contiguity in NUMA machines and for one-node applications. The current logic in handling placement decisions is very simple:
- Get the biggest physical range from each node
- Sort the ranges by distance to the local node
- Iterate over the sorted list of ranges
- If current range is the local and if *range_len >= a \* vma_length*, place vma there
- Else if current range is remote and if *range_len >= b \* vma_length*, place vma there
- If no range meets the above criteria, place vma in the local range

where *a*, *b* are percentages (between 0% and 100%) and configurable through /proc/sys/vm/capaging_min_local_coverage and /proc/sys/vm/capaging_min_remote_coverage respectively. They can be set to the same value, or *b* can be set higher to make the condition for remote placements stricter.
