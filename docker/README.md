# Howto build droidmedia

This docker image allow you to simple build of droidmedia library inside docker image.
Please note that variables set in Dockerfile are for Xperia 10.II, you may need to modify
it for your device.

It is based on howto from [Dcaliste on SailfishOS forum](https://forum.sailfishos.org/t/camera2-api-development/14491/23)
and other posts in Camera2 API development thread.

Build docker image:
```bash
docker build -t droidmedia docker
```

Then you may switch the branch of droidmedia, prepare your own changes and build it in container:
```bash
docker run --name droidmedia-devel -v $(pwd):/workdir/external/droidmedia -it droidmedia

# in the container:
source build/envsetup.sh
lunch aosp_$DEVICE-user
make libdroidmedia
```

backup original library on the device:
```bash
ssh root@xperia
cp  /usr/libexec/droid-hybris/system/lib64/libdroidmedia.so /usr/libexec/droid-hybris/system/lib64/libdroidmedia.so.backup
```

deploy libdroidmedia.so to your phone:
```bash
docker cp droidmedia-devel:/workdir/out/target/product/pdx201/system/lib64/libdroidmedia.so . 
scp libdroidmedia.so root@xperia:/usr/libexec/droid-hybris/system/lib64/libdroidmedia.so
```

