# Unpack the debirf ISO

## Unpack the contents of the debirf live ISO:
- Mount the image

	```shell
  sudo mount -o loop debirf-live_bullseye_amd64.iso /mnt/iso
  ```

- Create a destination for the compressed files

  ```shell
  mkdir ~/debirf
  ```

- Copy the initrd file to the new directory

  ```shell
  cp /mnt/iso/debirf-live_bullseye_6.0.0-0.deb11.6-amd64.cgz ~/debirf
  ```


  ```shell
  cd ~/debirf
  ```

- Change the extension of the file to be recognized by gzip

  ```shell
  mv debirf-live_bullseye_6.0.0-0.deb11.6-amd64.cgz debirf-live_bullseye_6.0.0-0.deb11.6-amd64.gz
  ```

- Uncompress the file

  ```shell
  gunzip debirf-live_bullseye_6.0.0-0.deb11.6-amd64.gz
  ```

- Unpack the cpio file

  ```shell
  cpio -idv < debirf-live_bullseye_6.0.0-0.deb11.6-amd64
  ```

- Change the extension of rootfs to be recognized by unxz

  ```shell
  mv rootfs.cxz rootfs.xz
  ```

- Uncompress the rootfs file

  ```shell
  unxz rootfs.xz
  ```

- Create a new directory to store the rootfs

  ```shell
  mkdir rootdir
  ```


  ```shell
  cd rootdir
  ```

- Unpack the cpio file

  ```shell
  cpio -idv < ../rootfs
  ```
