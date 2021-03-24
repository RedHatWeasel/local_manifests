# AOSP based rom for Pixel5 (redfin)

Rom includes MicroG-Services, FDroid, AuroraStore, Firefox, K9Mail

VoWifi and VoLTE working


**Build for redfin:**
\
Initialise AOSP tree
```
mkdir aosp11 & cd aosp11
repo init -u https://android.googlesource.com/platform/manifest -b android-11.0.0_r32 --depth=1
git clone https://github.com/RedHatWeasel/local_manifests.git .repo/local_manifest

repo sync -c -j8 --no-tags --no-clone-bundle
```

Checkout lfs repository
```
pushd vendor/apps
git lfs fetch
git lfs checkout
popd
```

Prepare the build
```
source build/envsetup.sh
lunch redfin-userdebug
```

Grab vendor blobs
```
pushd vendor/prepare_vendor
./execute-all.sh -d redfin -b RQ2A.210305.006 -o tmp
cp -r tmp/RQ2A.210305.006/vendor/* ../
popd
```

Manually change file vendor/google_devices/redfin/product/etc/vendor.qti.hardware.data.connection-V1.0-java.xml to
```
<?xml version="1.0" encoding="utf-8"?>
```

Start the build
```
make -j16
```

flash to device (-w will erase userdata)
```
fastboot flashall -w
```
