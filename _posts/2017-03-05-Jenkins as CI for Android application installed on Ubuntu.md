---
title: Jenkins as CI for Android application installed on Ubuntu
date: 2017-03-05 09:12:54 +0100
categories: [android]
tags: [android,mobile,ubuntu,jenkins,ci]
image:
  path: /assets/img/jenkins-ci-i-android.png
  alt: jenkins ci
author: michal_cwiklinski
toc: false
---

# Jenkins as CI for Android application installed on Ubuntu

I won't be going deep with some functions, but try to focus on simple steps allowing to run builds of android application on Jenkins server. What I'll be needing for this is just ISO file for netboot Ubuntu image, some computer (in my case it will be some nettop fanless computer with Intel Atom CPU and 1Gb of RAM), and of course some code repository (in my case - project on BitBucket). So - let's start...

I won't be going through all install procedure on Ubuntu, but for best performance of my small computer, the best solution was to install just command-based version of Ubuntu (netboot), without any graphic interface. The ISO images for latest versions of Ubuntu are [here](http://cdimage.ubuntu.com/netboot/). I choose newest one. Next step after configuring and installing Ubuntu is to install CI server - in this case - Jenkins. In order to do it, I had to add proper record to Ubuntu sources: 

```bash
wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
```

Next thing is to install proper package after refreshing repositories, with additional jdk and git support: 
```bash
sudo apt-get update
sudo apt-get install jenkins default-jdk git-core
```

By default, Jenkins server is working on 8080 port, so in order to see the server, I had to open address: `http://your-server-ip:8080`. If you want to use different port number, open `/etc/init.d/jenkins` in any editor, and change value for HTTP_PORT key. After previous step, in order to make Jenkins build my application, I had to install Android SDK on my machine. First thing to do in this case, is to download package with Android SKD core. I opened page [developer.android.com](https://developer.android.com/studio/index.html) and in section Get just the command line tools I found a link to package `tools_r25.2.3-linux.zip` containg latest Android SDK. Next step was to download it, unpack, and make it accessible:

```bash
cd /opt
wget https://dl.google.com/android/repository/tools_r25.2.3-linux.zip
unzip tools_r25.2.3-linux.zip
rm tools_r25.2.3-linux.zip
```

Next, I prepared the environment to be able to access all tools from Android SDK by setting few global variables:
```bash
/etc/profile.d/android.sh
export ANDROID_HOME="/opt/android-sdk-linux"
export PATH="$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/platform-tools:$PATH"
source /etc/profile
```

What's left was to install proper packages inside Android SDK. There are two approaches to do this: one is to use android binary with --no-ui switch, and the second is to use new tool: `sdkmanager`,  provided lately by Android SDK package. Both ways allowed me to achieve my goal:
```bash
// First of all in order to NOT accept all licenses during packages install I did this
mkdir $ANDROID_HOME/licenses
echo 8933bad161af4178b1185d1a37fbf41ea5269c55 > $ANDROID_HOME/licenses/android-sdk-license
echo 84831b9409646a918e30573bab4c9c91346d8abd > $ANDROID_HOME/licenses/android-sdk-preview-license

/**
 * android binary approach
 */
// update the SDK itself and install all packages
android update sdk --no-ui

// In order to install just chosen packages this command allows to list all packages
android list sdk --all

// Then I choose just needed and put it into filter switch separated by comma
android update sdk -u --filter 3,4,5,33,34,35,36,37,162,168,169

/**
 * sdkmanager approach
 */

// update the SDK itself and install all packages
sdkmanager --update

// In order to install just chosen packages this command allows to list all packages
sdkmanager --list

// Then I choose just needed and put it into filter switch separated by comma
sdkmanager "tools" "platform-tools"
sdkmanager "build-tools;25.0.2" "build-tools;25.0.1" "build-tools;25.0.0"
sdkmanager "platforms;android-25" "platforms;android-24" "platforms;android-23" "platforms;android-22" "platforms;android-21"
sdkmanager "extras;android;m2repository" "extras;google;m2repository"
sdkmanager "extras;m2repository;com;android;support;constraint;constraint-layout;1.0.1"
sdkmanager "extras;m2repository;com;android;support;constraint;constraint-layout-solver;1.0.1"
```

Then I was ready to setup our first project on Jenkins. In order to setup fully packed Android project into Jenkins, I had to install some plugins. My obvious choices were: [BitBucket plugin](https://wiki.jenkins-ci.org/display/JENKINS/BitBucket+Plugin), [Slack plugin](https://wiki.jenkins-ci.org/display/JENKINS/Slack+Plugin), [Slack File Uploader Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Slack+File+Uploader+Plugin), [Gradle Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Gradle+Plugin), [JaCoCo Plugin](https://wiki.jenkins-ci.org/display/JENKINS/JaCoCo+Plugin), [JUnit plugin](https://wiki.jenkins-ci.org/display/JENKINS/JUnit+Plugin). The rest is your choice. What I needed additionally, was to add my Jenkins CI server to trusted computers on my BitBucket account by putting there my SSH key. I won't be putting details, just a [link](https://confluence.atlassian.com/bitbucket/add-an-ssh-key-to-an-account-302811853.html) with directions. After connecting my CI server with BitBucket account, I was ready to start. I created a new project after configuring an account on Jenkins server. A small tip - try to put a names in "directory friendly" format - the best approach is to use camel case one-word name, like MyNewestAndroidProject - this is caused by specifics of Jenkins workspaces, which are just simple directories, so putting there space will just make problems, especially in Linux environments.

Then I added my code repository as source After I had to tell Jenkins to run my project as Gradle one - in order to make it build I added build step as `Invoke Gradle script`. Next step was to choose proper gradle command, with configuring this project to report build to Slack, and then after if succeed post apk files in proper Slack channel.

So - that's it. I was able to run project manually, or configure it in a way, that it's being run automatically after any change committed to code repository. There are plenty of additional plugins, which everybody can install and configure in order to fulfill their needs.