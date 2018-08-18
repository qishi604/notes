# Android Studio attach sdk source

Edit 

```
~/Library/Preferences/AndroidStudioBeta/options/jdk.table.xml
```

如下面是将28的source url 设置为27的

```
<jdk version="2">
      <name value="Android API 28 Platform" />
      <type value="Android SDK" />
      <homePath value="$USER_HOME$/android/adt/sdk" />
      <roots>
        <annotationsPath>
          <root type="composite">
            <root url="jar://$APPLICATION_HOME_DIR$/plugins/android/lib/androidAnnotations.jar!/" type="simple" />
          </root>
        </annotationsPath>
        <classPath>
          <root type="composite">
            <root url="jar://$USER_HOME$/android/adt/sdk/platforms/android-28/android.jar!/" type="simple" />
            <root url="file://$USER_HOME$/android/adt/sdk/platforms/android-28/data/res" type="simple" />
          </root>
        </classPath>
        <javadocPath>
          <root type="composite">
            <root url="file://$USER_HOME$/android/adt/sdk/docs/reference" type="simple" />
          </root>
        </javadocPath>
        <sourcePath>
          <root type="composite">
          <root url="file://$USER_HOME$/android/adt/sdk/sources/android-27" type="simple" />
          </root>
        </sourcePath>
      </roots>
      <additional jdk="1.8" sdk="android-28" />
    </jdk>
```

## references

- [how to attach Android SDK sources?](https://stackoverflow.com/questions/21221679/android-studio-how-to-attach-android-sdk-sources)