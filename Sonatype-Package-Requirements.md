# Meeting the Sonatype Package Requirements
Sonatype has several requirements for packages in order to ensure that users can find relevant details from the metadata.

## 1. Javadoc and Sources
We need Javadoc and Source attachments that allow users to access documentation for display and navigation in their IDE. This can be done automatically by using the Maven Source Plugin and Maven Javadoc plugin, as configured below:
```
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-source-plugin</artifactId>
      <version>2.2.1</version>
      <executions>
        <execution>
          <id>attach-sources</id>
          <goals>
            <goal>jar-no-fork</goal>
          </goals>
        </execution>
      </executions>
    </plugin>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-javadoc-plugin</artifactId>
      <version>2.9.1</version>
      <executions>
        <execution>
          <id>attach-javadocs</id>
          <goals>
            <goal>jar</goal>
          </goals>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>
```

## 2. GPG Signature
GnuPG allows you to encrypt and sign files in your library. Deployed files must contain a GPG signature. A `.asc` file containing the signature must be included for each file.

The details on creating a gpg key and signature were described in a previous guide file. We can automate the process of signing artifacts with the Maven GPG plugin:
```
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-gpg-plugin</artifactId>
      <version>1.5</version>
      <executions>
        <execution>
          <id>sign-artifacts</id>
          <phase>verify</phase>
          <goals>
            <goal>sign</goal>
          </goals>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>
```

In order for the plugin to work, the GPG command must be installed and the credentials should be available from `settings.xml`. The file should contain:
```
<settings>
  <profiles>
    <profile>
      <id>ossrh</id>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>
      <properties>
        <gpg.executable>gpg2</gpg.executable>
        <gpg.passphrase>the_pass_phrase</gpg.passphrase>
      </properties>
    </profile>
  </profiles>
</settings>
```

## 3. Sufficient Metadata in `pom.xml`
Your deployment needs to have a `pom.xml`, or Project Object Model, file. This is used by Maven to define your project. You must ensure that it contains the following information.

### Correct Coordinates
The project coordinates determine the location of your project in the Maven repository:
- `groupId`: the top namespace level of your project. An example is: `<groupId>com.github.huskydj1</groupId>`.
- `artifactId`: the unique name of your component. An example is: `<artifactId>circleArea</artifactId>`.
-  `version`: the version of your component. An example is: `<version>1.1.8</version>`.

### Project Name, Description, and URL
The project name, description, and url to your project website are also required for users to read and learn more about your project. An example is:

```
<name>Area Library for Circles</name>
<description>A java library for calculating the area of a circle.</description>
<url>http://github.com/huskydj1/circleArea</url>
```

### License information
You need to state the license(s) for your library. If you are publishing a library that is on Github, you can use  the auto-generated MIT license file. You must then include the following in your `pom.xml` file.

```
<licenses>
  <license>
    <name>MIT License</name>
    <url>http://www.opensource.org/licenses/mit-license.php</url>
  </license>
</licenses>
```

### Developer information
Other information must be added to associate the project with its developers. This should be in the form:
```
<developers>
   <developer>
     <name>Manfred Moser</name>
     <email>manfred@sonatype.com</email>
     <organization>Sonatype</organization>
     <organizationUrl>http://www.sonatype.com</organizationUrl>
   </developer>
 </developers>
```
You can link your Github profile if you do not have a website.

### SCM (Source Control Management) Information
In our case, the version control system that we are using is Git hosted on Github. Our information should be in the form:
```
<scm>
  <connection>scm:git:git://github.com/simpligility/ossrh-demo.git</connection>
  <developerConnection>scm:git:ssh://github.com:simpligility/ossrh-demo.git</developerConnection>
  <url>http://github.com/simpligility/ossrh-demo/tree/master</url>
</scm>
```
`connection` contains a read-only connection, `developerConnection` contains read/write access details, and `url` contains the front end Github page to our repository.

## 4. Distribution Management and Authentication
In order to deploy to the OSSRH Nexus Repository Manager with the Nexus Staging Maven plugin, we have to configure Maven by attaching the following:
```
<distributionManagement>
  <snapshotRepository>
    <id>ossrh</id>
    <url>https://oss.sonatype.org/content/repositories/snapshots</url>
  </snapshotRepository>
  <repository>
    <id>ossrh</id>
    <url>https://oss.sonatype.org/service/local/staging/deploy/maven2/</url>
  </repository>
</distributionManagement>
<build>
  <plugins>
    <plugin>
      <groupId>org.sonatype.plugins</groupId>
      <artifactId>nexus-staging-maven-plugin</artifactId>
      <version>1.6.7</version>
      <extensions>true</extensions>
      <configuration>
        <serverId>ossrh</serverId>
        <nexusUrl>https://oss.sonatype.org/</nexusUrl>
        <autoReleaseAfterClose>true</autoReleaseAfterClose>
      </configuration>
    </plugin>
    ...
  </plugins>
</build>
```
Note: The code doesn't need to be copied exactly. If other plugins are used, the Nexus Staging Maven Plugin should be configured alongside them.  

Those configurations take the user's account details to deploy from the `settings.xml` file. For authentication purposes, the file should contain the code:
```
<settings>
  <servers>
    <server>
      <id>ossrh</id>
      <username>your-jira-id</username>
      <password>your-jira-pwd</password>
    </server>
  </servers>
</settings>
```
The jira id and pwd should be defined.

##5. Nexus Staging

## References:
1. https://www.devdungeon.com/content/publish-java-packages-maven-central-repository
2. https://www.youtube.com/watch?v=bxP9IuJbcDQ&ab_channel=Recursive
