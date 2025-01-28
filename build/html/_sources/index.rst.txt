.. role:: raw-html-m2r(raw)
   :format: html


How to Publish Your Java Library to Maven Central
=================================================


Step 1: Create an Account on Central Sonatype
---------------------------------------------

First things first — go to `central.sonatype.com <https://central.sonatype.com/>`_
 and create an account. You can use your existing **GitHub account** to make the process easy.

When you link your GitHub, Sonatype will automatically generate a **namespace** for you based on your GitHub username. For example:

* **GitHub Username**: ``init-to``
* **Namespace**: ``io.github.init-io``  
* Bonus: It comes with a lovely green "Verified" tag. Fancy, right?

.. image:: https://raw.githubusercontent.com/Init-io/MavenCentralGuide/refs/heads/main/images/namespace.png
   :target: https://raw.githubusercontent.com/Init-io/MavenCentralGuide/refs/heads/main/images/namespace.png
   :alt: Sonatype Account Creation Example

*(Imagine a green badge here. You know you want it.)*


Step 2: Add and Configure Your ``pom.xml`` File
---------------------------------------------------

The ``pom.xml``\  file is the heart of your Maven project. To publish your library to Maven Central, it needs some extra love and care. Here's what to include in your ``pom.xml``\ :

Below is the complete pom.xml:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>

       <groupId>io.github.init-io</groupId>
       <artifactId>javase</artifactId>
       <version>1.2.2</version>

       <name>Javase</name>
       <description>A java library to easily interact with firebase realtime database, authentication and storage</description>
       <url>http://github.com/init-io/javase</url>

       <licenses>
           <license>
               <name>MIT License</name>
               <url>http://www.opensource.org/licenses/mit-license.php</url>
           </license>
       </licenses>

       <developers>
           <developer>
               <name>Siam Rayhan</name>
               <email>developers.init.io@gmail.com</email>
               <organization>Init.io</organization>
               <organizationUrl>https://github.com/init-io</organizationUrl>
           </developer>
       </developers>

       <dependencies>
           <dependency>
               <groupId>org.json</groupId>
               <artifactId>json</artifactId>
               <version>20240303</version>
           </dependency>
       </dependencies>

       <scm>
           <connection>scm:git:git://github.com/init-io/javase.git</connection>
           <developerConnection>scm:git:ssh://github.com:init-io/javase.git</developerConnection>
           <url>http://github.com/init-io/javase/tree/master</url>
       </scm>

       <properties>
           <maven.compiler.source>17</maven.compiler.source>
           <maven.compiler.target>17</maven.compiler.target>
           <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
       </properties>

       <build>
           <plugins>
               <plugin>
                   <groupId>org.sonatype.central</groupId>
                   <artifactId>central-publishing-maven-plugin</artifactId>
                   <version>0.5.0</version>
                   <extensions>true</extensions>
                   <configuration>
                       <publishingServerId>central</publishingServerId>
                   </configuration>
               </plugin>

               <!--            javadoc-->
               <plugin>
                   <groupId>org.apache.maven.plugins</groupId>
                   <artifactId>maven-javadoc-plugin</artifactId>
                   <version>3.3.0</version>
                   <executions>
                       <execution>
                           <id>attach-javadocs</id>
                           <goals>
                               <goal>jar</goal>
                           </goals>
                       </execution>
                   </executions>
               </plugin>
               <!--            source-->
               <plugin>
                   <groupId>org.apache.maven.plugins</groupId>
                   <artifactId>maven-source-plugin</artifactId>
                   <version>3.2.1</version>
                   <executions>
                       <execution>
                           <id>attach-sources</id>
                           <goals>
                               <goal>jar-no-fork</goal>
                           </goals>
                       </execution>
                   </executions>
               </plugin>
               <!--            pom, .asc-->
               <plugin>
                   <groupId>org.apache.maven.plugins</groupId>
                   <artifactId>maven-gpg-plugin</artifactId>
                   <version>1.6</version>
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

   </project>

..

   **Tip**\ : If your project doesn't have a ``pom.xml`` file yet, you can generate one using the Maven quickstart archetype:

   .. code-block:: bash

      mvn archetype:generate -DgroupId=io.github.snoop-dog -DartifactId=mylib -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false


Understanding Versioning in Maven Central
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

When you publish a library to **Maven Central**\ , it's important to follow **semantic versioning** to ensure that your library remains stable and easily usable by others. Versioning 
helps users and automated systems understand the nature of changes between releases. Let's break down how versioning works:

