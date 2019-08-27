## Compiling R on Puhti

Instructions for installing R from source on CSC's Puhti supercomputer

#### 1. Log into Puhti login node 3

This node is used for installation work.

```
ssh user@puhti-login3.csc.fi
```

**Notes:**

For this to work, it is necessary to add an ssh key to login node 1. The following steps will let you create a password-protected ssh key.

```
# On your local computer
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
scp -pr ~/.ssh/id_rsa.pub user@puhti-login1.csc.fi:/tmp

# on Puhti login 1
mkdir ~/.ssh
chmod 700 ~/.ssh
cat /tmp/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

#### 2. Switch to correct access group

R is installed into the `/appl/soft/math` folder. The 'install' access group (called `p_installation` on Puhti) has access to this. Run the following command to switch to the correct access group:

```
newgrp p_installation
```

**Notes:** To view available groups, use `id` (your current group is under `gid`).

#### 3. Check your umask

Setting the umask to 002 means that members of your group can create and modify files that you've created. Those outside the group can read the file, but cannot modify it.

```
umask 0002
```

#### 4. Download R into a temp directory

For example, `/local_scratch/build/USER` (create a directory using `mkdir` if you don't yet have one set up).

Then, in the folder:

```
wget https://cran.r-project.org/src/base/R-3/R-3.6.0.tar.gz
```

It could be another version, but using R 3.6.0 as an example here.

#### 5. Pull together libraries needed to compile R

This is where things get interesting. On Puhti, many of the external dependencies for R (and other software) have been installed using Spack. One way to make use of this for compiling R is to create a separate folder for dependencies using `spack view`. What this does is to create a collection of symlinks that can then be used to build R.

In addition a means of compiling C and Fortran 90, we need at least the following libraries: **bzip2, curl, PCRE, readline, xz and zlib**. We also need other libraries (libiconv, libX11 and libXt), but those come as part of the default system set-up on Puhti and we don't need to point towards those explicitly.

**Some notes before proceeding:**

- The Spack libraries available on Puhti have been installed under different GCC versions, and it is necessary to use libraries that aren't in conflict with one another. This set of instructions uses libraries installed using GCC 9.1.0, as that is also what we intend to use to compile R.

- It's good practice to create a separate symlink folder for each new R installation, since we cannot predict in advance when the contents of your dependency folder need to change in order to compile a new version.

- To keep things neat, we will also use separate folders for libraries needed for the initial compilation step vs. those needed for R package installations.

#### 5a. Create directories for R and its dependencies

Some of these steps are only necessary when doing initial installation work (i.e. when installing the very first version of R to be used on Puhti).

```
# General R directory
mkdir /appl/soft/math/R/

# General directory for R dependencies
mkdir /appl/soft/math/R/R_dependencies

# Directory for compilation dependencies
mkdir /appl/soft/math/R/R_dependencies/R_compile_dependencies/

# Directory for compilation dependencies for R 3.6.0
mkdir /appl/soft/math/R/R_dependencies/R_compile_dependencies/R_compile_dependencies_3.6.0/  
```

#### 5b. Add symlinks to the dependency folder

The first thing we need to do is to source Spack settings:

```
source /appl/opt/utils/spack-settings.sh
```

We can then find all available Spack libraries using:

```
spack find -l 

# can also be used to look for a specific library, e.g.
spack find -l zlib
```

To create a symlink using `spack view`, we need to type something following this general format:

```
spack view symlink FOLDER /IDENTIFIER

# A real-life example would be something like:
# spack view symlink R_compile_dependencies_3.6.0 /6hsrgbc
```

Using `spack find -l`, we can locate the following libraries and their corresponding identifiers:

```
nq5wt2f: zlib@1.2.11 (gcc@9.1.0)
vfi5ei6: curl@7.60.0 (gcc@9.1.0)
qxjiucd: bzip2@1.0.6 (gcc@9.1.0)
pvyb7uv: xz@5.2.4 (gcc@9.1.0)
pyytfb2: pcre@8.42 (gcc@9.1.0)
nonqgpv: readline@7.0 (gcc@9.1.0)

