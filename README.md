Cloud Foundry Stacks
====================

This repo contains scripts for creating warden root filesystems.
* cflinuxfs2 derived from Ubuntu 14.04 (Trusty Tahr))

# Dependencies

* Docker
* Ruby 1.9.3-p545 or higher

# Adding a new package to the rootfs

`cflinuxfs2/build/install-packages.sh` has a list of packages passed to `apt-get install` as well.

# Creating a rootfs tarball

To create a rootfs for the cflinuxfs2 stack:

```shell
make
```

This will create the `cflinuxfs2/rootfs.tgz` file.

# Uploading to s3 bucket

s3 bucket is used by [warden-test-infrastructure](https://github.com/cloudfoundry/warden-test-infrastructure), so it needs to be uploaded there.

# Uploading cflinuxfs2 Stack to Docker Hub and S3 bucket

```shell
export AMAZON_ACCESS_KEY_ID=your-aws-id
export AMAZON_SECRET_ACCESS_KEY=your-aws-key

export DOCKERHUB_USERNAME=your-docker-hub-name
export DOCKERHUB_PASSWORD=your-docker-password
export DOCKERHUB_EMAIL=you@dockerhub-email.com

bundle install
./bin/upload_stack
```

This will use the `cflinuxfs2.tar.gz` file created with `make` and upload it to S3 and the Docker Hub. It will also create a receipt file named `cflinuxfs2/cflinuxfs2_receipt` that contains the SHA sum of the rootfs, the Docker image ID, and the ETag value from S3.

This will remove the `cflinuxfs2/rootfs.tgz` file, but the tarball `cflinuxfs2.tar.gz` will remain.

# Updating rootfs blob in cf-release

To update rootfs package in cf-release overwrite rootfs blob cf-release/blobs/rootfs/[ROOTFS_NAME].tar.gz with the new tarball.

Run `bosh upload blobs` to upload package to bosh blobstore.

After cf-release is updated with the new rootfs, future DEAs will automatically use that rootfs for its warden containers.

# Downloading from S3

http://cf-runtime-stacks.s3.amazonaws.com/cflinuxfs2.dev.tgz
