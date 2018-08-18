# Android creash

## Android StrictMode

```kotlin
if (IS_DEBUG) {
StrictMode.setThreadPolicy(StrictMode.ThreadPolicy.Builder()
//                    .detectCustomSlowCalls()
//                    .detectDiskReads()
//                    .detectDiskWrites()
//                    .detectNetwork()
//                    .detectResourceMismatches()
//                    .detectUnbufferedIo()
                    .detectAll()
                    .penaltyDialog()
                    .penaltyLog()
                    .build()
            )

            StrictMode.setVmPolicy(StrictMode.VmPolicy.Builder()
//                    .detectActivityLeaks()
//                    .detectCleartextNetwork()
//                    .detectLeakedClosableObjects()
//                    .detectLeakedSqlLiteObjects()
//                    .detectLeakedRegistrationObjects()
//                    .detectFileUriExposure()
//                    .detectUntaggedSockets()

                    .detectAll()
                    .penaltyLog()


                    .build()
            )
}

```

## reference

- [美团外卖Android Crash治理之路](https://tech.meituan.com/waimai-android-crash.html)
- [Android 严苛模式 StrictMode 使用详解](https://juejin.im/entry/576b7421d342d30057ac72ab)