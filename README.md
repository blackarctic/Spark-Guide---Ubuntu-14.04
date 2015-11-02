#Guide to Installing & Running Apache Spark in Standalone Mode on Ubuntu 14.04

In order to install Apache Spark, we must first install its dependencies: Java 7+, Scala, and Git. Then we can proceed to build Spark using the Scala build tool (sbt). Finally we will run a small Spark app to test it.

It's super easy, let's get started.

<br>
####Important Notes:

* Your machine must have at least 4GB of RAM in order to build and run Spark.

<br>
##Installing Java 7

1) Add the Java repo

```sh
sudo apt-add-repository ppa:webupd8team/java
```

2) Update and upgrade through apt-get

```sh
sudo apt-get update
sudo apt-get upgrade
```

3)  Install Java 7

```sh
sudo apt-get install oracle-java7-installer
```

<br>
##Installing Scala

1) [Download Scala](http://www.scala-lang.org/download/)

2) Create the directory in which to install Scala

```sh
sudo mkdir /usr/local/src/scala
```

3) Untar and install Scala

```sh
sudo tar -xvf scala-2.11.7.tgz -C /usr/local/src/scala/
```

4) Add these environment variables to .bashrc in your home directory

```sh
cd ~
echo "" >> .bashrc
echo "export SCALA_HOME=/usr/local/src/scala/scala-2.­11.7" >> .bashrc
echo "export PATH=$SCALA_HOME/bin:$PATH" >> .bashrc
echo "" >> .bashrc
```

5) Reload .bashrc for changes to take effect

```sh
. .bashrc
```

<br>
##Installing Git

1) Install Git through apt-get

```sh 
sudo apt-get install git 
```

<br>
##Building Spark

1) [Download Apache Spark](http://spark.apache.org/downloads.html) (Choose source code)

2) Untar Spark

```sh
tar -xvf spark-1.5.1.tgz
```

3) Build Spark

```sh
cd spark-1.5.1
build/sbt assembly
# This will take ~15-25 min to complete
```

4) Finally you should see a success message

```sh
[success] Total time: 1010 s, completed Oct 30, 2015 5:30:16 PM
```

<br>
##Running Spark

1) Start up spark shell

```sh
bin/./spark-shell
```

(Type ```exit``` to leave the spark shell at any time)

<br>
##Playing with Spark

2) Once spark is booted up, let's create an [RDD](https://spark.apache.org/docs/1.3.1/api/scala/index.html#org.apache.spark.rdd.RDD) from the README

```sh
val textFile = sc.textFile("README.md")
```
This should produce an RDD similar to the following:

```sh
textFile: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[1] at textFile at <console>:21
```

3) Let's count the number of lines in the README file

```sh
textFile.count()
```

4) Let's also check out the first line in the README file

```sh
textFile.first()
```

5) Next, let's get a bit more advanced with a **filter** function to count all the lines with "spark"

```sh
textFile.filter(line => line.contains("Spark")).count()
```

6) Let's go a step further and perform a **map reduce** to find the number of words in the line with the most words. First we will **map** each line to a value and then we will **reduce** all the values down to a single value representing the most words in a line.

```sh
textFile.map(line => line.split(" ").size).reduce((a, b) => if (a > b) a else b)
```

A slightly more readable version uses the Math library:

```sh
import java.lang.Math
textFile.map(line => line.split(" ").size).reduce((a, b) => Math.max(a, b))
```

<br>
For more examples, check out <http://spark.apache.org/examples.html>

<br>
####Very Useful Sources:

<http://spark.apache.org/docs/latest/quick-start.html>

<http://spark.apache.org/docs/latest/spark-standalone.html>

<http://spark.apache.org/examples.html>

<https://www.youtube.com/watch?v=eQ0nPdfVfc0>

<https://www.youtube.com/watch?v=bWorBGOFBWY&list=PL-x35fyliRwhKT-NpTKprPW1bkbdDcTTW>