# Note! These are subject to change between installations.
```

To add these to the version-specific dependency folder we created earlier, first let's move into the general directory for compilation dependencies:

```
cd /appl/soft/math/R/R_dependencies/R_compile_dependencies/
```

Now we are ready to add the symlinks:

```
spack view symlink R_compile_dependencies_3.6.0 /nq5wt2f
spack view symlink R_compile_dependencies_3.6.0 /vfi5ei6
spack view symlink R_compile_dependencies_3.6.0 /qxjiucd
spack view symlink R_compile_dependencies_3.6.0 /pvyb7uv
spack view symlink R_compile_dependencies_3.6.0 /pyytfb2
spack view symlink R_compile_dependencies_3.6.0 /nonqgpv
```

**Notes:** We might also be interested in seeing what's already present on the system by default and where those libraries are located. For that, one could use `which LIB` (with LIB being the library of interest) and `rpm -q LIB` (this will tell you the version installed).

#### 6. Unpack the R file into the installation directory

After creating the dependency folder, let's unpack R from its temporary location into the actual installation directory:

```
cd
cd /local_scratch/build/USER
tar xvf R-3.6.0.tar.gz -C /appl/soft/math/R/

# Creates a directory called 'R-3.6.0' inside the R directory
```

#### 7. Load modules needed to compile R

We also need to load GCC, MPI and MKL modules to compile R. This is the set of modules used to compile R 3.6.0:

```
module load gcc/9.1.0
module load hpcx-mpi/2.4.0
module load intel-mkl/2019.0.4

# Later installations may use different settings. Always document
# any changes!
```

#### 8. MKL call

Browse into the installation directory, in the current case:

```
cd
cd /appl/soft/math/R/R-3.6.0
```

Then run the MKL export call given below. 

**Note before proceeding:**

The MKL call is subject to change and can be created using this link:
https://software.intel.com/en-us/articles/intel-mkl-link-line-advisor

Current choices for generating the call:

```
Select Intel(R) product: Intel(R) MKL 2019.0
Select OS: Linux
Select compiler: GNU Fortran
Select architecture: Intel(R) 64
Select dynamic or static linking: Dynamic
Select interface layer: 64-bit integer
Select threading layer: OpenMP threading
Select OpenMP library: GNU (libgomp)
```

 One could also use a 32-bit interface layer:

https://cran.r-project.org/doc/manuals/r-devel/R-admin.html#Choosing-between-32_002d-and-64_002dbit-builds

Current version of the MKL export call:

```
export MKL="-L${MKLROOT}/lib/intel64 -Wl,--no-as-needed -lmkl_gf_ilp64 -lmkl_gnu_thread -lmkl_core -lgomp -lpthread -lm -ldl"
```

#### 9. Run ./configure

Inside the installation folder, run this very long configuration call in a single go. The different flags are set to point toward the 'include' and 'lib' folders inside the dependency folder. Change the dependency folder path as necessary!

```
CFLAGS="-I/appl/soft/math/R/R_dependencies/R_compile_dependencies/R_compile_dependencies_3.6.0/include/ -O3 -march=native -fopenmp -m64" FCFLAGS="-I/appl/soft/math/R/R_dependencies/R_compile_dependencies/R_compile_dependencies_3.6.0/include/ -O3 -march=native -fopenmp -m64" CXXFLAGS="-I/appl/soft/math/R/R_dependencies/R_compile_dependencies/R_compile_dependencies_3.6.0/include/ -O3 -march=native -fopenmp -m64" LDFLAGS="-L/appl/soft/math/R/R_dependencies/R_compile_dependencies/R_compile_dependencies_3.6.0/lib/ -fopenmp -lcurl" ./configure --prefix=/appl/soft/math/R/R-3.6.0 --with-blas="$MKL" --with-lapack --enable-R-shlib --disable-R-profiling
```

**Notes:** Earlier installation guidelines did not include `-lcurl` as part of the `LDFLAGS` call. Adding this resolved an issue where `./config` was unable to find https support for libcurl.

#### More to come... probably also modifications to the above to enable extra features.

```
--with-cairo
--with-libpng  
--with-libtiff 
```
