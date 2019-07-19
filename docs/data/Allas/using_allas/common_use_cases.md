
## Common Use Cases

&nbsp;


### Processing data

To use the computing environment, you need to use the open-source, parallel file system [Lustre](http://lustre.org/){:target="_blank"}.

* **Copying data from object storage to Lustre (stage-in):** You need to copy data to the parallel file system (Lustre) before computing. This can be done with the stage-in mechanism described in the User Guide. (_Insert link_)

* **Copying data from Lustre to object storage (stage-out):** After computation you should copy the files to Allas. The stage-out mechanism is described in the User Guide. **Please note:** your files will be automatically removed from Lustre scratch if they have not been touched for 90 days.

&nbsp;


### Sharing data

With object storage you can easily share data, e.g. datasets or research results. You can share these with other projects or open up access to everybody.
 
The data can be accessed and shared in several different ways:
 
* **Private - default:** By default, if you do not specify anything else, contents of buckets can only be accessed by authenticated members of your project. **Private**/**Public** settings can be managed with:
	* [Web client](./web_client.md#web_public){:target="_blank"}
	* [Swift client](./swift_client.md#temp_urls){:target="_blank"}
	* [S3 client](./s3_client.md#s3cmd_public_objects){:target="_blank"}
 

* **Access Control Lists:** Access Control Lists (ACLs) work on buckets, not on objects. With ACLs you can share your data in a limited fashion to other projects. You can e.g. enable authenticated read access to your datasets to a collaboration project.

 
* **Public:** You can also have ACLs granting public read access to the data, which is useful for e.g. sharing public scientific results or public datasets.

 
* **Temp URLs:** A temp URL is a unique URL to access an object. These URLs can be time limited. Anybody with the URL can access the object, but the URL is not feasible to just guess. This is a good way to somewhat securely share data to a limited audience, who do not need to have their own Allas accounts. Temp URLs are created per object, not per bucket. Temp URLs are a feature of Swift. In S3 the corresponding feature is called _a Signed URL_. Please see the differences between using the [Swift and S3 protocols](../accessing_allas.md#protocols){:target="_blank"}.

&nbsp;


### Using Allas in Puhti and Taito

The first step to use Allas is to authenticate to a project in Allas. In Taito and Puhti you can do the authentication with command:

    [user@puhti ~] source /appl/opt/allas_conf

The command above generates and stores authentication information into shell variables OS_AUTH_TOKEN and OS_STORAGE_URL. The authentication is valid for max 3 hours.  

**Please note:** The environment variables are available only for that login session so if you log into Puhti in another session, you need to authenticate again there to use Allas.

You can find some guidance for using Allas in Puhti and Taito below:

 * **Using Allas to move data between CSC computing environments:** [Quick and safe: a_put, a_get, a_find](../../a_commands.md){:target="_blank"}


 * **Accesing data in Allas directly from Puhti or Taito computing environment:** [Advanced tools: Rclone and swift](../../rclone.md){:target="_blank"}


 * **Persistent connections to Allas:** [Persistent Allas connections: s3cmd](../../s3cmd.md){:target="_blank"}

&nbsp;

 
### Static web content

A common way to use object storage is to store static web content there (images, videos, audio, pdfs, downloadable content), and just add links to it from your web page, which can run either inside Allas or somewhere else. [Here is an example](https://object.pouta.csc.fi/my_fishbucket/my_fish){:target="_blank"}

Uploading data to Allas can be done with any of the clients: [Web client](./web_client.md){:target="_blank"}, [Swift client](./swift_client.md){:target="_blank"} or [S3 client](./s3_client.md){:target="_blank"}.
 
&nbsp;


### Storing data for distributed use

There are several cases where you need to access to common data from several locations. In these cases, the practice of staging in data to individual servers or computers from the object storage can be used instead of shared file storage.

&nbsp;


### Accessing the same data from multiple CSC platforms

Since the data in object store is available from anywhere, you can access the data from both the CSC clusters and from the cloud services. This makes the object store a good place to store data and intermediate and final results in cases where your workflow requires the use of both Taito and Allas, for example.

&nbsp;


### Collecting data from different sources

It is easy to push data to object storage from several different sources. This data can then later be processed as needed.


An example of this is several data collectors pushing data to be processed. These could be, for example, scientific instruments, meters or software which harvest social media streams for scientific analysis. These can push their data into object store, and later virtual machines, or compute jobs on Puhti can process this data.
 
&nbsp;


### Self-service backups of data

Object storage is also often used as a location where you store backups. It is a convenient place to push copies of such as database dumps.

&nbsp;


### Viewing usage

If you are using _s3cmd client_, you can check your project's Object Storage usage with command:

	s3cmd du -H

In the case you use _swift client_, you can check it with command:
 
	swift stat

To see how much space a bucket has used
 
	swift stat $bucketname

Please contact servicedesk@csc.fi if you have questions.

&nbsp;


### Using Allas for sensitive data

Sensitive data can be stored in the Allas provided that the files are encrypted before uploading to the CSC storage system. CSC will not take responsibility of the encryption key management - our platforms cannot be used for storing these keys and transfer of keys from data owner to the user must be made outside of the CSC infrastructure. The buckets are available on the Internet and can be shared using the same processes that apply to all data stored in the Allas service. However, these data cannot be used as part of the CSC sensitive data services (as CSC cannot have access to the encryption keys).