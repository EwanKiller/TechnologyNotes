# Android build.gradle

## 启用混淆
```
plugins {
	id 'com.android.library'
}

android {
	compileSdk 30

	defaultConfig {
		minSdk 29
		targetSdk 30
		versionCode 1
		versionName "1.0"

		testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
		consumerProguardFiles "consumer-rules.pro"
	}

	buildTypes {
		release {
			// 启用混淆
			minifyEnabled true
			// Resource shrinker cannot be used for libraries.
			// shrinkResources true
			proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
			
			zipAlignEnabled true
		}
    }
compileOptions {
	sourceCompatibility JavaVersion.VERSION_1_8
	targetCompatibility JavaVersion.VERSION_1_8
	}
}

dependencies {
	implementation 'androidx.appcompat:appcompat:1.3.1'
	implementation 'com.google.android.material:material:1.4.0'
	implementation project(path: ':IPCCommon')
    testImplementation 'junit:junit:4.+'
	androidTestImplementation 'androidx.test.ext:junit:1.1.3'
	androidTestImplementation 'androidx.test.espresso:espresso-core:3.4.0'
}
```

## 修改VersionName和输出包名匹配时间戳
```
import java.time.LocalDateTime
import java.time.format.DateTimeFormatter

apply plugin: 'com.android.application'

android {

    compileSdkVersion 29
    ndkVersion '21.4.7075529'

    defaultConfig {
        applicationId "com.seed.seedremoteplayer"
        minSdkVersion 29
        versionCode 1

		// 修改VersionName为构建时间戳
        LocalDateTime compileTime = LocalDateTime.now()
        DateTimeFormatter formatter  = DateTimeFormatter.ofPattern("yyyyMMddHHmm")
        String formattedTime = compileTime.format(formatter)
        versionName formattedTime

        ndk {
            abiFilters 'arm64-v8a'
        }
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.release
        }
        debug {
            buildConfigField "boolean", "ENABLE_DEBUG", "true"
        }
    }

    applicationVariants.all { variant ->
        variant.outputs.all {
            LocalDateTime compileTime = LocalDateTime.now()
            DateTimeFormatter formatter  = DateTimeFormatter.ofPattern("yyyyMMddHHmm")
            String formattedTime = compileTime.format(formatter)
            // 自定义输出apk名匹配时间戳
            outputFileName = "remoteplayer_" + formattedTime +".apk"
        }
    }
}

dependencies {
    implementation files('libs/SeedXRSDK-release.aar')
}

```

## 引用同层级的其他build.gradle并解压aar中so
```
# build.gradle
apply plugin: 'com.android.application'
apply from: "$projectDir/build_sdk.gradle"

android {

    compileSdkVersion 29
    ndkVersion '21.4.7075529'

    defaultConfig {
        applicationId "com.seed.seedremoteplayer"
        minSdkVersion 29
        versionCode 1
        versionName "1.0.0"

        ndk {
            abiFilters 'arm64-v8a'
        }
    }

}
```

```
# build_sdk.gradle

def OBOE_SDK_ROOT= file("${buildDir}/oboe")
def CLOUDXR_SDK_ROOT= file("${buildDir}/CloudXR")
def QVR_SDK_ROOT = file("${buildDir}/QVR")
def SEED_XR_SDK_ROOT= file("${buildDir}/SeedXR")

android {
    defaultConfig {
        externalNativeBuild {
            cmake {
                arguments "-DOBOE_SDK_ROOT=${OBOE_SDK_ROOT}"
                arguments "-DCLOUDXR_SDK_ROOT=${CLOUDXR_SDK_ROOT}"
                arguments "-DQVR_SDK_ROOT=${QVR_SDK_ROOT}"
                arguments "-DSEED_XR_SDK_ROOT=${SEED_XR_SDK_ROOT}"
            }
        }
        ndk {
            abiFilters 'arm64-v8a'
        }
    }

    externalNativeBuild {
        cmake {
            path file('src/main/cpp/CMakeLists.txt')
            version '3.18.1'
        }
    }

    buildTypes {
        release {
            externalNativeBuild {
                cmake {
                    arguments "-DNDK_DEBUG=0"
                }
            }
        }
        debug {
            externalNativeBuild {
                cmake {
                    arguments "-DNDK_DEBUG=1"
                }
            }
        }
    }
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar', '*.aar'])
}

// Extract the Oboe SDK
task extractOboeSDK(type: Copy)  {
    def sourceFile = file("${projectDir}/libs/oboe-1.5.0.aar")
    from zipTree(sourceFile)
    into OBOE_SDK_ROOT
}

// Extract the CloudXR SDK
task extractCloudXRSDK(type: Copy)  {
    def sourceFile = file("${projectDir}/libs/CloudXR.aar")
    from zipTree(sourceFile)
    into CLOUDXR_SDK_ROOT
}

// Extract the QVR SDK
task extractQvrSDK(type: Copy) {
    def sourceFile = file("${projectDir}/libs/sxrApi-release.aar")
    from zipTree(sourceFile)
    into QVR_SDK_ROOT
}

// Extract the SEED XR SDK
task extractSeedXRSDK(type: Copy) {
    def sourceFile = file("${projectDir}/libs/SeedXRSDK-release.aar")
    from zipTree(sourceFile)
    into SEED_XR_SDK_ROOT
}

preBuild.dependsOn(extractOboeSDK)
preBuild.dependsOn(extractCloudXRSDK)
preBuild.dependsOn(extractQvrSDK)
preBuild.dependsOn(extractSeedXRSDK)
```
