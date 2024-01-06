# Deploying robot code to a Raspberry Pi
Most of the infrastructure that is used to deploy code to a roboRIO can be reused to deploy code to a Raspberry Pi. The key things to do are to configure build.gradle to deploy to a Pi, modify SSH settings, and put Java on the Pi.

1. Install the JRE with `sudo apt install openjdk-17-jre`. You do not need the JDK unless you are programming on the Pi.
2. Edit `sshd_config` to allow root login and empty passwords, as well as password authentication if you haven't done so already.
3. Run `sudo passwd -d root` to remove the password off the root user. This allows Gradle to login as the root user without a password and is necessary for it to deploy all the files.
4. Download the zip file [here](https://github.com/Gold856/allwpilib/actions/workflows/gradle.yml?query=branch%3Asocketcan) and click the latest link at the top. Then download the correct artifact for your architecture (Arm32 if you have the 32-bit version installed, Arm64 if you have the 64-bit version installed, and Linux if you're not using a Pi, but a x86 Linux machine.)
5. Download the contents of this repo.
6. Extract the contents of the zip file, and move them into the deploy directory of your robot project (src/main/deploy). Also move the two .sh files into the deploy directory as well. This will copy all the required files to the Pi.
7. Replace your build.gradle file with the one from this repo. If you have a custom build.gradle, it is up to you to combine them manually.
8. Connect to the network your Pi is on.
9. Edit the address in the build.gradle file to point to the Pi. You can use mDNS if you don't want to mess with IP addresses, but be aware that the address may not resolve in time, and the deploy will fail. If it fails, try again a few more times, and if it doesn't succeed, check your setup.

# Notes about libraries
REVLib will deploy without any custom setup. For Phoenix 6, you will need to follow the [instructions](https://pro.docs.ctr-electronics.com/en/stable/docs/installation/installation.html) for installing Phoenix 6 on a Linux machine. The navX library is completely unsupported, so if you *really* want to use it, you'll have to figure it out. If you figure it out, please send a PR! Everything else not mentioned may or may not work. Let us know what vendordeps do and don't work and we'll add it to the list.