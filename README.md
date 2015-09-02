# android-sdk-fat32lib

Forked to solve an issue with the JOBB tool distributed with the Android SDK as of today (Android SDK 23) not able to produce OBB files larger than 512M.

Typical backtrace
```
Slop: 0   Directory Overhead: 0
Slop: 305385   Directory Overhead: 10112
java.io.IOException: FAT Full (155947, 155948)
	at de.waldheinz.fs.fat.Fat.allocNew(Fat.java:298)
	at de.waldheinz.fs.fat.Fat.allocNew(Fat.java:351)
	at de.waldheinz.fs.fat.ClusterChain.setChainLength(ClusterChain.java:164)
	at de.waldheinz.fs.fat.ClusterChain.setSize(ClusterChain.java:132)
	at de.waldheinz.fs.fat.FatFile.setLength(FatFile.java:91)
	at de.waldheinz.fs.fat.FatFile.write(FatFile.java:154)
	at com.android.jobb.Main$1.processFile(Main.java:495)
	at com.android.jobb.Main.processAllFiles(Main.java:604)
	at com.android.jobb.Main.processAllFiles(Main.java:600)
	at com.android.jobb.Main.processAllFiles(Main.java:600)
	at com.android.jobb.Main.processAllFiles(Main.java:600)
	at com.android.jobb.Main.main(Main.java:417)
Exception in thread "main" java.lang.RuntimeException: Error getting/writing file with name: 42.txt
	at com.android.jobb.Main$1.processFile(Main.java:501)
	at com.android.jobb.Main.processAllFiles(Main.java:604)
	at com.android.jobb.Main.processAllFiles(Main.java:600)
	at com.android.jobb.Main.processAllFiles(Main.java:600)
	at com.android.jobb.Main.processAllFiles(Main.java:600)
	at com.android.jobb.Main.main(Main.java:417)
```

## References

* Original version available at https://github.com/waldheinz/fat32-lib/
* Modified version available at https://android.googlesource.com/platform/external/fat32lib/
* Stackoverflow issue http://stackoverflow.com/questions/18906055/what-causes-jobb-tool-to-throw-fat-full-ioexception

## How to use

(on OS X, `$ANDROID_SDK` is by default `~/Library/Android/sdk`)

1. Produce the library with `gradle jar`
2. Archive SDK version `mv $ANDROID_SDK/tools/lib/fat32lib.jar $ANDROID_SDK/tools/lib/fat32lib.jar.orig`
3. Put patched version in place `cp build/lib/android-sdk-fat32lib.jar $ANDROID_SDK/tools/lib/fat32lib.jar`

Now jobb should work on larger folders
