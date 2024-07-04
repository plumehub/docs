# GRICAD

## Access to GRICAD

 - If you don't have an account, create it [here](https://perseus.univ-grenoble-alpes.fr/create-account/portal) (select chercheur in contract type if you have a permanent position or are a post-doc, doctorant if you are PhD) and ask to join the pr-plume project.
 - Read carefully the [computation center documentation](https://gricad-doc.univ-grenoble-alpes.fr/en/)
 - Once logged to dahu (CPU) or bigfoot (GPU), you have access to the storage at : ```/summer/plume```

## Configuration of your access to GRICAD

You can set up a direct access to dahu via [this procedure](https://gricad-doc.univ-grenoble-alpes.fr/en/hpc/connexion/#connecting-to-the-gateways-without-a-password) so that ssh and scp takes only one line of command, a simplified version is reported below :

  - Check that you should be able to connect to the bastions : ```ssh <your-login>@rotule.u-ga.fr``` and ```ssh <your-login>@trinity.u-ga.fr``` (replace <your-login> with your perseus account)
  - Once connected on one of the bastions, check that you can connect to dahu (for CPU) : ```ssh dahu``` or bigfoot (for GPU) ```ssh bigfoot```
  - To connect directly to dahu or bigfoot, add the following lines to your ```.ssh/config``` :

```bash
Host *
  ServerAliveInterval 30
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_rsa
  ForwardX11 yes
  TCPKeepAlive yes
 
Host *.ciment
  User <your-login>
  ProxyCommand ssh -q <your-login>@access-gricad.univ-grenoble-alpes.fr "nc -w 60 `basename %h .ciment` %p"
```

  - Now you should be able to connect to dahu directly with ```ssh dahu.ciment``` or to bigfoot with ```ssh bigfoot.ciment```
  - To connect without typing your password, you can set up a SSH key :
    - on your local machine create the ssh key : ```ssh-keygen```
    - copy the public key to the bastions : ```ssh-copy-id -i .ssh/id_rsa.pub <your-login>@rotule.u-ga.fr:``` and  ```ssh-copy-id -i .ssh/id_rsa.pub <your-login>@trinity.u-ga.fr:``` and to the clusters : ```ssh-copy-id -i .ssh/id_rsa.pub <your-login>@dahu.ciment:``` or ```ssh-copy-id -i .ssh/id_rsa.pub <your-login>@bigfoot.ciment:```


## Workspaces

 * On each cluster (dahu, bigfoot) you have a personnal worspace : ```/home/yourlogin```
 * A personnal scratch workspace accessible from both dahu and bigfoot : ```/bettik/PROJECTS/pr-plume/yourlogin``` with a quota of between 3 and 5 Tb per user.
 * same architecture for another scratch workspace called silenus instead of bettik
 * the summer data which is mirrored [online](https://ige-meom-opendap.univ-grenoble-alpes.fr/thredds/catalog/meomopendap/extract/MEOM/catalog.html) : ```/summer/plume``` ;it is advised to copy data from it locally so that the computation is faster (not the same filesystem)

### Connection

 * Now that you are all set up, you just have to type ```ssh dahu``` 
 * If you need to transfer some data yo can do a simple ```scp mydata dahu:/path/to/data/.```
 * If the data you want to transfer is big, go through the cargo server : ```scp mydata yourlogin@cargo.univ-grenoble-alpes.fr:/path/to/data/.```

### Submitting jobs

  * You are actually sitting on login nodes (f-dahu or bigfoot), to do some computation you will need to request some computing nodes
  * You do that by either launching your script inside a job or ask for interactive access to a computing node :

<details>
<summary>An example for interactive computing</summary>
 
 ```oarsub -l /nodes=1/core=16,walltime=03:30:00 --project pr-plume -I```
 
</details>

 * When your request is granted you will be connected to a specific dahu node and you will be able to compute there.
 * Maximum time limit is 12 hours
 * The memory allocated to your request is nb_cores_requested*node_memory/nb_cores_per_node, for instance on a classical dahu node there is a total of 192Gb per node, if you ask for 16 cores you will be granted 96Gb, on a fat node a total of 1.5Tb is available (check node properties with recap.py)


<details>
<summary>An example job</summary>
 
 ```
 #!/bin/bash

#OAR -n jobname
#OAR -l /nodes=2/core=1,walltime=00:01:30
#OAR --stdout jobname.out%jobid
#OAR --stderr jobname.err%jobid
#OAR --project data-ocean

yourscript
```
 
</details>

 * Make sure your job script is executable ```chmod +x job.ksh``` and then launch it with ```oarsub /path/to/the/job.ksh``` (you have to provide absolute path or be in the directory and type : ```oarsub -S ./job.ksh```

 * You can check the status of your job with ```oarstat -u yourlogin``` and kill your job if needed with ```oardel jobid``` with jobid being the first number in the result of oarsat

 * Maximum time limit on dahu is 2 days
 
 * If your code is not in the production phase yet, you can ask to test it first on a development queue by adding the option ```-t devel``` to your oarsub command or in your job with a maximum time limit of 30 minutes 
 
 * Another useful queue is the fat one (option ```-t fat```and provide access to nodes with a total of 1.5Tb of RAM per node)
 *  A queue called visu is also available
 
 * For more informations about jobs read https://gricad-doc.univ-grenoble-alpes.fr/en/hpc/joblaunch/

### See availability of Dahu nodes : 

 * Command ```chandler``` in the terminal or go to the website : https://ciment-grid.univ-grenoble-alpes.fr/clusters/dahu/monika for instaneous availablity or https://ciment-grid.univ-grenoble-alpes.fr/clusters/dahu/drawgantt/drawgantt.php for availability over time (history and forecast)

 
---

## Using NCO tools (+ncdump & ncview)

   * put this line in your .bash_profile : ```source /applis/site/nix.sh```
   * download the desired package with nix :

```
nix-env -i -A netcdf
nix-env -i -A nco
nix-env -i -A ncview
```
   * the binaries are now in ```/home/yourlogin/.nix-profile/bin``` and accessible from anywhere (your PATH is updated when you source the nix application)

---
