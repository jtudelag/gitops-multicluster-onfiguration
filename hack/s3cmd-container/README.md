# s3cmd-container

The repo `rhel-8-for-x86_64-baseos-rpms` must be enabled in the RHEL host in order to build this container image
because the s3cmd package has `python3-magic` as a dependency.

# Build the container

To build the container image, run the following command:

```bash
podman build -f Containerfile.fedora -t localhost/s3cmd:latest
```

# Apply expiration policy to a bucket

First we configure s3cmd, setting the config `~/.s3cfg` file:
```bash
cat ~/.s3cfg
[default]
access_key = YYY
secret_key = XXX
host_base = s3.example.io
host_bucket = s3.example.io
check_ssl_certificate = False
check_ssl_hostname = False
use_http_expect = False
use_https = True
signature_v2 = True
signurl_use_https = True
```

Then we first make sure the bucket exists:
```bash
s3cmd ls
2024-01-15 10:38  s3://test-b45d56f6-a99c-4bda-8956-4df50f3b44fb
```

We make sure there is no previous policy (LifecycleConfiguration):
```bash
s3cmd getlifecycle s3://test-b45d56f6-a99c-4bda-8956-4df50f3b44fb
<?xml version="1.0" ?>
<LifecycleConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/"/>
```

Weset the expiration policy, to 30 days in this case:
```bash
s3cmd expire s3://test-b45d56f6-a99c-4bda-8956-4df50f3b44fb --expiry-days 30
Bucket 's3://test-b45d56f6-a99c-4bda-8956-4df50f3b44fb/': expiration configuration is set.
```

Finally we check the policy has been applied correctly:
```bash
s3cmd getlifecycle s3://test-b45d56f6-a99c-4bda-8956-4df50f3b44fb
<?xml version="1.0" ?>
<LifecycleConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
        <Rule>
                <ID>a2523a74-1943-4e8b-ae8f-fa874083f8c3</ID>
                <Status>Enabled</Status>
                <Filter>
                        <Prefix/>
                </Filter>
                <Expiration>
                        <Days>30</Days>
                </Expiration>
        </Rule>
</LifecycleConfiguration>
```
