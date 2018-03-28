1) install ec2-api tool if not installed
```
$ apt install ec2-api-tools
```
if return error like below
Required option '-O, --aws-access-key KEY' missing (-h for usage)
 
open aws console and create IAM use with the requried role and download access key and secret key
 
2) Set it as the environment variable. Perhaps, append the following to your ~/.bashrc

```
$ export AWS_ACCESS_KEY=AKIAI5I4EPN4KIMQ3Fa28Q_xyz
$ export AWS_SECRET_KEY=Md5YIBDX1mdDWjDSiEacPr_xyz
```

3) Get a date-sorted overview of all snapshots
```
$ ec2-describe-snapshots | sort -k 5
```

4) Delete all snapshots from the specified prior day of month

# list all snapshots   | sort by date | target month  | get snapshot ID  | delete snapshots 1 at a time

```
$ ec2-describe-snapshots | sort -k 5    | grep 2017-09-27 | awk '{print $2}' | xargs -n 1 -t ec2-delete-snapshot
```

Delete all snapshots from the specified prior month

```
 $ ec2-describe-snapshots | sort -k 5    | grep 2017-09- | awk '{print $2}' | xargs -n 1 -t ec2-delete-snapshot
```

Delete all snapshots for the month but keep the first four
-We can add sed 1,4d to the pipeline (in this example I keep the first four because I have 2 instances and 2 volumes on each instance):
 
As per blow suppose i have total december month 31 snapshot so it take first 1 to 4 other deleted

```
$ ec2-describe-snapshots | sort -k 5 | grep 2017-12- | sed 1,4d | awk '{print $2}' | xargs -n 1 -t ec2-delete-snapshot
```

Delete all snapshots except the current month 
It will not delete January data 

```
$ ec2-describe-snapshots | grep -v 2018-01- | awk '{print $2}' | xargs -n 1 -t ec2-delete-snapshot
```