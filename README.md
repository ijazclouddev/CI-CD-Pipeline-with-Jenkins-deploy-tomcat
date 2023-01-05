# CI-CD-Pipeline-with-Jenkins-deploy-tomcat

This project contains a sample example webapp that uses Maven to package a web app and deply in tomcat.

This project is laid out like this:

- `hello-world-maven/`
  - **`pom.xml`** is a Maven [POM file](https://maven.apache.org/pom.html) that defines the project.
  - `src/main/` is a directory that contains the code.
    - `java/` is a directory that contains server-side code.
      - `io.happycoding.servlets.`**`HelloWorldServlet.java`** is a Java servlet that returns some HTML content.
    - `webapp/` is a directory that contains web files.
      - **`index.html`** is an HTML file that shows static content.

You can compile this into a directory and a `.war` file by executing this command:

```
mvn package
