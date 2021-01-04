1.

![이미지 6](https://user-images.githubusercontent.com/30336831/101978514-cbfd4200-3c98-11eb-8faf-81768aa93e73.png)

2.

![이미지 4](https://user-images.githubusercontent.com/30336831/101978489-b0923700-3c98-11eb-81b1-bde30db338d5.png)

```
buildscript {
	repositories {
		google()  // Google's Maven repository	
	}
	dependencies {
		...
		classpath 'com.google.gms:google-services:4.3.4'
	}
}
allprojects {
	...
	repositories {
		google()  // Google's Maven repository
        ...
	}
}

```

![이미지 5](https://user-images.githubusercontent.com/30336831/101978476-99ebe000-3c98-11eb-8233-0f90aef96bce.png)

```
apply plugin: 'com.android.application'
apply plugin: 'com.google.gms.google-services'

dependencies {
	implementation platform('com.google.firebase:firebase-bom:26.1.1')
	implementation 'com.google.firebase:firebase-analytics'
}
```

