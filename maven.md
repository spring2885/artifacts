# Installing Maven on OS/X

1. Set JAVA_HOME using /usr/libexec/java_home. 
   See this [Stack Overflow Link](http://stackoverflow.com/questions/6588390/where-is-java-home-on-osx-yosemite-10-10-mavericks-10-9-mountain-lion-10)
   Example: 
````
  vi ~/.bash_profile
````
Add this line:
````
export JAVA_HOME="$(/usr/libexec/java_home -v 1.8)"
````

11. Exit VI (ESCAPE + :wq)
11. Exit terminal (completely)
11. Launch a new terminal and verify your JAVA_HOME.
````
	$ echo $JAVA_HOME
	/Library/Java/JavaVirtualMachines/jdk1.8.0.jdk/Contents/Home
````
1.  Let's install Maven according to the [Maven Install Instructions](https://maven.apache.org/install.html)
11. Download ```apache-maven-3.3.9-bin.tar.gz```
   I'm assuming it'll go into the ~/Downloads directory as typical.
   
11. Create a directory for Maven and untar it there.
````
	mkdir -p ~/bin/
	cd ~/bin/
	tar zxvf ~/Downloads/apache-maven-3.3.9-bin.tar.gz
````

11. Now test it by checking the version.
````
	./apache-maven-3.3.9/bin/mvn -v
````

You should see output like this:
````
Apache Maven 3.3.9 (bb52d8502b132ec0a5a3f4c09453c07478323dc5; 2015-11-10T08:41:47-08:00)
Maven home: /Users/rcleveng/bin/apache-maven-3.3.9
Java version: 1.8.0, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "mac os x", version: "10.11.2", arch: "x86_64", family: "mac"
````

3. Add maven to your PATH by editing ```$HOME/.bash_profile``` again:

````
  vi ~/.bash_profile
````

Add this line:
````
export PATH=$PATH:$HOME/bin/apache-maven-3.3.9/bin
````
Restart your terminal

