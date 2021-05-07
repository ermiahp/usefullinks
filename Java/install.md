How to Install JDK 14 on Ubuntu 20.04
=====================================

11 months ago

by [Younis Said](https://linuxhint.com/author/yunissaid12/)

The Java programming language is one of the most famous languages and has been used to develop many software products. Java runs on all the major operating systems, and the applications developed using Java can be found in laptops, desktops, gaming consoles, and mobile phones. There are several applications of Java, but the two most commonly used applications are OpenJDK (Java Development Kit) and Oracle Java. The only real difference between these is that Oracle Java is used for non-commercial use, i.e., development use.

[![](https://linuxhint.com/wp-content/uploads/2020/06/1-14.jpg)](https://linuxhint.com/wp-content/uploads/2020/06/1-14.jpg)

### Installing OpenJDK 14\
Ubuntu 20.04 includes the latest version of JDK latest (ver. 14 at the time this article was written) in the official Ubuntu repository.

Step 1: Update APT

As always, first, update and upgrade your APT through the following command.

$ sudo apt update

[![](https://linuxhint.com/wp-content/uploads/2020/06/2-30-1024x305.png)](https://linuxhint.com/wp-content/uploads/2020/06/2-30.png)

$ sudo apt upgrade

[![](https://linuxhint.com/wp-content/uploads/2020/06/3-29-1024x290.png)](https://linuxhint.com/wp-content/uploads/2020/06/3-29.png)

Step 2: Download and Install JDK Kit

Next, download and install the latest JDK kit using the given terminal command.

$ sudo apt install openjdk-14-jdk

[![](https://linuxhint.com/wp-content/uploads/2020/06/4-27-1024x489.png)](https://linuxhint.com/wp-content/uploads/2020/06/4-27.png)

Step 3: Check Installed JDK Framework

You can check the installed JDK framework on your system using the following terminal command.

$ java -version

Now, you have successfully installed the JDK package. This package includes JDK, JRE (Java runtime environment), and the 64-bit Server VM.

[![](https://linuxhint.com/wp-content/uploads/2020/06/5-29-1024x226.png)](https://linuxhint.com/wp-content/uploads/2020/06/5-29.png)

Step 4: Update Path to JDK (Optional)

Now update the path to the latest JDK package using the following terminal command. This step should be performed when there are multiple JDK versions installed on the Ubuntu machine. Since the current version (at the time this article was written) is JDK 14, select JDK 14 from this list.

$ sudo update-alternatives --config java

Select the JDK14 version in the list and continue to the next step.

[![](https://linuxhint.com/wp-content/uploads/2020/06/6-25-1024x429.png)](https://linuxhint.com/wp-content/uploads/2020/06/6-25.png)

Step 6: Set Up Environment Variable

The JAVA_HOME environment variable must be set up before using dependent applications. Use the following terminal command to find the java installation path.

$ sudo update-alternatives --config java

Find the path to JDK 14 and copy this path.

[![](https://linuxhint.com/wp-content/uploads/2020/06/7-26-1024x422.png)](https://linuxhint.com/wp-content/uploads/2020/06/7-26.png)

Step 7: Open Environment File

Now, open the environment file using your preferred text editor via the following terminal command.

$ sudo gedit /etc/environment

Add the copied path to this file at the end, as follows.

JAVA_HOME="/usr/lib/jvm/java-14-openjdk-amd64"

[![](https://linuxhint.com/wp-content/uploads/2020/06/8-25-1024x540.png)](https://linuxhint.com/wp-content/uploads/2020/06/8-25.png)

Step 8: Save Your Changes

To save the changes made to the Java Home variable, close the current terminal window. Open the terminal again and type the following command.

$ source /etc/environment\
$ echo $JAVA_HOME

[![](https://linuxhint.com/wp-content/uploads/2020/06/9-20-1024x190.png)](https://linuxhint.com/wp-content/uploads/2020/06/9-20.png)

### Uninstalling JDK

You can remove the JDK package using the following terminal command.

$ sudo apt remove openjdk-14-jdk

[![](https://linuxhint.com/wp-content/uploads/2020/06/10-22-1024x443.png)](https://linuxhint.com/wp-content/uploads/2020/06/10-22.png)

### Conclusion

This article covered how to install JDK14 on Ubuntu 20.04, how to set up the JAVA HOME environment variable, and how to uninstall JDK 14 package. After reading this article, you should know how to install and set up the current JDK package.
