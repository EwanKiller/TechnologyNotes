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
