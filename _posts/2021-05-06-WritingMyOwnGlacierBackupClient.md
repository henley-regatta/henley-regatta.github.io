---
layout: post
title:  "Writing my own Glacier Backup Client in Python"
date:   2021-05-06 18:00 +0100
categories: systemsmanagement
tags:  amazon aws glacier python backup
---

1. TOC
{:toc}

## Because life is too short... write your own backup client.
There's any amount of clients already out there that'll do proper backup/recovery
work, and lots of _those_ work with Cloud service providers too. I'm even using
(at least) one on my NAS to do just that.

But sometimes, sometimes, you've got to DIY to understand what it is you want
and need.

This post is written in the order in which I tackled the problem, which is almost
certainly different from the natural order of things.

## tl;dr - show me the code

OK, it's here on [GlacierBackup repository on Github](https://github.com/henley-regatta/GlacierBackup)

BUt before you disappear: this post is most of the documentation for the code...

## The Requirement, Specified.
1. Create a LOCAL optimised (incremental) Backup
    1. Define a set of directories / file filters to include in a backup
    1. Compare this to any previously-created / uploaded archive
    1. Construct a "delta" archive that only reflects the changes
    1. (encrypt the delta)
1. Check the Glacier Vault usage isn't above a defined threshold
    1. Check the size of the Glacier Vault against a defined limit
    1. If the Vault size + size of archive to be uploaded exceeds threshold, prune
        1. Look for archives OLDER than the "free deletion" threshold
        1. Delete those archives oldest-first until we're under the size threshold.
1. Send the optimised backup to Glacier

(You'll note there's nothing here about getting stuff _back_ from the vault....)

### Requirement Breakdown Analysis
The requirement splits nicely into two almost-separate parts:

1. Creation of an optimised, encrypted local archive
1. Uploading of that archive to a defined Glacier Vault with proper space-management
applied to ensure size limits aren't busted (...because that would cost actual money)

One could easily split the project into 2 distinct programs to tackle each
problem separately, and a "scalable" solution absolutely would, if only to
abstract out the "cloud service" specific stuff to let another storage solution
be used.

