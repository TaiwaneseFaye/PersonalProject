# mvn-template
A Java/Scala mixed project template for maven build

## How to build and install  
```bash
 #!/bin/bash 
 set -e 
 MAVEN_OPTS="-XX:+TieredCompilation -XX:TieredStopAtLevel=1"
 mvn -DskipTests -T 1C  install
```

or alternatively have a look at `build-install.sh`

## How to build and execute 
```bash 
#!/bin/bash 
set -e 
MAVEN_OPTS="-XX:+TieredCompilation -XX:TieredStopAtLevel=1"
mvn -T 1C clean compile assembly:single
```
or alternatively have a look at `build-with-dependencies.sh`

### If Scala code needs to be compiled first 
The interesting bit that you have to add in your `<build>`
```
<plugin>
	<groupId>net.alchim31.maven</groupId>
	<artifactId>scala-maven-plugin</artifactId>
	<executions>
		<execution>
			<id>scala-compile-first</id>
			<phase>process-resources</phase>
			<goals>
				<goal>add-source</goal>
				<goal>compile</goal>
			</goals>
		</execution>	
	</executions>
</plugin>
```

Full example: 

```
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
<modelVersion>4.0.0</modelVersion>

<groupId>sandbox</groupId>
<artifactId>testJavaAndScala</artifactId>
<version>1.0-SNAPSHOT</version>
<name>Test for Java + Scala compilation</name>
<description>Test for Java + Scala compilation</description>

<dependencies>
	<dependency>
		<groupId>org.scala-lang</groupId>
		<artifactId>scala-library</artifactId>
		<version>2.7.2</version>
	</dependency>
</dependencies>

<build>
	<pluginManagement>
	<plugins>
		<plugin>
			<groupId>net.alchim31.maven</groupId>
			<artifactId>scala-maven-plugin</artifactId>
			<version>3.3.1</version>
		</plugin>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-compiler-plugin</artifactId>
			<version>2.0.2</version>
		</plugin>
	</plugins>
	</pluginManagement>
	
	<plugins>
		<plugin>
			<groupId>net.alchim31.maven</groupId>
			<artifactId>scala-maven-plugin</artifactId>
			<executions>
				<execution>
					<id>scala-compile-first</id>
					<phase>process-resources</phase>
					<goals>
						<goal>add-source</goal>
						<goal>compile</goal>
					</goals>
				</execution>
				<execution>
					<id>scala-test-compile</id>
					<phase>process-test-resources</phase>
					<goals>
						<goal>testCompile</goal>
					</goals>
				</execution>
			</executions>
		</plugin>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-compiler-plugin</artifactId>
			<executions>
				<execution>
					<phase>compile</phase>
					<goals>
						<goal>compile</goal>
					</goals>
				</execution>
			</executions>
		</plugin>
	</plugins>
</build>
</project>
```
http://davidb.github.io/scala-maven-plugin/example_java.html

## Example run
```bash
$java -jar ./target/empty-project-1.0-jar-with-dependencies.jar 
 Hello World!
 concat arguments =
```
```bash 
$java -jar ./target/empty-project-1.0-jar-with-dependencies.jar -h 
 Hello World!
 concat arguments = -h
 usage: Main
  -h,--help   show help.
```
