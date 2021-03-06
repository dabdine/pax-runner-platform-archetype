pax-runner-platform-archetype
=============================

A simple maven archetype that will generate an ops4j pax definition.xml suitable for use by pax runner's definitionURL argument.

Usage
=====
This project is hosted in the Maven Central repository.

1. Generate your archetype:

        mvn archetype:generate -DarchetypeGroupId=com.rapid7.pax \
                               -DarchetypeArtifactId=pax-runner-platform-archetype \
                               -DarchetypeVersion=1.0 \
                               -DgroupId=org.myplatform \
                               -DartifactId=pax-platform-felix-4.2.0
2. Modify the `definition.xml` file in the generated directory. Here's an example for felix 4.2.0:

        <platform>
            <name>Felix 4.2.0</name>
            <system>mvn:org.apache.felix/org.apache.felix.main/4.2.0</system>
        
            <packages>
            </packages>
        
            <profile name="default" default="true">
                <bundle>
                    <name>Apache Felix Gogo Command (0.12.0)</name>
                    <url>link:classpath:runner-links/org.apache.felix.gogo.command-0.12.0.link</url>
                </bundle>
                <bundle>
                    <name>Apache Felix Gogo Runtime (0.10.0)</name>
                    <url>link:classpath:runner-links/org.apache.felix.gogo.runtime-0.10.0.link</url>
                </bundle>
                <bundle>
                    <name>Apache Felix Gogo Shell (0.10.0)</name>
                    <url>link:classpath:runner-links/org.apache.felix.gogo.shell-0.10.0.link</url>
                </bundle>
            </profile>
        </platform>
3. `mvn clean install`
4. run pax runner with `--definitionURL=mvn:org.myplatform/pax-platform-felix-4.2.0/1.0-SNAPSHOT/xml`. Here's an example if you're using the maven-pax-plugin:

        <build>
          <plugins>
             <plugin>
                <groupId>org.ops4j</groupId>
                <artifactId>maven-pax-plugin</artifactId>
                <version>1.5</version>
                <configuration>
                   <runner>1.8.5</runner>
                   <provision>
                      <param>--definitionURL=mvn:org.myplatform/pax-platform-felix-4.2.0/1.0-SNAPSHOT/xml</param>
                   </provision>
                </configuration>
             </plugin>
             ...
          </plugins>
        </build>
        
NOTE: You can also have multiple definition files and use the maven classifier to build them. You would simply copy the definition.xml and slightly modify the maven plugin configuration in the generated pom to include all the definition files with specific classifiers. For example, lets say you want to create a felix 4.2.0 and 4.2.1 configuration. Create both definition.xml files (perhaps named definition-4.2.0.xml and definition-4.2.1.xml), then modify the configuration in your generated pom:

          <configuration>
             <artifacts>
                <artifact>
                   <file>definition-4.2.0.xml</file>
                   <classifier>felix-4.2.1</classifier>
                   <type>xml</type>
                </artifact>
                <artifact>
                   <file>definition-4.2.1.xml</file>
                   <classifier>felix-4.2.1</classifier>
                   <type>xml</type>
                </artifact>
             </artifacts>
          </configuration>

Then, you would use the same maven protocol handler in your definitionURL, this time including the classifier: `--definitionURL=mvn:org.myplatform/pax-platform-felix-4.2.0/1.0-SNAPSHOT/felix-4.2.0/xml`


License
=======

The MIT License (MIT)

Copyright (c) 2014 Derek Abdine, Rapid7 LLC.

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
