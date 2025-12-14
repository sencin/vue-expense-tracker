Step 1: Create an SDK Directory

Create a directory where the Android SDK will be installed (e.g., in your home directory): 
bash
mkdir -p ~/android/sdk
cd ~/android/sdk


Step 2: Download and Extract the Command-Line Tools

You need to download the latest command-line tools package from the official Android developer website. Since the download URL can change, the most reliable method is to visit the Android Studio download page in a browser, scroll to the "Command line tools only" section, copy the Linux download link, and use wget in your terminal. 

1.Download the package (replace the URL with the current one if needed):

bash
wget dl.google.com

2.Unzip the file:

bash
unzip commandlinetools-linux-*.zip

3.Move the contents to the required directory structure (The sdkmanager expects a specific structure: android_sdk/cmdline-tools/latest/...):
bash
mkdir -p cmdline-tools/latest
mv cmdline-tools/* cmdline-tools/latest/


Step 3: Set Environment Variables
You must set the ANDROID_HOME environment variable and update your PATH so your system and build tools can find the SDK. 

1.Open your shell configuration file (e.g., ~/.bashrc or ~/.zshrc) in a text editor (e.g., nano ~/.bashrc):
bash
nano ~/.bashrc


2.Add the following lines to the end of the file:
bash

export ANDROID_HOME=$HOME/android/sdk
export PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools


3.Save the file and exit the editor.
4.Apply the changes to your current terminal session:
bash

source ~/.bashrc

Step 4: Install Required SDK Packages
Now you can use the sdkmanager tool to list and install specific SDK components like platform tools, build tools, and a specific Android platform version. 
1.List available packages (optional, for reference):
bash

sdkmanager --list

2.Install essential packages (e.g., platform-tools, build tools, and a specific Android API level like 34):
bash
sdkmanager "platform-tools" "platforms;android-34" "build-tools;34.0.0"

3.Accept the licenses when prompted during installation by typing y. 



Step 5: Verify the Installation
You can verify that the tools are accessible and working correctly:
bash

adb devices
sdkmanager --version

Your Android SDK is now installed and configured in your Ubuntu terminal environment.









FOR BUILDING APk


This is generally the recommended approach for managing signing configurations. 
1.Create a Keystore: If you don't already have one, generate a keystore file using Android Studio (Build > Generate Signed Bundle/APK > Create new) or the keytool command-line utility.
bash

keytool -genkey -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias alias_name

fill up the questions after entirring

2.For capacitor.config.json:
json
{
  "appId": "...",
  "appName": "...",
  // ... other configurations
  "android": {
    "buildOptions": {
      "releaseType": "AAB",
      "keystorePath": "/path/to/your/my-release-key.jks",
      "keystorePassword": "your_keystore_password",
      "keystoreAlias": "alias_name",
      "keystoreAliasPassword": "your_alias_password"
    }
  }
}


npx cap build android