Semantic Versioning: MAJOR.MINOR.PATCH
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In Maven, your library's version number is typically written as ``MAJOR.MINOR.PATCH``. Here's what each part means:


#.
   **MAJOR**\ : Increment this number when you make **backward-incompatible changes**. For example, if you remove or change public APIs, or introduce breaking changes that will affect users of previous versions, increase the MAJOR version.


   * Example: ``1.0.0`` → ``2.0.0``

#.
   **MINOR**\ : Increment this number when you add **new features in a backward-compatible manner**. This means that new functionality has been added, but old functionality still works as expected.


   * Example: ``1.0.0`` → ``1.1.0``

#.
   **PATCH**\ : Increment this number when you make **backward-compatible bug fixes**. This means that the library has improved without adding new features, just fixing issues like bugs or performance problems.


   * Example: ``1.0.0`` → ``1.0.1``

Snapshot Versions
^^^^^^^^^^^^^^^^^

During development, you may release **snapshot versions**. These versions are used to denote that the code is still under development and not stable. A snapshot version ends with ``-SNAPSHOT``\ , like ``1.0.0-SNAPSHOT``. Snapshot versions are typically used for early builds or pre-releases, and they’re automatically updated by Maven.


* Example: ``1.0.0-SNAPSHOT`` → ``1.1.0-SNAPSHOT``

Once the code is stable and ready for a release, remove the ``-SNAPSHOT`` suffix and release it as a final version (e.g., ``1.0.0``\ ).

Versioning in Your ``pom.xml``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Ensure the version number in your ``pom.xml`` reflects your current release, and remember to update it each time you make a change. Here's an example snippet of what versioning looks 
like in the ``pom.xml`` file:

.. code-block:: xml

   <version>1.0.0</version>

When you release a new version of your library, update this field accordingly. If you're maintaining multiple versions, be sure to keep track of which version corresponds to which release.

Best Practices for Versioning
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


* **Never overwrite a released version**\ : Once a version is published to Maven Central, it's **immutable**. This ensures that developers relying on your library don’t unexpectedly get new features or bug fixes unless they explicitly request them.
* **Use clear version numbers**\ : Avoid using ambiguous version numbers like ``1.0.0-beta`` or ``1.0.1-dev`` in production releases, as these can cause confusion.
* **Update your version regularly**\ : Even if you're fixing bugs or making small changes, be sure to update the version to avoid conflicts and confusion with users of previous versions.

By following these versioning practices, you ensure that your library's users can easily track updates, understand changes, and manage dependencies in their own projects.

----

