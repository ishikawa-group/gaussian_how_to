# TSUBAME
* In the following, we assume as the user is the Tokyo-tech student or employee.

# Making account
1. Login to the Tokyo-tech portal, and click "TSUBAME portal"
2. Fill the required fields
3. E-mail comes in moment, and the click the URL in the mail
4. Confirm the account name

# ssh key generation
1. Open the terminal in mac/windows-wsl and type `ssh-keygen`
2. Specify the name of public key
3. Set your passphrase if you like
4. Copy the texts in the above public key file
5. Back to the TSUBAME portal, and paste the copied text into the field of "SSH public key registration"
6. Click "add"
7. Go back to terminal, and you can login like `ssh [account_name]@login.t3.gsic.titech.ac.jp -i [private_key-file]`
8. Setup the ssh configure file `~/.ssh/config` to make login easier. An example of minimal configuration is,
    ```bash
    Host tsubame
        HostName login.t3.gsic.titech.ac.jp
        User your_name
        IdentityFile /Users/your_name/.ssh/id_rsa_tsubame
    ```
9. You can login TSUBAME by `ssh tsubame`

# Getting computational resources
* You need to make a group to get computational resources.
* This section involves the budget payment procedures, so often only instructors need to manage them.

## Making group
1. login to "TSUBAME-portal"
2. click the making group, and fill the required fields
3. you can manage the groups with "managing the group" -> "detail"

## Payment procedure
### Registering purchase code
1. you need the "purchase code" for paying the points
2. click "managing the purchase code" --> "apply for the new purchase code"
3. fill the required fields
4. wait
5. the purchase code will be issued, and then fill the budget information from "managing the group"

### Purchasing points
1. login to the TSUBAME portal
2. click "managing the group"
3. select the belonging groups
4. click "detail"
5. purchasing points

# Submitting jobs
## Normal
* You can execute your calculation as *jobs* to the supercomputer.
* Supercomputer queuing system takes care of your (and also others') jobs.
* To register your job, execute: `qsub -g [group_name] script.sh`
    + If you don't specify the group name, the job will be a trial-run so it stops in 1 hour.
* The `script.sh` file contains the procedure of your calculation.
* Eexamples of scripts are as follows:
    + Gaussian: input file is `h2o.com`
    ```bash
    #!/bin/bash
    #$ -cwd
    #$ -l node_o=1
    #$ -l h_rt=00:10:00
    #$ -V

    source /etc/profile.d/modules.sh
    module load gaussian16

    INP="h2o.com"
    PRG="g16"

    ${PRG} ${INP}
    ```
    + VASP
    ```bash
    #!/bin/sh
    #$ -cwd
    #$ -l node_o=1
    #$ -l h_rt=0:10:00
    #$ -N flatmpi

    source /etc/profile.d/modules.sh

    module load intel
    module load intel-mpi

    PRG="/home/1/uk02411/vasp/vasp.6.4.3/bin/vasp_std"

    mpiexec.hydra -ppn 1 -n 1 ${PRG} >& stdout
    ```

* You need to specify the **resource type** from the following table. The list can be found in https://www.t4.gsic.titech.ac.jp/docs/handbook.ja/jobs/

|  name   | #CPU | #GPU | Memory (GB) | Meaning         |
| :-----: | :--- | :--- | :---------- | :-------------- |
| node_f  | 192  | 4    | 768         | Using full node |
| node_h  | 96   | 2    | 384         | Using half node |
| node_q  | 48   | 1    | 192         | Using 1/4 node  |
| node_o  | 24   | 1/2  | 96          | Using 1/8 node  |
|  gpu_1  | 8    | 1    | 96          | Only GPU        |
|  gpu_h  | 4    | 1/2  | 48          | Only GPU        |
| cpu_160 | 160  | 0    | 368         | Only CPU        |
| cpu_80  | 80   | 0    | 184         | Only CPU        |
| cpu_40  | 40   | 0    | 92          | Only CPU        |
| cpu_16  | 16   | 0    | 36.8        | Only CPU        |
|  cpu_8  | 8    | 0    | 18.6        | Only CPU        |
|  cpu_4  | 4    | 0    | 9.2         | Only CPU        |

* Write the "name" to the above jobscript of `#$ -l f_node=N`, where N is the number of the specified node type.

# Confirming jobs
* To confirm your job status, type: `qstat`.
* Job states
    * `qw`: waiting for run
    * `r` : running
    
# Stopping jobs
* To stop your jobs, type: `qdel [JOB_ID]`.
* To know the JOB_ID, do `qstat`.

# Storage
* Each user has `$HOME` directory, which has 25GB.
* Users can purchase (from TSUBAME portal) the following strages for the group usage.
    + `/gs/fs/GROUP_NAME/`: SSD (fast access)
    + `/gs/bs/GROUP_NAME/`: HDD (fast access)
* You need to pay **every months** these group disks if you want to keep the storage
* It is better to put symbolic link to your home (or any) directory: `ln -s /gs/bs/tga-your-groupname bs`
