# Welcome to the SpringBoot for Java!

This tutorial will help teach you how to build Java web applications using
SpringBoot 3.0.

Before you get started, you will need to install [java](https://www.oracle.com/java/technologies/downloads/) and [git](https://git-scm.com/downloads).

I recommend installing Java 17, as it's the latest LTS (Long Term Support)
release version of Java as of the time this file was created, which should
be valid throughout the entire semester.

These installs may require you to update your PATH, which is done differently
in Windows or the Mac. Those details are not provided in this tutorial.

Test that the `java` command works by running:

    java -version

You should see text displaying the version of java installed. If you do not,
ensure you have java installed and referenced in your PATH.

Optionally, you can also install/use an IDE such as [Eclipse](https://eclipseide.org/)
or [IntelliJ](https://www.jetbrains.com/idea/download/) to provide an more
robust programming environment. Note, I recommend the Ultimate version of the
latter, which is use by many commercial developers, but is free for
students/teachers, see [here](https://www.jetbrains.com/community/education/#students)
for details on obtaining an educational license.

Alright! You are ready to go!


Create a new project
--------------------

The initial documentation is heavily influenced by the following sites:

* [quickstart](https://spring.io/quickstart)
* [spring-boot](https://spring.io/guides/gs/spring-boot/)
* [IntelliJ](https://spring.io/guides/gs/intellij-idea/)

Or, you can choose to follow this guide, which is MUCH more comprehensive.

---

Create a new Spring Boot Project by going to <https://start.spring.io/>

* Project - Gradle - Groovy
* Language - Java
* Spring Boot - 3.1.3
* Project Metadata
  - Group: edu.carroll
  - Artifact: cs389-demo
  - Name: cs389
  - Description: CS389 project for Spring Boot
  - Package name: edu.carroll.cs389
  - Packaging: Jar
  - Java: 17

Next, in the upper-left corner of the screen, Click on the 'Add dependencies'
button, and scroll down until you see 'Spring Web', and then click to add it.

Finally, click the 'GENERATE' button at the bottom of the page, which should
create and then download the file cs389-demo.zip onto your laptop.

Bring up a command line window and browse to the directory for projects
in this class) (either create a new directory or use an existing directory).

Once in that directory, unpack the file in the top-level directory, which
should create a new folder named cs389-demo containing the basic configuration
for your demo project.

In the newly created `cs389-demo` directory create a new git repository by running:

    cd cs389-demo
    git init

```sh
git add .
git commit -m "Initial commit"
```

At this point, I would *HIGHLY* recommend creating a new Github repository
associated with your personal Github account, and then upload this project
to github before proceeding. Instructions on how to accomplish this are
not included in this tutorial.

Throughout this tutorial you will be able to check your progress against the
official tutorial.  To do this add the official tutorial as a new git remote
named `upstream` by running:

    git remote add upstream https://github.com/YogoGit/SpringBootTutorial.git


Fetch the remote repository:

    git fetch upstream --tags


Verify your repo is up-to-date with the remote repository code:

```sh
git diff spring_start
```

Set up an IDE
-------------

Before we do anything else, let's bring the project into our IDE.

Since the preferred IDE for this class is IntelliJ due to it being the more
likely IDE used by professional Java developes, I'll only provide the
instructions to use it.

In IntelliJ, select 'File -> New -> Projects from Existing Sources', and then
browse to the folder where the **cs389-demo** folder repository is located.
Click on the **cs389-folder** name (do NOT double-click it), then select
'Open', which should allow IntelliJ to setup and configure the project.

For Eclipse, there are more instructions and plugins needed. The following
article should get you started <https://www.eclipse.org/community/eclipse_newsletter/2018/february/springboot.php>.

Create a Simple Web Application
-------------------------------

Now you can create a web controller for this simple web application, as the
following (src/main/java/edu/carroll/cs389/HelloController.java):

    package edu.carroll.cs389;

    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class HelloController {
        @GetMapping("/")
        public String index() {
            return "Greetings from Spring Boot!";
        }
    }

Add the class to git with a proper commit message

```sh
git add src/main/java/edu/carroll/cs389/HelloController.java
git commit -m "Controller for responding to root context requests"
```

Verify your repository is up-to-date with no uncommitted files by running
a `git status` command

    On branch main
    nothing to commit, working tree clean

Finally, verify your project is up-to-date by comparing it against the
source repo

```sh
git diff hello_controller
```


Running Application
-------------------

To run the application from the command line, it's a simple matter of running
gradle in the project directory

    gradlew bootRun

The command should be the same whether you are using Windows or MacOS.  (Note,
on MacOS, you *may* need to provide the path as './gradlew' to start it up.

The FIRST TIME you do this, it will download all of the dependencies for
the project, which may take a bit. However, startup time for subsequent runs
will be much faster as the downloads are all cached locally.

After starting, you should eventually see a line that looks similar to the
following.

    2022-12-28T13:26:17.102-07:00  INFO 9483 --- [           main] edu.carroll.cs389.Cs389Application       : Started Cs389Application in 0.998 seconds (process running for 1.219)

Open the following URL in your browser to verify the app is working:
<http://localhost:8080>

This should now return the text from the HelloController we coded above, as
well as verify that your environment and dependencies are all setup properly.

Routes
------

Spring routes HTTP requests to a controller using the annotation defined in
the Controller class.  In the case of the HelloController, we use the following
annotation:

    @GetMapping("/")

This indicates that the type of operation (GET) and the context ("/") will be
mapped to the method just below the annotation (the index() method).

There are other types of operations (PUT, POST, DEL, etc...), and each request
type has a unique annotation associated with the HTTP type.  You can also
specify the routing using a different annotation, similar to the following

    @RequestMapping(value = "/", method = RequestMethod.GET)

This alternate syntax allows for mapping multiple requesst types to the same
method.

    @RequestMapping(value = "/", method = { RequestMethod.GET, RequestMethod.POST })

Note, if you don't provide any method to the RequestMapping annotation, it
responds to all HTTP operations. The syntax can get as complicated or as simple
as you'd like, so see the documentation for more examples.

---

So, let's quickly change the mapping from the root context to a different
context by modifying the annotation.

In the HelloController class, modify the annotation on the index method to
look like the following:

    @GetMapping("/hello")
    public String index() {
        return "Hello from Spring Boot!";
    }

Restart the application, and open the following URL in your browser to verify
the app is working:
<http://localhost:8080/hello>

With the updated context, you should get a slightly different message.

Commit the updated controller to git, and compare against the reference
repository.

```sh
git add src/main/java/edu/carroll/cs389/HelloController.java
git commit -m "Update route and changed output"
git diff hello_route
```

Note, if you go back to the root context (/), you will receive an error as
there is no longer any controller associated with that route any longer.

<http://localhost:8080/>

This should return a __Whitelabel Error Page__.

Unit Test
---------

Now that we have a controller that listens for a route, let's write a
test to verify it works properly to ensure that future modifications don't
break the application.


First, the project should already contain a file to ensure the Spring
application context is created properly for unit tests (src/test/java/edu/carroll/cs389/Cs389ApplicationTests.java).

Note, by convention, the name of the test class is in the exact same package
as the class that's being tested, and the class name is similar but simply
appends __Test__ to the end of the class name. In this case, __Tests__ (plural)
was appened to the end, for historical reasons.

Add a new controller test class to your project (src/test/java/edu/carroll/cs389/HelloControllerTest.java).

    package edu.carroll.cs389;

    import static org.hamcrest.Matchers.equalTo;
    import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
    import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
    import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
    import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

    import org.junit.jupiter.api.Test;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
    import org.springframework.test.web.servlet.MockMvc;

    @WebMvcTest(HelloController.class)
    public class HelloControllerTest {
        @Autowired
        private MockMvc mockMvc;

        @Test
        public void indexTest() throws Exception {
            mockMvc.perform(get("/hello")).andDo(print())
                    .andExpect(status().isOk())
                    .andExpect(content().string(equalTo("Hello from Spring Boot!")));
        }
    }

After adding to the project, run your test using gradle:

```sh
gradlew test
```

    BUILD SUCCESSFUL in 7s
    4 actionable tasks: 2 executed, 2 up-to-date

Success.  Add the test file to your repo, commit it, and once completed, verify
your code against the reference repository.

```sh
git diff unit_test
```

Once you've verified you have a valid unit test, either modify the controller
class or the unit test class to change the string to no longer match, and re-run
the test again. You should get a result similar to the following:

    HelloControllerTest > indexTest() FAILED
        java.lang.AssertionError at HelloControllerTest.java:23


HTML Response
-------------

Till now, we've generated simple text as responses to our requests, however
normal websites return entire HTML pages. There are MANY ways to do this,
including creating the browser's HTML interface with a javascript front-end
that interacts with the backend using REST calls (or similar), such as
[Angular](https://angular.io/) or [React](https://reactjs.org/). Or, you can
render the pages on the back-end and simply serve them to browser directly.
For simplicity sakes, we're going to do the latter, but feel free to
investigate full-fledged front-end environments that you can use to create
the UI the browser interacts with.

Now that it's been decided to create all the HTML content on the backend,
we've still got a few frameworks to decide on. There are quite a few, but
we have to choose one, and for this tutorial we're going to be using
[Thymeleaf](https://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html).

First, we need to add the dependency to our project in the build.gradle
file.  In the dependencies sections, add the following line:

    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'

Note, we could have also simple selected this additional dependency when we
were first creating the project on <https://start.spring.io/>.

Thymeleaf stores it's resources file in the resources folder, which is located
in src/main/resources/templates.

Create a template file as the following following (src/main/resources/templates/index.html):

    <!DOCTYPE HTML>
    <html xmlns:th="http://www.thymeleaf.org">
      <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
        <title>Serving HTML Content</title>
      </head>
      <body>
        <p>Hello World!<p>
      </body>
    </html>

Next, modify the code for HelloController to change the class from a
@RestController to a normal @Controller, and instead of returning the
content as the String, return the name of the template.  The file should
look like the following:

    package edu.carroll.cs389;

    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.GetMapping;

    @Controller
    public class HelloController {
        @GetMapping("/hello")
        public String index() {
            return "index";
        }
    }

Restart the application, and open the following URL in your browser to verify
the app is returning the correct content:
<http://localhost:8080/hello>

Commit your updated code to your repo, and verify your repo against the
remote repository code:

```sh
git diff thymeleaf_template
```

Dynamic HTML rendering
----------------------

While this is nice, wouldn't it be nice if we could render the HTML
using content generated by the Java code.  Let's do that next!

First, let's change our index method a bit to take an optional
parameter.

Let's make two changes.  First, change the context on the page from
'/hello' to '/' (as index pages are usually on the root context).

    @GetMapping("/")

Next, change the signature of the method to take an optional parameter,
and add the ability to send data to the template file.

    public String index(@RequestParam(value="name", required=false, defaultValue="Student")String name, Model model) {

Note, @RequestParam has 3 attributes, **value**, **required**,
and **defaultValue**. By default, all parameters are required unless
required is set to false.  Or, you can just set the defaultValue to a
value, which implies that required=false.  Also, we just added a new
**Model** parameter.

Let's pass the parameter to our template using the model in the index() method.

    model.addAttribute("name", name);

Once done, your method should look like the following:

    @GetMapping("/")
    public String index(@RequestParam(value="name", required=false, defaultValue="Student")String name, Model model) {
        model.addAttribute("name", name);
        return "index";
    }

Note, you'll need to make sure all of your imports are done properly for
this to work properly (I won't mention imports any more, with the
assumption the user's IDE will aid in getting the imports done
properly.)

At this point, we've setup our Java code to send in the data to the
template, so let's make a change to the HTML template code to expect the
name parameter.

Update the index template file to change the paragraph to look like the
following:

    <p th:text="'Hello, ' + ${name} + '!'"/>

Note, that we're using a special **th:text** attribute in the **p** tag
to expand the content of the name parameter.

After updating the template, save your changes to both the Java and
template code, and test your application make sure it works fine.

Here are a couple of URLs to test things are working:

* <http://localhost:8080/>
* <http://localhost:8080/?name=You>

Once you get everything working properly, commit the changes to your
repo, and verify your repo against the remote repository code:

```sh
git diff request_param
```

Updated Unit test
-----------------

Unfortunately, we now have a problem in that the Unit test is now
failing. Try modifying your test to see if you can fix the unit test to
be both flexible yet still able to test the actual index method.

Ensure your tests pass, and then commit your code.  Once done, you can
compare your implementation with the author's updated test.

```sh
git diff template_tests
```

Congratulations, you've now create a basic Java backend application, and
with a bit of work, you can create both front and back end pages
dynamically based on the user input.

Spring Boot Devtools
--------------------

Once useful ability when developing web applications is to avoid the
update/restart/refresh cycle whenever making changes. This process can
eat up a lot of time.  To speed up this cycle, you can add the
[spring-boot-devools](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using.devtools)
module to your application.

This enables a number of tools which speeds up the development
process. I recommend adding this to your project by adding the following
line to the build.gradle file in the dependencies section:

    developmentOnly 'org.springframework.boot:spring-boot-devtools'

Update the file, verify if works by restarting your application, commit
your code, and compare your file with the reference repository.

```sh
git diff devtools
```

Form Validation
---------------

Now that we have a working web application, let's do something a bit
more complex. Specifically, let's traverse between a couple of web
pages, and ask the user to supply some data to us and have the
application provide some sort of validation that the data is correct.

First thing, let's remove the HelloController and HelloControllerTest
from our project (git rm et. al), and create a more aptly named
IndexController and IndexControllerTest in their place, but this time,
let's create some sub-packages to break out the application into
different functional units. Note, by convention, the package names are
not plural, and neither are the class names.

src/main/java/edu/carroll/cs389/web/controller/IndexController.java:

    package edu.carroll.cs389.web.controller;

    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.GetMapping;

    @Controller
    public class IndexController {
        @GetMapping("/")
        public String index() {
            return "index";
        }
    }

src/test/java/edu/carroll/cs389/web/controller/IndexControllerTest.java:

    package edu.carroll.cs389.web.controller;

    import static org.hamcrest.Matchers.containsString;

    import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
    import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;
    import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
    import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

    import org.junit.jupiter.api.Test;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
    import org.springframework.test.web.servlet.MockMvc;

    @WebMvcTest(IndexController.class)
    public class IndexControllerTest {
        @Autowired
        private MockMvc mockMvc;

        @Test
        public void indexTest() throws Exception {
            mockMvc.perform(get("/")).andDo(print())
                    .andExpect(status().isOk())
                    .andExpect(content().string(containsString("Welcome to the application!")))
                    .andExpect(content().string(containsString("login")));
        }
    }

Finally, modify the index template file at src/main/resources/templates/index.html:

    <!DOCTYPE HTML>
    <html xmlns:th="http://www.thymeleaf.org">
      <head>
        <title>Welcome page</title>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
      </head>
      <body>
        <p>Welcome to the application!</p>
        <br/>
        <p><a href="#">login</a> to continue</p>
      </body>
    </html>

Ensure your applications runs (login doesn't do anything yet) and your
unit tests passed, and commit the code.

```sh
git diff simple_init
```

We first need to determine what data is in the login form, and create a
Java bean associated with the data.  We'll call that bean a LoginForm,
and implementation is as follows:
src/main/java/edu/carroll/cs389/web/form/LoginForm.java:

    package edu.carroll.cs389.web.form;

    public class LoginForm {
        private String username;
        private String password;

        public String getUsername() {
            return username;
        }

        public void setUsername(String username) {
            this.username = username;
        }

        public String getPassword() {
            return password;
        }

        public void setPassword(String password) {
            this.password = password;
        }
    }

This bean will allow us to store two text fields that the user provides
as a single submission.

Next, let's create the HTML templates:

First, src/main/resources/templates/login.html:

    <!DOCTYPE HTML>
    <html xmlns:th="http://www.thymeleaf.org">
      <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
        <title>Login page</title>
      </head>
      <body>
        <center><h1>Login</h1></center>
        <form action="#" th:action="@{/login}" th:object="${loginForm}" method="post">
          <input type="text" th:field="*{username}" placeholder="Username"/><br/>
          <input type="password" th:field="*{password}" placeholder="Password"/><br/>
          <button type="submit">Login</button>
        </form>
      </body>
    </html>

Pay special attention to the **form** and **input** tags and their
associated Thymeleaf tag attributes. We're using the LoginForm object
and properties to both allow for setting the information from the
back-end as well as store user-supplied information to it.

Note that the above page is not styled at all, as we're just focusing on
functionality, not looks for now, and I'm using a deprecated tag
(**center**).

Next, the success/failure template pages, just to allow us to see what's
going on.

src/main/resources/templates/loginSuccess.html:

    <!DOCTYPE HTML>
    <html xmlns:th="http://www.thymeleaf.org">
      <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
        <title>Success</title>
      </head>
      <body>
        <p>Login successful</p>
      </body>
    </html>

src/main/resources/templates/loginFailure.html:

    <!DOCTYPE HTML>
    <html xmlns:th="http://www.thymeleaf.org">
      <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
        <title>Failure</title>
      </head>
      <body>
        <p>Login failed</p>
      </body>
    </html>

* The case of the filenames is important, so MAKE SURE you match them up
correctly as once the application is created and bundled up, it may not
work if you didn't keep everything matched up properly.

The next step is to create the LoginController at
src/main/java/edu/carroll/cs389/web/controller/LoginController.java.

    package edu.carroll.cs389.web.controller;

    import edu.carroll.cs389.web.form.LoginForm;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.ModelAttribute;
    import org.springframework.web.bind.annotation.PostMapping;

    @Controller
    public class LoginController {
        @GetMapping("/login")
        public String loginGet(Model model) {
            model.addAttribute("loginForm", new LoginForm());
            return "login";
        }

        @PostMapping("/login")
        public String loginPost(@ModelAttribute LoginForm loginForm) {
            System.out.println("User '" + loginForm.getUsername() + "' attempted login");
            return "redirect:/loginSuccess";
        }

        @GetMapping("/loginSuccess")
        public String loginSuccess() {
            return "loginSuccess";
        }

        @GetMapping("/loginFailure")
        public String loginFailure() {
            return "loginFailure";
        }
    }

This is NOT the final version, but contains all of the code we need to
demonstrate the ability to traverse between pages. We do not do any
checking/validation in the loginPost method and just take the user's
data directly and redirect the browser directly to the success
page. We'll change this in a later step.

The final step to having a working (not necessarily correct) application
is to modify the index.html page to change the href from **#** to
**/login** since everything is now hooked up.

    <p><a href="/login">login</a> to continue!</p>

At this point, you should be able to connect to the site, click on the
login link, put some dummy data into the username/password field, and
then click the 'Login' button and successfully login to the
application. Whatever username you typed into the username field should
be printed on the console, ie;

    User 'MyName' attempted login

Save, test, and then commit your code to your repository, and then you
can compare your version to the reference repository.

```sh
git diff simple_login
```

Now, let's do something with the data, and ensure the user has provided
all of the necessary information.

First, we need to add the Spring validation dependency to the project,
which is done in build.gradle:

    implementation 'org.springframework.boot:spring-boot-starter-validation'

Next, let's modify our LoginForm to add some Validation annotations to
the fields. Both fields must be required, and let's make sure there are
minimum character lengths on both the username (6) and password (8) fields.

    @NotNull
    @Size(min = 6, message = "Username must be at least 6 characters long")
    private String username;

    @NotNull
    @Size(min = 8, message = "Password must be at least 8 characters long")
    private String password;

Next, let's add validation to the submission method for the login page.
This is as simple as adding a **@Valid** annotation to the form you want
validated, and adding a BindingResult parameter to the method.  With
both of those changes to the method signature, you can simply check for
any errors in the form, and if they exist, return the failed form.

    @PostMapping("/login")
    public String loginPost(@Valid @ModelAttribute LoginForm loginForm, BindingResult result) {
        System.out.println("User '" + loginForm.getUsername() + "' attempted login");
        if (result.hasErrors()) {
            return "login";
        }
        return "redirect:/loginSuccess";
    }


For the errors to be displayed, we need to make some slight changes to
the HTML template code, to add some error messages (see the span).

    <form action="#" th:action="@{/login}" th:object="${loginForm}" method="post">
      <input type="text" th:field="*{username}" placeholder="Username"/>
      <span th:if="${#fields.hasErrors('username')}" th:errors="*{username}">Username error</span><br/>
      <input type="password" th:field="*{password}" placeholder="Password"/>
      <span th:if="${#fields.hasErrors('password')}" th:errors="*{password}">Password error</span><br/>
      <button type="submit">Login</button>
    </form>

Again, styling is pretty bad, but the functionality is all there.

Let's also make one tweak to the successful login page, and send in the
name of the logged in username to the page.

This requires changes to two files, the HTML template, and the
LoginController.  First, the template file loginSuccess.html gets a
new **p** tag.

    <body>
      <p>Login successful</p>
      <p th:text="'Welcome ' + ${username} + '.'"/>
    </body>

Next, LoginController gets two modifications, one to the loginPost
method, and changes to the loginSuccess method.  For the former, we add
a new parameter (**RedirectAttributes**), which allows us to send data
to a page we redirect to.

    @PostMapping("/login")
    public String loginPost(@Valid @ModelAttribute LoginForm loginForm, BindingResult result, RedirectAttributes attrs) {
        if (result.hasErrors()) {
            return "login";
        }
        attrs.addAttribute("username", loginForm.getUsername());
        return "redirect:/loginSuccess";
    }

    @GetMapping("/loginSuccess")
    public String loginSuccess(String username, Model model) {
        model.addAttribute("username", username);
        return "loginSuccess";
    }

The latter accepts a new String parameter named username (note, we don't
HAVE to use **@RequestParam** if the field is a String and the name of
the field is identical to what is expected) and a Model so we can
forward the data to the template.

Let's do one final change to the application, and that's provide a
custom error based on server-side persistent. Note, what was done is NOT
the way that should be done for username/password validation, but was
done for demonstration purposes.  As mentioned in the comments, if you
do this in a real application, I will hunt you down and embarrass you in
front of your friends.

In the login.html template file, add a single line to display global
error messages.

    <form action="#" th:action="@{/login}" th:object="${loginForm}" method="post">
      <span th:if="${#fields.hasGlobalErrors()}" th:errors="*{global}">Global error</span><br/>  <!-- Add this line -->
      <input type="text" th:field="*{username}" placeholder="Username"/>

In LoginController, add the following snippets:

    @Controller
    public class LoginController {
        // XXX - If anything like this is used in a real application, I will hunt you down and embarrass you to all your peers.
        private static final String validUser = "cs389user";
        private static final String validPass = "supersecret";

And, later in loginPost (note, I also removed the println statement).

    @PostMapping("/login")
    public String loginPost(@Valid @ModelAttribute LoginForm loginForm, BindingResult result, RedirectAttributes attrs) {
        if (result.hasErrors()) {
            return "login";
        }
        // Username does not have to match the case, but password must be an exact match.
        // XXX - NEVER do password validation using a string match
        if (!(validUser.equalsIgnoreCase(loginForm.getUsername()) &&
              validPass.equals(loginForm.getPassword()))) {
            result.addError(new ObjectError("globalError", "Username and password do not match known users"));
            return "login";
        }
        ...

At this point, only a single username/password combination will be
allowed, which is hardcoded into the application code.

Save, test, and then commit your code to your repository, and then you
can compare your version to the reference repository.

```sh
git diff login_validation
```

Note, we're still missing unit tests for the new methods, which is bad,
and should be corrected (but is not going to happen quite yet).


SQL Backend Tier
----------------

At this point, we have a VERY basic application which contains at least
one decent unit test. However, most real applications allow for storing
data that can be referenced even if the application is restarted. The
most common storage mechanism used if a RDBMS (Relational Database
Management System), or SQL database.

So, instead of having a single (or multiple) hardcoded username and
password combinations that lives in code (VERY, VERY BAD!), we could
instead lookup a valid username/password combination from the database
to see if we have a match.

As we do at the start of all backends, we need to add some additional
dependencies to our project to bring in the new functionality. In this
case, we're adding JPA (Java Persistence API), which will allow us to
connect to our database. In addition, for this simple project, instead
of spinning up an entire separate database server, we're going to be
using H2, which allows us run everything as a built in process inside
our application, greatly simplifying the setup. This is not an
acceptable solutions for any modern application, but should suffice for
demonstration purposes.

In build.gradle, add the following dependencies:

    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    runtimeOnly 'com.h2database:h2'

Next, we'll need to configure the database connection setup, which is
done in the file src/main/resources/application.properties:

    spring.datasource.url=jdbc:h2:file:./demoDb
    spring.datasource.driverClassName=org.h2.Driver
    spring.datasource.username=h2
    spring.datasource.password=dbpass
    spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
    spring.jpa.hibernate.ddl-auto=update

This sets up H2 as the database for the application, storing the files
in the project directory with filenames starting with demoDb (ie;
demoDb.db, etc..), and ensures that JPA will create and/or update the
tables based on the provided models (defined below).  The last property
is VERY dangerous, and should never be done in a production application,
as creating and modifying the SQL database structure should normally not
be done by the application code. However, for our demo and for quickly
prototyping an application, it makes things easy.

If you run the application now, it will create an H2 database file
containing all the structure necessary to run an application.

To avoid committing the H2 database files, I'd add an entry to
.gitignore to tell it to not consider the H2 database files part of the
project files.

I added the following lines to my .gitignore

    # H2 database files
    /demoDb*

Now that we've got a database, lets create ourself a Model, which is the
way that we interact with the database. Rather than creating tables and
rows, we create a Java class that represents the table.

Create a new package named **jpa** and a sub-package under it named
**model**, and then create a new class named Login, which will map to
our new database table, including all of the columns in the table.

src/main/java/edu/carroll/cs389/jpa/model/Login.java:

    package edu.carroll.cs389.jpa.model;

    import java.util.Objects;

    import jakarta.persistence.Column;
    import jakarta.persistence.Entity;
    import jakarta.persistence.GeneratedValue;
    import jakarta.persistence.Id;
    import jakarta.persistence.Table;

    @Entity
    @Table(name = "login")
    public class Login {
        private static final long serialVersionUID = 1L;

        @Id
        @GeneratedValue
        private Integer id;

        @Column(name = "username", nullable = false, unique = true)
        private String username;

        @Column(name = "password", nullable = false)
        private String hashedPassword;

        public Integer getId() {
            return id;
        }

        public void setId(Integer id) {
            this.id = id;
        }

        public String getUsername() {
            return username;
        }

        public void setUsername(String username) {
            this.username = username;
        }

        public String getHashedPassword() {
            return hashedPassword;
        }

        public void setHashedPassword(String hashedPassword) {
            this.hashedPassword = hashedPassword;
        }

        private static final String EOL = System.lineSeparator();
        private static final String TAB = "\t";

        @Override
        public String toString() {
            StringBuilder builder = new StringBuilder();
            builder.append("Login @ ").append(super.toString()).append("[").append(EOL);
            builder.append(TAB).append("username=").append(username).append(EOL);
            builder.append(TAB).append("hashedPassword=").append("****").append(EOL);
            builder.append("]").append(EOL);
            return builder.toString();
        }

        @Override
        public boolean equals(Object o) {
            if (this == o)
                return true;
            if (o == null || getClass() != o.getClass())
                return false;

            final Login login = (Login)o;
            return username.equals(login.username) && hashedPassword.equals(login.hashedPassword);
        }

        @Override
        public int hashCode() {
            return Objects.hash(username, hashedPassword);
        }
    }

Lots of the above code is boiler-plate and/or code that you want to have
to make life easier (getter/setter, toString, equals, and hashCode), which can be
generated by your IDE. The code is also very verbose in that I was very
explicit in the naming of the tables and columns. By convention, many
companies prefer to keep all of the database objects using a single
case (not a mix of upper or lower case names), so I've followed the
convention to be explicit in the names for both by using all lower-case
names.

The Id field for the table is a sequence number that is auto-generated
by the database at insertion time, not the username.  So, in order to
ensure that a username can't be shared, we added a unique attribute to
the **@Column** annotation.

The next thing we need to do is create some code that will allow us to
access the data in the login table.  In Spring JPA, this is done with a
JpaRepository.  We'll put the code in a new package, which is as
follows:

src/main/java/edu/carroll/cs389/jpa/repo/LoginRepository.java:

    package edu.carroll.cs389.jpa.repo;

    import java.util.List;

    import edu.carroll.cs389.jpa.model.Login;
    import org.springframework.data.jpa.repository.JpaRepository;

    public interface LoginRepository extends JpaRepository<Login, Integer> {
        // JPA throws an exception if we attempt to return a single object that doesn't exist, so return a list
        // even though we only expect either an empty list of a single element.
        List<Login> findByUsernameIgnoreCase(String username);
    }

Now, this is where things get interesting, and somewhat verbose. Good
application design breaks up the business logic from the actual data,
and breaks up the persistence from either of those. This model is called
MVC, or Model (data), View (UI), and Controller (business logic/routing,
etc...).

To date, we've have a controller and a template, which makes up both the
Controller and View instance, and we're currently modeling the
Data. However, by convention, we want to add an additional business
layer on top of the Data, which we'll call the service layer, which
abstracts away the actual details of the data away from the UI. For
this, we're going to create another service layer, which contains the
operations we are using to operate on the data.

So, let's create another package, this one in a new sub-package named
service.  But, because we want to ensure that we do things in a more
portable way (and to make it easier to test the service), we'll first
create an interface that defines the type of business operations that we
want to do, and then once that's been defined, we'll actually create the
implementation.

So, create an interface file, which we'll call LoginService, which will
contain all the methods we need to do in order to properly implement a
login service.

src/main/java/edu/carroll/cs389/service/LoginService.java:

    package edu.carroll.cs389.service;

    import edu.carroll.cs389.web.form.LoginForm;

    public interface LoginService {
        /**
         * Given a loginForm, determine if the information provided is valid, and the user exists in the system.
         * @param form - Data containing user login information, such as username and password.
         * @return true if data exists and matches what's on record, false otherwise
         */
        boolean validateUser(LoginForm form);
    }

Pretty straight forward, although we'll need to expand it in the future,
as right now we have no way of adding users to the system, only validate
users.

Before going further, let's modify the LoginController to use the new
service. We can cleanup the hard-coded username and mess we made in the
controller previously.

    public class LoginController {
        private final LoginService loginService;

        public LoginController(LoginService loginService) {
            this.loginService = loginService;
        }

Next, the code for loginPost is much simpler:

    @PostMapping("/login")
    public String loginPost(@Valid @ModelAttribute LoginForm loginForm, BindingResult result, RedirectAttributes attrs) {
        if (result.hasErrors()) {
            return "login";
        }
        if (!loginService.validateUser(loginForm)) {
            result.addError(new ObjectError("globalError", "Username and password do not match known users"));
            return "login";
        }
        attrs.addAttribute("username", loginForm.getUsername());
        return "redirect:/loginSuccess";
    }

With the interface and the changes, your application should now compile
successfully, but will *NOT* run, due to two issues.  First, it will
fail to run at all and will exit if the application tries to run as
there is no implementation of the LoginService needed by the
LoginController (which actually contains the code to validateUser().

Second, there are also NO users in the database to match against.

However, we're getting much closer to having something!  Verify
everything compiles at this point, and then we'll move onto the last
step of getting the coding completed, and that's creating the
implementation of the LoginService.

src/main/java/edu/carroll/cs389/service/LoginServiceImpl.java:

    import edu.carroll.cs389.jpa.model.Login;
    import edu.carroll.cs389.jpa.repo.LoginRepository;
    import edu.carroll.cs389.web.form.LoginForm;
    import org.springframework.stereotype.Service;

    @Service
    public class LoginServiceImpl implements LoginService {
        private final LoginRepository loginRepo;

        public LoginServiceImpl(LoginRepository loginRepo) {
            this.loginRepo = loginRepo;
        }

        /**
         * Given a loginForm, determine if the information provided is valid, and the user exists in the system.
         *
         * @param loginForm - Data containing user login information, such as username and password.
         * @return true if data exists and matches what's on record, false otherwise
         */
        @Override
        public boolean validateUser(LoginForm loginForm) {
            // Always do the lookup in a case-insensitive manner (lower-casing the data).
            List<Login> users = loginRepo.findByUsernameIgnoreCase(loginForm.getUsername());

            // We expect 0 or 1, so if we get more than 1, bail out as this is an error we don't deal with properly.
            if (users.size() != 1)
                return false;
            Login u = users.get(0);
            // XXX - Using Java's hashCode is wrong on SO many levels, but is good enough for demonstration purposes.
            // NEVER EVER do this in production code!
            final String userProvidedHash = Integer.toString(loginForm.getPassword().hashCode());
            if (!u.getHashedPassword().equals(userProvidedHash))
                return false;

            // User exists, and the provided password matches the hashed password in the database.
            return true;
        }
    }

As you can tell, the service code knows how to use the database, but
provides are more descriptive user interface so that developers can use
more descriptive method names for describing the operations they want to
accomplish.

The last thing we need to do is get a record added to the database
containing at least one user/password pair, but first, we need to know
the hashCode of our password.  Using the following class:

    public class Pass {
        public static void main(String[] args) {
            final String hash = Integer.toString("supersecret".hashCode());
            System.out.println(hash);
        }
    }

I got the following output:

    -1164577301

So, I'd like to store that value in my database. Note, there are
numerous ways to pre-populate the database at startup, most of them bad
(including the provided solution). This should be done with a separate
process that loads the data externally, but for purposes of the demo,
we're populating the data using code.  (And, the code itself should
containing the data should normally never be committed to a git repo, as
credentials should never be in code.)

The following class runs code at startup and determines if the default
user has been added to the database, and if not, it creates the user and
persists it to the DB.

src/main/java/edu/carroll/cs389/DbInit.java:

    package edu.carroll.cs389;

    import java.util.List;

    import edu.carroll.cs389.jpa.model.Login;
    import edu.carroll.cs389.jpa.repo.LoginRepository;
    import jakarta.annotation.PostConstruct;
    import org.springframework.stereotype.Component;

    // This class optionally pre-populates the database with login data.  In
    // a real application, this would be done with a completely different
    // method.
    @Component
    public class DbInit {
        // XXX - This is wrong on so many levels....
        private static final String defaultUsername = "cs389user";
        private static final String defaultPassHash = "-1164577301";

        private final LoginRepository loginRepo;

        public DbInit(LoginRepository loginRepo) {
            this.loginRepo = loginRepo;
        }

        // invoked during application startup
        @PostConstruct
        public void loadData() {
            // If the user doesn't exist in the database, populate it
            final List<Login> defaultUsers =
        loginRepo.findByUsernameIgnoreCase(    defaultUsername);
            if (defaultUsers.isEmpty()) {
                Login defaultUser = new Login();
                defaultUser.setUsername(defaultUsername);
                defaultUser.setHashedPassword(defaultPassHash);
                loginRepo.save(defaultUser);
            }
        }
    }

With this change in place, you should now be able to run the
application, and successfully login to the application using the
username/password information cs389user/supersecret.

Verify you can run the code, commit it to your repo, and then compare
your version to the reference repository.

```sh
git diff jpa_login
```

Coupling Issues
---------------

What we have created so far works fine, but in reality, we have a
coupling/layering issue in the LoginService. Specifically, the method
`validUser` takes a *LoginForm* argument, which is arguably an issue,
because LoginForm is part of the HTML/web layer, and LoginService should
be callable by any layer. If we decide to provide multiple front-ends
to the application, such that the user could interact with a mobile
app and/or a javascript front-end, they would almost certainly not
use a LoginForm to accept the user's input.

To fix this, we should modify the service to take data that is less
web layer specific. This can be done a multitude of ways, including
defining a new class that just contains the data the service needs, such
as something that looks very much like LoginForm (without the web
validation tags), or we can simply use two Strings, which is what we'll do.

Modify the signature in LoginService to take two strings, which will
require changes to the LoginServiceImpl and any classes that call it.

src/main/java/edu/carroll/cs389/service/LoginService.java:

    package edu.carroll.cs389.service;

    public interface LoginService {
        /**
         * Given a loginForm, determine if the information provided is valid, and the user exists in the system.
         * @param username - Username of the person attempting to login
         * @param password - Raw password provided by the user logging in
         * @return true if data exists and matches what's on record, false otherwise
         */
        boolean validateUser(String username, String password);
    }

I'll leave you to figure out the details for the changes, and once you've
verified the code works as before, commit it to your repo,a nd then compare
your updated version to the reference repository.

```sh
git diff login_layering
```

Application Logging
-------------------

One of the last topics we're going to introduce is application logging. Bugs
occur in code based on a variety of reasons, and we rarely ever know
ahead of time the cause of the bug, so we must add in logging into our
application so we can more easily determine the cause of bugs.

In addition to logging, for large scale applications, the application is
running in an environment such that the developer may not have direct
access to the actual running instance, so the only way to debug the
issue is to look at the logs that occurred prior-to, during, and after
an error occurred.

Using a logging framework allows the application to generate logs in a
consistent manner that provides the developer much more context of
what's going on. We'll be using the most common logging framework
[Logback](https://logback.qos.ch/) used in Java applications, and
setting up logging for the applications.

First, for every class that we want to have logging enabled, we must
generate a shared logger class. Doing this is relatively
straight-forward, and we'll provide some good default configuration
files you can use to setup logging.

Per the spring recommendations, let's create the configuration file for
logback, using the filename they recommend using.

src/main/resources/logback-spring.xml:

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <appender name="Console" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
          <pattern>%date - [%level] - from %logger in %thread %n%message%n%xException%n</pattern>
        </encoder>
      </appender>

      <property name="LOGS" value="./logs" />

      <appender name="Logfile" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${LOGS}/spring-boot-logger.log</file>
        <encoder>
          <pattern>%date - [%level] - from %logger in %thread %n%message%n%xException%n</pattern>
        </encoder>

        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
          <!-- rollover daily and when the file reaches 10 MegaBytes -->
          <fileNamePattern>${LOGS}/archived/spring-boot-logger-%d{yyyy-MM-dd}.%i
         .log</fileNamePattern>
          <timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
            <maxFileSize>10MB</maxFileSize>
          </timeBasedFileNamingAndTriggeringPolicy>
        </rollingPolicy>
      </appender>

      <!--
        The logger name is typically the Java/Scala package name.
        This configures the log level to log at for a package and its children packages.
      -->
      <!-- Overwrite level for a internal code - Commented out for now
      <logger name="edu.carroll.cs389" level="DEBUG"/>
      -->

      <!-- Reduce DB startup chatter -->
      <logger name="org.hibernate" level="INFO" />
      <logger name="com.zaxxer.hikari" level="INFO" />

      <!-- Default level is INFO -->
      <root level="INFO">
        <appender-ref ref="Console"/>
        <appender-ref ref="Logfile"/>
      </root>
    </configuration>

With this in place, for EVERY class that you want to add logging to, you
need to add some boilerplate code. Note, there are ways to setup
logging in a generic way, but as you will find out as you write code,
you really need to write log messages that are specific to your
implementation, to give the most context to the application developer
reading the logs after-the-fact. In addition, you need to make sure that
any information in your logs does not contain any information that could
be compromise the users who are using your system. (For example, never
log out information should as SSN and/or other financial information if
that information could be tied to a particular user).

Because the business logic we have is probably the most useful location
for logging, we'll start with adding logging in that class.

src/main/java/edu/carroll/cs389/service/LoginServiceImpl.java:

Add the two log imports

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

Create the static Logger for the class (note, the class name MUST match
the classname).

    @Service
    public class LoginServiceImpl implements LoginService {
        private static final Logger log = LoggerFactory.getLogger(LoginServiceImpl.class);

Add the log lines (both info and debug), as shown:

    public boolean validateUser(String username, String password) {
        log.debug("validateUser: user '{}' attempted login", username);

        // Always do the lookup in a case-insensitive manner (lower-casing the data).
        List<Login> users = loginRepo.findByUsernameIgnoreCase(username);

        // We expect 0 or 1, so if we get more than 1, bail out as this is an error we don't deal with properly.
        if (users.size() != 1) {
            log.debug("validateUser: found {} users", users.size());
            return false;
        }
        Login u = users.get(0);
        // XXX - Using Java's hashCode is wrong on SO many levels, but is good enough for demonstration purposes.
        // NEVER EVER do this in production code!
        final String userProvidedHash = Integer.toString(password.hashCode());
        if (!u.getHashedPassword().equals(userProvidedHash)) {
            log.debug("validateUser: password !match");
            return false;
        }

        // User exists, and the provided password matches the hashed password in the database.
        log.info("validateUser: successful login for {}", username);
        return true;
    }

Choosing the correct level to log the message is based on experience, but a
good first step is to use **INFO** messages for normal kind of operations that
may lead to issues later, and **DEBUG** messages are a form of documentation,
and they allow you to do a deeper dive into the behavior of the code if you
suspect an issue. With proper debugging in palce, all it takes is a simple
configuration file change and a redeploy, and your logging information
can be greatly increased without having to make any coding changes.

Last but not least, because the application is now generating logfiles
into the log directory, we need to tell git to ignore all the files in
the log directory.

I added the following lines to my .gitignore file.

    # Logfiles
    logs/

Verify you can run the code, commit it to your repo, and then compare
your version to the reference repository.

```sh
git diff service_logging
```

Service Unit Test
-----------------

Doing unit tests for HTML controllers is error prone, as often-times the
developer creating the pages is often different from the person writing
the backend, and their skillsets vary widely. So, most HTML controllers
don't have much for unit tests.

However, backend services with all the business logic are RIPE for
testing, and are much easier to setup for testing. Plus, the value of
the tests tend to have much greater value.

So, the last thing we're going to do is create a unit test for the
LoginService.

Before doing that, let's add a convenience constructors and methods to
make the code shorter.

In the JPA Login model, let's add a constructor that takes two Strings,
one the username, and the second the raw string password that converts
it to a hashed value, along with a new setter that does the
onversion. Note, since we added a custom constructor, we must also add a
no-arg constructor, which is used internally by the JPA framework to
work properly.

In /src/main/java/edu/carroll/cs389/jpa/model/Login.java, add the
following constructors.

    public Login() {
    }

    public Login(String username, String rawPassword) {
        this.username = username;
        setRawPassword(rawPassword);
    }

And then the convenience method:

    public void setRawPassword(String rawPassword) {
        // XXX - This should *NEVER* be done in a real project
        this.hashedPassword = Integer.toString(rawPassword.hashCode());
    }

Note, this method is just a setter, and it uses a completely different
method name, and is not directly associated with the class
properties/field names.

With that done, let's setup some configuration files to be used for
testing, since our initial unit test didn't have either database or
logging configured, and we'd like to make sure our test environment is
configured as closely to the normal environment as we can make it.

Add the following configuration files, which should only be used during
unit testing.

src/test/resources/application.properties:

    spring.datasource.url=jdbc:h2:mem:test;DB_CLOSE_ON_EXIT=FALSE
    spring.datasource.driver-class-name=org.h2.Driver
    spring.jpa.hibernate.ddl-auto=create

Note, the database configure is much simpler than the production setup,
as we're using an in-memory database. Using a COMPLETELY SEPARATE
database for testing is not only common, it's almost certainly something
you should do in your application. NEVER EVER write test code that
writes to your production database, especially when you can easily (as
we're showing here) configure the application to use a database during
tests without effecting the normal production database.

Next, add the logging configuration file, which is also simpler (no need
to log to a file), with a couple of additions to decrease initialization
logging and to increase logging in the concrete classes during testing.

src/test/resources/logback-spring.xml:

    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
      <appender name="Console" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
          <pattern>%date - [%level] - from %logger in %thread %n%message%n%xExce    ption%n</pattern>
        </encoder>
      </appender>

      <!-- Reduce DB startup chatter -->
      <logger name="org.hibernate" level="WARN" />
      <logger name="com.zaxxer.hikari" level="WARN" />

      <!--- Spring boot has a lot of startup noise as well -->
      <logger name="org.springframework" level="WARN" />

      <!-- Increase verbosity for local classes during testing -->
      <logger name="edu.carroll.cs389" level="DEBUG" />

      <!-- Default level is INFO -->
      <root level="INFO">
        <appender-ref ref="Console"/>
      </root>
    </configuration>

With these two configuration files, we can now create a unit test class
for the LoginService.

As before, it should exist in the same package as the class that's being
tested, with **Test** appended to the name by convention.

 src/test/java/edu/carroll/cs389/service/LoginServiceTest.java:

    package edu.carroll.cs389.service;

    import static org.springframework.test.util.AssertionErrors.assertFalse;
    import static org.springframework.test.util.AssertionErrors.assertNotNull;
    import static org.springframework.test.util.AssertionErrors.assertTrue;

    import java.util.List;

    import edu.carroll.cs389.jpa.model.Login;
    import edu.carroll.cs389.jpa.repo.LoginRepository;
    import org.junit.jupiter.api.BeforeEach;
    import org.junit.jupiter.api.Test;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.test.context.SpringBootTest;

    @SpringBootTest
    public class LoginServiceTest {
        private static final String username = "testuser";
        private static final String password = "testpass";

        @Autowired
        private LoginService loginService;

        @Autowired
        private LoginRepository loginRepo;

        private Login fakeUser = new Login(username, password);

        @BeforeEach
        public void beforeTest() {
            assertNotNull("loginRepository must be injected", loginRepo);
            assertNotNull("loginService must be injected", loginService);

            // Ensure dummy record is in the DB
            final List<Login> users = loginRepo.findByUsernameIgnoreCase(username);
            if (users.isEmpty())
                loginRepo.save(fakeUser);
        }

        @Test
        public void validateUserSuccessTest() {
            assertTrue("validateUserSuccessTest: should succeed using the same user/pass info", loginService.validateUser(username, password));
        }

        @Test
        public void validateUserExistingUserInvalidPasswordTest() {
            assertFalse("validateUserExistingUserInvalidPasswordTest: should fail using a valid user, invalid pass", loginService.validateUser(username, password + "extra"));
        }

        @Test
        public void validateUserInvalidUserValidPasswordTest() {
            assertFalse("validateUserInvalidUserValidPasswordTest: should fail using an invalid user, valid pass", loginService.validateUser(username + "not", password));
        }

        @Test
        public void validateUserInvalidUserInvalidPasswordTest() {
            assertFalse("validateUserInvalidUserInvalidPasswordTest: should fail using an invalid user, valid pass", loginService.validateUser(username + "not", password + "extra"));
        }
    }

This test should pass, and contains some very simple tests. Once could
argue that the provided tests may be too simple (specifically, an
invalid username with a 'valid' password may seem like a silly test to
run, but perhaps there is a bug where the password is stored globally,
and once stored, it becomes valid of all users or something silly like
that).

Unit tests should test for even weird error cases, especically if the
cost for writing and running the tests is so small.

Verify the entire project runs properly (both the actual application
functionality as well as all the units tests), commit all your changes
to your git repo, and then compare your version to the code in the
reference repository.

```sh
git diff final_tests
```

Congratulations! At this point, we're done with this initial Spring
Boot tutorial. We'll be going over each of the topics in much greater
detail during class, but for now, you have completed the tutorial.