Also worth noting: I'm doing this in Python for two main reasons:
1. It's nice and quick and easy and as close to self-documenting code as it
comes
1. There's a handy library available covering most of the AWS services we need
_and_ Amazon themselves have provided [some very useful code examples](https://docs.aws.amazon.com/code-samples/latest/catalog/code-catalog-python-example_code-glacier.html)
doing most of what we need from Glacier

## Preparing the Amazon Environment
This project was written for home / demo / educational purposes, and I don't want
to spend much (any?) money doing it. So the implementation choices made have that
as an (unwritten) critical non-functional requirement.

Most accounts you'll find on the web have endless `aws-cli` commands and inline
JSON specifications to do much of the following. I found you can actually just
use the AWS Web Consoles to do everything I've listed, which makes life _much_
easier...

1. Create an AWS Account. I signed up for the "home projects" tier; you'll need
to give them a credit card but _hopefully_ that's only for identity-verification
purposes.
    1. Good practice: Setup multi-factor authentication (MFA) for the account.
    I used Google Authenticator for this.
    1. You may wish to change your default _Region_ here to whatever makes sense
    for you.
1. Sign up for S3 Glacier and create a Vault for use by this project
    1. Change _Settings_ for S3 Glacier Vaults to use the "Free Tier Only" for Retrieval policies
    1. Setup Notifications for the Vault with a unique Topic Name and enable
    all possible job types to trigger notifications.
    1. Make a note of the _Vault ARN_ here as you'll need it for the resources
    part of the restricted policy you'll grant.
1. Create and restrict a _User_ to work only with the Vault(s) (nb: Using the
  **AmazonGlacierFullAccess** policy is fine but grants access to _all_ Vaults;
  since we're mucking about and the risk of programmatic carnage is high, it's
  better to create a user with access to only a single Vault. [Amazon themselves
  have more and better examples of restrictive policies you may wish to consider](https://docs.aws.amazon.com/amazonglacier/latest/dev/access-control-identity-based.html#vault-access-policy-example-full-permission))
    1. Go to IAM -> Policies, create a new Policy
        1. Restrict the policy to the **Glacier** service
        1. Grant all actions to the policy (..or limit to taste...)
        1. Set the Resource for the policy to the ARN of the Vault created above
    1. Go to IAM -> User Groups, create a new Group
        1. Assign the Permissions Policy you created in the step above here
    1. Go to IAM -> Users, create a new User
        1. Set _Access type_ to **Programmatic access**
        1. Assign the user to the Group you just created
        1. After creating the user, _download the CSV_ and record:
            * The Access Key ID
            * The Secret access key
1. Make sure you sign up for Notifications. Launch the Simple Notification Service  
dashboard and create a _Subscription_ for the **topic** you created above during
Vault creation. Probably Email notification works best but you could use SMS or
something else as you fancy.

## Preparing the Python environment
There's a standard library available for working with AWS services in Python
called **boto3**. On most Linuxes - certainly on Debian-derived ones - setting
this up is as simple as:
{% highlight bash %}
sudo apt install python3-boto3
{% endhighlight %}

However, once you've done that you'll want to let the Python environment have
access to the IAM account you've setup, so create the `~/.aws` directory with
the following files, or [follow the guide for alternative methods of configuration](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/credentials.html):
#### ~/.aws/credentials
{: .no_toc}
{% highlight ini %}
[default]
aws_access_key_id = ACCESS_KEY_OF_IAM_USER_CREATED_ABOVE
aws_secret_access_key = SECRET_KEY_OF_IAM_USER_CREATED_ABOVE
{% endhighlight %}

#### ~/.aws/config
{: .no_toc}
{% highlight ini %}
[default]
region= REGION_CONFIGURED_AS_DEFAULT_FOR_VAULTS
{% endhighlight %}

**n.b:** if you don't know what region to use, it's encoded in the ARN of the
Vault created above.

### Testing the python environment
At this point it's probably a good idea to use the [Amazon-provided example code](https://docs.aws.amazon.com/code-samples/latest/catalog/python-glacier-list_vaults.py.html)
to list available vaults and test it provides the output you expect:
{% highlight bash %}
exleym@hotboi:~/glacier-examples $ python list_vaults.py
DEBUG: 2021-05-06 15:01:00,142: Changing event name from creating-client-class.iot-data to creating-client-class.iot-data-plane
DEBUG: 2021-05-06 15:01:00,154: Changing event name from before-call.apigateway to before-call.api-gateway
...
...
DEBUG: 2021-05-06 15:01:00,600: Event needs-retry.glacier.ListVaults: calling handler <botocore.retryhandler.RetryHandler object at 0xb5d4a550>
DEBUG: 2021-05-06 15:01:00,600: No retry needed.
INFO: 2021-05-06 15:01:00,603:   0             0  PiBackup
INFO: 2021-05-06 15:01:00,603:   0             0  QnapBackup
{% endhighlight %}

..And there, buried deep in all the debug output, is the list of all the Vaults
I've created. Note that even though I've restricted the IAM account's **access rights**
to only the one I intend to use for this project (`PiBackup` in this case), I left
"query rights" for Glacier-as-a-whole as default so it can still see what vaults
and what size/access permissions etc apply across the whole of Glacier. That can,
and probably should be, restricted if you did this in a production environment...

## Understanding the Backup requirement
It's important to dig into the local backup requirement a little to make sense
of what a solution would look like.

### Defining the backup increment
What appears to be the key requirement is to produce, for each time the backup is
run, the smallest-possible backup increment file to be uploaded to a safe storage
location. If the inputs haven't changed[^1], then "Backup N" would ideally consist
only of the _bytes_ different in all files modified/added since "Backup N-1" was
taken.

However this is a very fragile state of affairs since the ability to _restore_
files then requires restoration of every backup ever taken with the imposition on
top of all deltas to get to the "final state". So it's slow, too. And, for various
programming / optimisation reasons it's also quite hard to write such a backup too.

Instead we'll adopt a more reasonable heuristic: Backup N will consist of a copy
of all files added/changed since Backup N-1 was taken. We'll use compression to
make up some of the difference. And this heuristic enables a more useful practical
pattern which is that we can take a full backup at any time and know that this
resets how far back in time we have to go to look for "base files".

### Generating the backup increment - what should be included
However all of this does require us to hold state - Backup N depends on knowing
what was in Backup N-1. We either have access to the _contents_ of N-1, or we
retain the _metadata_ of N-1 and use that to determine what's changed. Generally
holding the metadata is more space and processing efficient for most metrics
used to determine "different". For this project we'll just say a file has
altered if:
* File does not exist in previous backups
OR
* File exists in previous backups but has a different signature than that stored

"Signature" here could be a filesystem metric like mtime, size, atime or attributes
but as we're lazy we'll use a hash of the file instead. In practice this is good
enough to capture all actual file changes, but if only e.g. permissions or
ownership is changed we'll miss that. We can live with this.

### Zooming out - what should be in the backup set in the first place?
We also need to identify what files are "candidates" for being included in
the backups in the first place. Proper full-scale backup clients allow specification
across a number of different criteria - "all photos on the system" down to "files
less than 1MB in this directory that aren't .pdfs" and everything in between.
For our purposes we'll use a specification of **global** include and exclude
statements that are intended to backup whole directories _except_ for certain
file types or subdirectory names. So our specification will look something like:
{% highlight json %}
{
  "includes" : ["/path/to/first/dir", "/path/to/second/dir", "/this/specific/file.txt"],
  "excludes" : ["*.tmp", ".git", "/etc/passwd"]
}
{% endhighlight %}

Worth noting that the implementation is a bit more limited than the spec above
makes it appear - for ease-of-implementation I assume the `excludes` is either
a directory or, if it starts with `*` it's a file extension. There's no explict way
to force a particular file _name_ to be excluded, but specifying it as `*exclude.this.file.txt`
would work as a side-effect.

## Re-stating the backup requirement as pseudocode
With all of the above understood we can re-write the backup part of the
requirement as pseudocode:
{% highlight python %}
  read the json includeexclude list
  for each filespec in includes :
    walk the directory tree from that filespec
    for each filefound in spec:
      NEXT if filefound matches an excludes spec
      OTHERWISE calculate filehash for filefound and store in "currenthashes"

  if a "previoushashes" store can be read AND
     if the number of successive incrementals is at the threshold:
        write a flag-file out to indicate to external programs that this
        backup set is now complete.
     the number of successive incrementals is below a threshold :
    for each file in "currenthashes" :
      if file.hash != previoushashes.file.hash :
        add file to backupset
  else:
    backupset = every entry in "currenthashes"

  Create a single tar archive using backupset as input
  Compress the tar archive
  Encrypt the compressed tar archive using local credentials

  Write the "currenthashes" store to a file marked "previoushashes"
{% endhighlight %}  

Implementation of this pseudocode results in basically usable local backup
utility all on it's own - you end up with a local directory that fills with
full and incremental backup archives all encrypted on top. Quite neat.


## Drilling down into working with Glacier as a long-term archive store
There's a number of nuanced implementation details that make actually working
with Glacier storage (in particular) a little more tricky. Not least of all is
the fact that most operations - _apart_ from actually sending it files - are
both asynchronous and _slow_. Which is why you want notifications. Most of what
we will want to do with Glacier (apart from actually send a backup) should
probably be hived-off to a separate script.

Amazon are [pretty good at documentation](https://docs.aws.amazon.com/amazonglacier/latest/dev/introduction.html)
but the main points to pick out in terms of this use-case are:
* All operations within Glacier occur with an *Vault*. These are located in a _Region_
and can be thought of as analogous to a filesystem or block-device.
* Within Vaults, all data (files, archives, photos, videos etc) are handled as *Archives*.
    * When a file/archive/photo/video is uploaded as an _Archive_, a unique *ArchiveID* is returned
    which identifies the object created within the vault.
* Retrieval of an _Archive_ is asynchronous: A request is made, a job created, and on
completion of the job the bytes within the Archive can be downloaded.
    * The length of time between making the request and the job completing is determined
    by the option chosen. Faster retrieval is more expensive...
    * The first 10GB data retrieved per month using the "Standard" retrieval option
    is free.
    * "Standard" data is typically available 3-5 hours after requesting it
    * To keep within a download allowance, range-retrieval options can be used to
    split downloads into 1MB chunks. This probably isn't useful for backup/recovery.
* Archives stored in Glacier have a minimum 90 day retention period. Archives
deleted prior to that 90 day window incur additional storage charges.
    * Each archive has a 32Kbyte metadata overhead that is included in the billable
    size.
* Asynchronous jobs can either be polled for status or SNS can be used to initiate
actions when completion is notified.
    * Programmatically the `ListJobs` and `DescribeJob` APIs can be used to query
    the state of ongoing jobs
    * Completed job status and output - e.g. an updated Inventory, or an actual retrieved
    file - are available for 24 hours.

### But the implications, though....
The consequence of all of the above has some implications for our intended low-cost /
low-volume backup implementation:

1. We absolutely do not want to bust our total storage threshold. So we need a
total size cap for the vault.
    1. By design, working out the actual usage of a Vault - retrieving it's **Inventory** -
    is an asynchronous task with *~hours* latency. So we'll probably want to maintain a
    longer-term cached record of everything we've uploaded there for in-the-moment
    calculations on how close we are to the size cap, with infrequent/longer-term
    refreshes from the actual retrieved Inventory details.
1. Managing to that cap will require proactive activity: If we want to upload
an archive _now_ we need to have cleared out the space for it _earlier_ because
getting updated Inventory and deleting Archives is asynchronous and with assumed
longer delays.
    1. The practical implication is that we need to maintain the Vault size such
    that there's always room for one more backup before hitting the size cap.
    1. And if we want to be able to backup _now_ we're going to need to run a
    "maintenance/pruning" thread periodically to ensure this condition is true.
1. Since only an "Upload" activity is synchronous (_running in-line with code calls where a return code can be trusted as complete_),
performing all the inventory update / delete activities should be tackled offline from
the local backup activities.
    1. This is the driver that really says local backup should be a separate process -
    indeed a separate script - from the upload-backup-to-cloud process. Because,
    fundamentally, we can't guarantee from moment-to-moment that the cloud vault
    is ready to receive a new backup, there may be pruning to perform first...
1. The implication of the 32Kb per Archive overhead for total size use is fairly
insignificant for large backups *but* certainly in this demo environment the
incremental backups would actually be dominated by the metadata overhead if
they were backed up individually. So we probably want to batch together backups
into larger chunks anyway.
    1. This actually naturally ties in with the fact that our local backup process
    has been split into a series of incrementals followed by a full backup. In that
    for this low-volume/low rate-of-change use case we probably only want to upload
    a batch of backups as a complete set of full+all incrementals.
    1. Some care will need to be taken given that the two are separate processes
    to make sure we don't get out-of-sync - uploading partial incremental sets
    that don't tie to a related full backup. Need to think about how to manage
    that...

### Approaches to working with the asynchronous tasks
Fundamental to meeting our requirements is a way of dealing with Glacier's
asynchronous operations, particularly around Inventory management. There are
two basic strategies for this:

1. Polling. Have your local script make occasional calls to Glacier to check
on job status and for jobs that have recently completed take whatever closure
or completion actions are required
1. Event Driven. Use SNS to have an always-running script notified when the
requested job completes and handle appropriately.

There's no doubt that proper big-boy programs intended to be full-time backup
suites for professional or serious use would be event driven. However for our
purposes we can exploit the fact that we don't need to get to the job results
_immediately_ after completion - Amazon holds them for 24 hours - to implement an
infrequently-scheduled repetitive job processor that handles things that way.

Whether handling a locally-cached list of jobs and states is harder or easier than
handling event notifications from SNS is an open question, although the fact that
Amazon provides code samples for (effectively) handling job polling status in
Python does _somewhat_ make our approach a bit more understandable...


## Re-stating the Cloud Storage requirement as psuedocode
With the assumption that we're writing a script that will be called periodically
(_i.e. scheduled via e.g. cron_), we can restate the requirement for cloud
operations as the following:

{% highlight python %}
   Keep a local cache of stored Archives in the Vault
   Keep a record of the last Inventory retrieved from the Vault
   Keep a local store of active JobIDs

   FOR EACH job in local cache with a status other than "completed"
      Retrieve latest job status from Amazon.
      IF status is now completed:
        remove job from local cache of JobIDs
        push jobID onto list of jobs to retrieve results for

  FOR EACH jobID on the list of completed jobs:
    Check the job type:
      IF job type is "Inventory Retrieve":
        retrieve job results
        Update record of local inventory
        Reconcile cached-upload Archive store with retrieved Inventory details

  Calculate Vault Size based on cached Archive summary
  IF Vault Size > (configured threshold - last full backup size) :
    WHILE Vault Size > (threshold - last full backup size) :
        IF Oldest Archive is older than the age threshold :
          Submit DELETE job for oldest archive
          Calculate Vault Size as minus deleted archive size
        ELSE :
          Abort script with an error (protect size budget over cloud backup recency)

  IF the local backup-set-complete flag file is present:
    Create a single archive file of all local backups in the cache dir
    Encrypt the single archive with local encryption key
    Upload the archive to the configured Glacier Vault
    IF Upload successful :
      Delete the local single archive
      Delete the source archives going into that single archive
      Delete the backup-set-complete flag file.
      Update the local Vault Inventory Cache with the new Archive created

{% endhighlight %}

## Implementation Notes
The first set of `InventoryRetrieve` jobs I ran returned "empty" inventories - valid
JSON but without any data in the `ArchiveList` array and with a value of `InventoryDate`
that's a default (1970). This was both before and after actually uploading some data
to the Vault. This is _broadly_ in-line with [Amazon's documentation](https://docs.aws.amazon.com/amazonglacier/latest/dev/vault-inventory.html#vault-inventory-about)
which says it can be up to about a day-day and a half before a valid Inventory is
generated.

A second try at it a day or so later did actually update the Inventory with contents.

###Update 2021-05-23
Would it surprise anyone to find that the documentation's not 100% accurate?
Experience so far shows that no matter how often you _request_ an updated
inventory from Amazon, the server-side updates seem to happen no more than ~weekly.
So I've adjusted my code such that even if the InventoryDate is stale (the previous
trigger I was using for updates), it won't request a new one (pointless) for a
configurable further period. 

### End.
{: .no_toc}

***
[^1]: If you translate "inputs" to "A list of file/directory specifications to include in the backup set"
