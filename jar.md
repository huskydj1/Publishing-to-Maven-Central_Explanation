# Step One:
Objective: create a maven package in the local repository. 

Software: Eclipse 

### 1. start the project and put in basic info 
> Eclipse File – new – project 

![image](https://github.com/yxie21/manual/blob/master/1.jpg?raw=true)

> Choose Maven Project (left)  ---- Use default workspace location (right)

![image](https://github.com/yxie21/manual/blob/master/2.png?raw=true)
![image](https://github.com/yxie21/manual/blob/master/3.png?raw=true)

> Choose maven-archetype-quickstart

![image](https://github.com/yxie21/manual/blob/master/4.png?raw=true)

> Put in some basic information, group id and artifact id. 

![image](https://github.com/yxie21/manual/blob/master/5.png?raw=true)
 
### 2. put in the code you want to execute 

Under project in the left control panel, find App.java under src/main/java and then under Java2. Project. App.java is where you can put in the code you want (here shown is a simple default “helloworld” code. 
 
![image](https://github.com/yxie21/manual/blob/master/6.png?raw=true)

### 3. run the project (attaining the package)

> Click on the project --- Run as --- 4 Maven Build
 
![image](https://github.com/yxie21/manual/blob/master/7.png?raw=true)

> Goals: package

![image](https://github.com/yxie21/manual/blob/master/8.png?raw=true)
 
Then, if this is successfully run, you should see the following message: 

![image](https://github.com/yxie21/manual/blob/master/9.png?raw=true)
 

### 4. a change in pom.xml in case the following problem is encountered 

*(this step is not necessary for all eclipse users)*

##### a)	Problem: 

Failed to run successfully initially, and had the following message:

```

[ERROR] Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:3.1:compile (default-compile) on project groupproject: Compilation failure: Compilation failure: 
[ERROR] Source option 5 is no longer supported. Use 7 or later.
[ERROR] Target option 5 is no longer supported. Use 7 or later.
[ERROR] -> [Help 1]
[ERROR] 
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR] 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoFailureException

```

##### b)	Solution: 

In pom.xml, replace the following lines 

```

 <properties>
    <project.build.sourceEncoding>UTF-8 </project.build.sourceEncoding>
  </properties>

  ```

With 

```
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
       <java.version>1.8</java.version>
       <maven.compiler.source>1.8</maven.compiler.source>
       <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```

Then, you should see the build success message in step 3!

### 5. save the package to a local repository 

The last step is to turn it into a local repository. 

> Click on the project—run as (again) – 4 Maven build (same as before)
 
![image](https://github.com/yxie21/manual/blob/master/10.png?raw=true)


> *(different)* Goals: install 


![image](https://github.com/yxie21/manual/blob/master/11.png?raw=true)
 

Then, you should see the build success message again!

### 6. check if everything is correct and get the jar file

Lastly, run the following commands in the terminal

![image](https://github.com/yxie21/manual/blob/master/12.png?raw=true)
 

**The “Project-0.0.1-SNAPSHOT.jar” file is the file you want to share with others!**

