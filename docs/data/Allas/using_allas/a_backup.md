# allas-backup: nearly a self-service backup

CSC does not provide a backup service as a free service for its customers. In this chapter, we describe a tool called _allas-backup_
that can be used to make backup copies of files and directories in Allas. However, this tool does not 
provide an actual backup service – the data is stored in only one location and one bucket in Allas. This bucket can be
removed by an authenticated user with a single command, irreversibly erasing all backups.  

The _allas-backup_ tool provides an easy-to-use command line interface for the [restic](https://restic.readthedocs.io/) backup tool.
_allas-backup_ automatically creates a project specific backup repository in the Allas storage service at CSC and uses it for cumulative backups.

Unlike data upload tools such as `a-put`, `s3cmd put` or `rclone copy`, allas-backup (or actually the _restic_ program) stores the imported data as a collection hash.

In order to use this tool, first open a connection to Allas:
```text
module load allas
allas-conf
```
A connection remains open for eight hours.

**BACKUP OPERATIONS**

Operations `allas-backup` can be used for:

 - `allas-backup <file_or_directory>`  or `allas-backup add <file_or_directory>`   
 	Adds a new backup version (snapshot) of a file or directory in the backup repository.

 - `allas-backup list`   
 	Lists the snapshots saved in the repository. The option **-last** lists only the latest versions of snapshots.
 
 - `allas-backup files <snapshot_id>`   
 	Lists the files the snapshot includes.

 - `allas-backup find <query>`          
 	Finds snapshots that contain a file or directory matching the query term.

 - `allas-backup restore <snapshot_id>`  
 	Retrieves the data of the snapshot to the local environment. 
	By default, stored data is restored to the local directory. Other locations can be defined with the *-target* option.

 - `allas-backup delete <snapshot_id>`  
 	Deletes a snapshot in the backup repository.
