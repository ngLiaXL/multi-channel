# multi-channel


## Project/build.gradle

```
ext {
    android = [compileSdkVersion: 29,
               buildToolsVersion: "29.0.0",
               applicationId    : "com.ngliaxl.channel",
               minSdkVersion    : 21,
               targetSdkVersion : 29,
               versionCode      : 1,
               versionName      : "1.0"]

    support_library_version = '25.0.0'
}
```

## App/build.gradle
```
apply plugin: 'com.android.application'
def keystorePropertiesFile = rootProject.file("keystore.properties")
def keystoreProperties = new Properties()
keystoreProperties.load(new FileInputStream(keystorePropertiesFile))


android {
    compileSdkVersion rootProject.ext.android.compileSdkVersion
    buildToolsVersion rootProject.ext.android.buildToolsVersion
    defaultConfig {
        applicationId rootProject.ext.android.applicationId
        minSdkVersion rootProject.ext.android.minSdkVersion
        targetSdkVersion rootProject.ext.android.targetSdkVersion
        versionCode rootProject.ext.android.versionCode
        versionName rootProject.ext.android.versionName
        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    signingConfigs {

        release {
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
            storeFile file(keystoreProperties['storeFile'])
            storePassword keystoreProperties['storePassword']
        }
    }


    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.release
        }
    }

    flavorDimensions "mark"
    productFlavors {
        Google { dimension "mark" }
        Xiaomi { dimension "mark" }
    }

    productFlavors.all {
        flavor -> flavor.manifestPlaceholders = [MULTI_CHANNEL: name]
    }

    android.applicationVariants.all { variant ->
        variant.outputs.all {
            outputFileName = "Multi-Channel-${variant.name}-${new Date().format("yyyyMMdd")}.apk"
        }
    }

}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'androidx.appcompat:appcompat:1.0.2'
    implementation 'androidx.constraintlayout:constraintlayout:1.1.3'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'androidx.test:runner:1.2.0'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.2.0'
}
```
