# slurm_batch
Examples on how to submit batch jobs in slurm.

## Preprocessing
Both submit scripts assumes that the input is a file containing the input files needed for submission.
By default, slurm doesn't allow job arrays larger than 1000, so if you want to submit all `com` files in a folder, you could first generate a file containing the name of all `com` files:

```
ulimit -s 65536 # Only needed if you get an error about too many arguments
ls -f *.com > files
split -l 1000 files
```

You will now have a series of files named `xaa`, `xab` etc. containing 1000 or less `com` files.

## Submit job array
[submit_g09_array](./submit_g09_array) is an example file of how to submit Gaussian09 jobs as a jobarray.
This method should be preferred over the below one, but the slurm setup might not always allow it.
The partition, number of cpu's, memory and program is hardcoded, but the file serves as an example.
The jobs in `xaa` can be submitted by `./submit_g09_array xaa`.

## Submit job node
[submit_g09_node](./submit_g09_node) is an example file of how to submit Gaussian09 jobs in cases where you need to reserve an entire node.
Since your calculations might not scale linearly with cpu's, the jobs are allocated on the reserved cpu's using [GNU parallel](https://www.gnu.org/software/parallel/), by Ole Tange.
The partition, number of cpu's, memory and program is hardcoded, but the file serves as an example.
The jobs in `xaa` can be submitted by `./submit_g09_array xaa`.