Step 3: Install GPG (Trust Me, It's Important)
----------------------------------------------

To sign your artifacts (like a true professional), you need **GPG** installed on your system.  
Download it for Windows from `GPG Download Page <https://www.gpg4win.org/>`_.

Once installed:
^^^^^^^^^^^^^^^


#. Open your terminal or Command Prompt.
#. Navigate to your project directory.
#. Generate a key pair with the following command:

   .. code-block:: bash

      gpg --gen-key

   Follow the on-screen instructions to set up your key. Choose RSA and an adequate key size (e.g., 2048).

#. To confirm your key was created, type:

   .. code-block:: bash

      gpg --list-keys

   You should see your key details listed.

Publish Your GPG Key:
^^^^^^^^^^^^^^^^^^^^^

To share your GPG key with the world (and Maven Central), publish it to a keyserver. Use this command:

.. code-block:: bash

   gpg --keyserver keyserver.ubuntu.com --send-keys <YOUR_KEY_ID>

Replace ``<YOUR_KEY_ID>`` with the actual ID of your key (you'll see it when you run ``gpg --list-keys``\ ).

..

   **Example Output**\ :

   .. code-block::

      pub   rsa2048 2025-01-01 [SC]
            ABCD1234EF567890
      uid   [ultimate] Your Name <your-email@example.com>


----


#.
   Backup Your Key
   Since you're already about to use the key for signing artifacts, make sure to back it up. You can export the private key (for signing future versions) like this:

   .. code-block:: bash

      gpg --export-secret-keys --armor ABCD1234EF567890 > my-private-key.asc

   This will save your private key into a file named my-private-key.asc, which you should store securely (in an encrypted backup location or secure storage).

#.
   Don't Lose Your Private Key
   Losing the private key means you can't sign or update your published artifacts. It's crucial to have a secure backup of this key.

#.
   Use the Key for Future Versions
   When you publish new versions of your library or any other artifact to Maven Central, you'll continue using this same GPG key ``(ABCD1234EF567890)``. Make sure to sign each new version with this key to maintain consistency.

Step 4: Make Your Project Maven-Ready
-------------------------------------

Before deploying, you need to make sure your project is in ship-shape.\ :raw-html-m2r:`<br>`
Run the following command in your project directory to build your library:

.. code-block:: bash

   mvn install

----

Step 5: Sign Your Files
-----------------------

Now comes the fun part. You'll need to **sign** your artifacts (JARs, POMs, etc.). Use the following commands:

.. code-block:: bash

   gpg -ab target/javase-1.2.2.jar
   gpg -ab target/javase-1.2.2-javadoc.jar
   gpg -ab target/javase-1.2.2-sources.jar
   gpg -ab target/javase-1.2.2.pom

After each command, enter ``y`` when prompted. This generates ``.asc`` signature files alongside your artifacts.

----

Step 6: Configure Your Maven ``settings.xml``
-------------------------------------------------

You'll need a ``settings.xml`` file to tell Maven how to authenticate with Sonatype. This file is usually located in your ``.m2/`` directory:

* On Windows: ``C:\Users\<YourUser>\.m2\settings.xml``
* On Mac/Linux: ``/Users/<YourUser>/.m2/settings.xml``

Here's an example template for ``settings.xml``\ :

.. code-block:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
       <servers>
           <server>
               <id>central</id>
               <username>your-user-name</username>
               <password>your-password</password>
           </server>
       </servers>
   </settings>

Make sure to replace ``your-user-name`` and ``your-password`` with your actual credentials.  
To generate a password (private key) in Sonatype, log in to your account, go to `View Account <https://central.sonatype.com/account>`_.

.. image:: https://raw.githubusercontent.com/Init-io/MavenCentralGuide/refs/heads/main/images/View%20Account.png
   :target: https://raw.githubusercontent.com/Init-io/MavenCentralGuide/refs/heads/main/images/View%20Account.png
   :alt: View Account Example

and click **Generate User Token**.

.. image:: https://raw.githubusercontent.com/Init-io/MavenCentralGuide/refs/heads/main/images/Generate%20User%20Token.png
   :target: https://raw.githubusercontent.com/Init-io/MavenCentralGuide/refs/heads/main/images/Generate%20User%20Token.png
   :alt: Generate User Token

*(Generate the key to roll.)*.


Step 7: Deploy Your Library
---------------------------

Now it's time for the final move. Run the following command to deploy your library to the **Sonatype Nexus Repository**\ :

.. code-block:: bash

   mvn deploy

----

Step 8: Validate and Publish
----------------------------


#. Log in to your `Sonatype Central <https://central.sonatype.com/>`_ account.
#. Navigate to `Publish <https://central.sonatype.com/publishing>`_.

   .. image:: https://raw.githubusercontent.com/Init-io/MavenCentralGuide/refs/heads/main/images/Publish.png
      :target: https://raw.githubusercontent.com/Init-io/MavenCentralGuide/refs/heads/main/images/Publish.png
      :alt: Publish


#. Validate your deployment:

   * It will take some time to be published.
   * After few minitues you will see it says Published.

Within a few minutes, your library will be published and available on **Maven Central**. To confirm:


* Go to the `Browse <https://central.sonatype.com/search/>`_ section in Sonatype and search for your library.

.. image:: https://raw.githubusercontent.com/Init-io/MavenCentralGuide/refs/heads/main/images/Browse.png
   :target: https://raw.githubusercontent.com/Init-io/MavenCentralGuide/refs/heads/main/images/Browse.png
   :alt: Browse

Thank You for Reading!
=======================

We appreciate you taking the time to go through this guide on publishing your Java library to Maven Central. Whether you're new to the process or looking for a refresher, we hope this guide helped you navigate the steps to success.

This guide was created by the **Init.io** team, with special contributions from **Siam Rayhan**. We are passionate about making open-source development more accessible, and we hope you found the information useful.

If you have any questions, suggestions, or feedback, feel free to reach out to us on `GitHub <https://github.com/init-io>`_ or via email at `developers.init.io@gmail.com <https://mail.google.com/mail/u/0/#inbox?compose=new>`_.

Thank you again for reading, and best of luck with your Maven Central journey!
