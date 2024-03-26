# A Primer on Selenium

> Reference Book:
> 
> Hands-On Selenium WebDriver with Java
> 
> By Boni Garcia

---

[*Selenium*](https://www.selenium.dev/) is an open source suite composed of a set of libraries and tools that enable the automation of web browsers. We can see Selenium as an umbrella project with three core components: WebDriver, Grid, and IDE (Integrated Development Environment). Selenium WebDriver is a library that allows the driving of browsers programmatically. Thus, we can use Selenium WebDriver to navigate websites and interact with web pages (e.g., clicking on links, filling in forms, etc.) as a real user would do, in an automated fashion. The primary use of Selenium WebDriver is the automated testing of web applications. Other Selenium uses include the automation of web-based administration tasks or web scraping (automated web data extraction).

This chapter provides a comprehensive overview of the Selenium core components: WebDriver, Grid, and IDE. Then, it reviews the Selenium ecosystem, i.e., other tools and technologies around it. Finally, it analyzes the foundations of software testing related to Selenium.

# Selenium Core Components

Jason Huggins and Paul Hammant created Selenium in 2004 while working in Thoughtworks. They chose the name “Selenium” as a counterpart to Mercury, an existing testing framework developed by Hewlett-Packard. The name is significant because the chemical selenium is known for reducing the toxicity of mercury.

That initial version of Selenium (known today as *Selenium Core*) is a JavaScript library that impersonates user actions in web applications. Selenium Core interprets the so-called *Selenese* commands to achieve this task. These commands are encoded as an HTML table composed of three parts: *command* (action executed in a web browser, such as opening a URL or clicking a link), *target* (locator that identifies a web element, such as the attribute of a given component), and *value* (optional data, such as the text typed into a web-form field).

Huggins and Hammant added a scripting layer to Selenium Core in a new project called *Selenium Remote Control* (RC). Selenium RC follows a client-server architecture. Clients use a binding language (such as Java or JavaScript) to send Selenese commands over HTTP to an intermediate proxy called the *Selenium RC Server*. This server launches web browsers on demand, injecting the Selenium Core library into a website and proxying requests from clients to Selenium Core. In addition, the Selenium RC Server masks the target website to the same local URL of the injected Selenium Core library to avoid same-origin policy concerns. This approach was a game-changer for browser automation at that time, but it had significant limitations. First, because JavaScript is the underlying technology to support automation, some actions are not permitted since JavaScript does not allow them—for instance, uploading and downloading files or handling pop-ups and dialogs, to name a few. Besides, Selenium RC introduces a relevant overhead that impacts its performance.

In parallel, Simon Stewart created the project *WebDriver* in 2007. WebDriver and Selenium RC were equivalent from a functional perspective, i.e., both projects allow programmers to impersonate web users using a programming language. Nevertheless, WebDriver uses the native support of each browser to carry out the automation, and therefore, its capabilities and performance are far superior to RC. In 2009, after a meeting between Jason Huggins and Simon Stewart at the Google Test Automation Conference, they decided to merge Selenium and WebDriver in a single project. The new project was called *Selenium WebDriver* or Selenium 2. This new project uses a communication protocol based on HTTP combined with the native automation support on the browser. That approach is still the basis of Selenium 3 (released in 2016) and Selenium 4 (released in 2021). Now we refer to Selenium RC and Core as “Selenium 1,” and its use is discouraged in favor of Selenium WebDriver. This book focuses on the latest version of Selenium WebDriver to date, i.e., version 4.

Today, Selenium is a well-known automation suite composed of three subprojects: WebDriver, Grid, and IDE. The following subsections present the main characteristics of each one.

## Selenium WebDriver

Selenium WebDriver is a library that allows the controlling of web browsers automatically. To that aim, it provides a cross-platform API in different language bindings. The official programming languages supported by Selenium WebDriver are Java, JavaScript, Python, Ruby, and C#. Internally, Selenium WebDriver uses the native support implemented by each browser to carry out the automation process. For this reason, we need to place a component called *driver* between the script using the Selenium WebDriver API and the browser.

###### NOTE

The name *Selenium* is widely used to refer to the library for browser automation. Since this term is also the name of the umbrella project, I use *Selenium* in this book to identify the browser automation suite, which is composed of three components: Selenium WebDriver (library), Selenium Grid (infrastructure), and Selenium IDE (tool).

Drivers (e.g., chromedriver, geckodriver, etc.) are platform-dependent binary files that receive commands from a WebDriver script and translate them into some browser-specific language. In the first releases of Selenium WebDriver (i.e., in Selenium 2), these commands (also known as the *Selenium protocol*) were JSON messages over HTTP (the so-called *JSON Wire Protocol*). Nowadays, this communication (still JSON over HTTP) follows a standard specification named [*W3C WebDriver*](https://www.w3.org/TR/webdriver). This specification is the preferred Selenium protocol as of Selenium 4.

Figure 1-1 summarizes the basic architecture of Selenium WebDriver we have seen so far. As you can see, this architecture has three tiers. First, we find a script using the Selenium WebDriver API (Java, JavaScript, Python, Ruby, or C#). This script sends W3C WebDriver commands to the second layer, in which we find the drivers. This figure shows the specific case of using chromedriver (to control Chrome) and geckodriver (to control Firefox). Finally, the third layer contains the web browsers. In the case of Chrome, the native browser follows the [*DevTools Protocol*](https://chromedevtools.github.io/devtools-protocol). DevTools is a set of developer tools for browsers based on the Blink rendering engine, such as Chrome, Chromium, Edge, or Opera. The DevTools Protocol is based on JSON-RPC messages and allows inspecting, debugging, and profiling these browsers. In Firefox, the native automation support uses the [*Marionette*](https://firefox-source-docs.mozilla.org/testing/marionette) protocol. Marionette is a remote protocol based on JSON, allowing instrumenting and controlling web browsers based on the Gecko engine (such as Firefox).

![hosw 0101](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781098109998/files/assets/hosw_0101.png)

###### Figure 1-1. Selenium WebDriver architecture

Overall, Selenium WebDriver allows controlling web browsers as a user would, but programmatically. To that aim, the Selenium WebDriver API provides a wide variety of features to navigate web pages, interact with web elements, or impersonate user actions, among many other capabilities. The target application is web-based, such as static websites, dynamic web applications, Single Page Applications (SPA), complex enterprise systems with a web interface, etc.

## Selenium Grid

The second project of the Selenium family is *Selenium Grid*. Philippe Hanrigou started the development of this project in 2008. Selenium Grid is a group of networked hosts that provides browser infrastructure for Selenium WebDriver. This infrastructure enables the (parallel) execution of Selenium WebDriver scripts with remote browsers of a different nature (types and versions) in multiple operating systems.

Figure 1-2 shows the basic architecture of Selenium Grid. As you can see, a group of nodes provides browsers used by Selenium scripts. These nodes can use different operating systems with various installed browsers. The central entry point to this Grid is the *Hub* (also known as *Selenium Server*). This server-side component keeps track of the nodes and proxies requests from the Selenium scripts. Like in Selenium WebDriver, the W3C WebDriver specification is the standard protocol for the communication between these scripts and the Hub.

![hosw 0102](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781098109998/files/assets/hosw_0102.png)

###### Figure 1-2. Selenium Grid hub-nodes architecture

The hub-nodes architecture in Grid has been available since Selenium 2. This architecture is also present in Selenium 3 and 4. Nevertheless, this centralized architecture can lead to performance bottlenecks if the number of requests to the Hub is high. Selenium 4 provides a fully distributed flavor of Selenium Grid to avoid this problem. This architecture implements advanced load balancing mechanisms to avoid overloading any component.

## Selenium IDE

[Selenium IDE](https://www.selenium.dev/selenium-ide) is the last core component of the Selenium suite. Shinya Kasatani created this project in 2006. Selenium IDE is a tool that implements the so-called *Record and Playback* (R&P) automation technique. As the name suggests, this technique has two steps. First, in Selenium IDE, the *record* part captures user interactions with a browser, encoding these actions as Selenium commands. Second, we use the generated Selenium script to execute a browser session automatically (*playback*).

This early version of Selenium IDE was a Firefox plug-in that embedded Selenium Core to record, edit, and play back Selenium scripts. These early versions were XPI modules (i.e., a technology used to create Mozilla extensions). As of version 55 (released in 2017), Firefox migrated support for add-ons to the [W3C Browser Extension specification](https://browserext.github.io/browserext). As a result, Selenium IDE was discontinued, and for some time, it has not been possible to use it. The Selenium team rewrote Selenium IDE following the Browser Extensions recommendation to solve this problem. Thanks to this, we can now use Selenium IDE in multiple browsers, such as Chrome, Edge, and Firefox.

Figure 1-3 shows the new Selenium IDE GUI (Graphical User Interface).

Using this GUI, users can record interactions with a browser and edit and execute the generated script. Selenium IDE encodes each interaction in different parts: a command (i.e., the action executed in the browser), a target (i.e., the locator of the web element), and a value (i.e., the data handled). Optionally, we can include a description of the command. Figure 1-3 shows a recorded example of these steps:

1. Open website ([Hands-On Selenium WebDriver with Java](https://bonigarcia.dev/selenium-webdriver-java)). We will use this website as the practice site in the rest of the book.

2. Click on the link with the text “GitHub.” As a result, the navigation moves to the examples repository source code.

3. Assert that the book title (*Hands-On Selenium WebDriver with Java*) is present on the web page.

4. Close the browser.

![hosw 0103](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781098109998/files/assets/hosw_0103.png)

###### Figure 1-3. Selenium IDE showing an example of a recorded script

Once we have created a script in Selenium IDE, we can export this script as a Selenium WebDriver test. For instance, Figure 1-4 shows how to convert the presented example as a JUnit test case. Finally, we can save the project on our local machine. The resulting project for this sample is available in the [examples GitHub repository](https://github.com/bonigarcia/selenium-webdriver-java/tree/master/selenium-ide).

###### NOTE

The Selenium project is porting Selenium IDE to [Electron](https://www.electronjs.org/) at the time of this writing. Electron is an open source framework based on Chromium and Node.js that allows desktop application development.

![hosw 0104](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781098109998/files/assets/hosw_0104.png)

###### Figure 1-4. Exporting a Selenium IDE script to a JUnit test case

# Selenium Ecosystem

Software ecosystems are collections of elements interacting with a shared market underpinned by a common technological background. In the case of Selenium, its ecosystem involves the official core projects and other related projects, libraries, and actors. This section reviews the Selenium ecosystem, divided into the following categories: language bindings, driver managers, frameworks, browser infrastructure, and community.

## Language Bindings

As we already know, the Selenium project maintains various language bindings for Selenium WebDriver: Java, JavaScript, Python, Ruby, and C#. Nevertheless, other languages are also available.

## Driver Managers

Drivers are mandatory components to control web browsers natively with Selenium WebDriver. For this reason, before using the Selenium WebDriver API, we need to manage these drivers. *Driver management* is the process of downloading, setting up, and maintaining the proper driver for a given browser. The usual steps in the driver management procedure are:

1. Download

Each browser has its own driver. For example, we use chromedriver for controlling Chrome or geckodriver for Firefox. The driver is a platform-specific binary file. Therefore, we need to download the proper driver for a given operating system (typically, Windows, macOS, or Linux). In addition, we need to consider the driver version since a driver release is compatible with a given browser version (or range). For example, to use Chrome 91.x, we need to download chromedriver 91.0.4472.19. We usually find the browser-driver compliance in the driver documentation or release notes.

2. Setup

Once we have the proper driver, we need to make it available in our Selenium WebDriver script.

3. Maintenance

Modern web browsers (e.g., Chrome, Firefox, or Edge) upgrade themselves automatically and silently, without prompting the user. For this reason, and concerning Selenium WebDriver, we need to maintain the browser-driver version compatibility in time for these so-called *evergreen* browsers.

As you can see, the driver maintenance process can be time-consuming. Furthermore, it can cause problems for Selenium WebDriver users (e.g., failed tests due to browser-driver incompatibility after an automatic browser upgrade). For this reason, the so-called *driver managers* aim to carry out the driver management process in an automated fashion to some extent.

## Locator Tools

The Selenium WebDriver API provides different ways to locate web elements: by attribute (id, name, or class), by link text (complete or partial), by tag name, by CSS (Cascading Style Sheets) selector, or by XML Path Language (XPath). Specific tools can help to identify and generate these locators.

## Frameworks

In software engineering, a *framework* is a set of libraries and tools used as a conceptual and technological base and support for software development. Selenium is the foundation for frameworks that wrap, enhance, or complement its default features.

## Browser Infrastructure

We can use Selenium WebDriver to control local browsers installed in the machine running the WebDriver script. Also, Selenium WebDriver can drive remote web browsers (i.e., those executed in other hosts). In this case, we can use Selenium Grid to support the remote browser infrastructure. Nevertheless, this infrastructure can be challenging to create and maintain.

Alternatively, we can use a *cloud provider* to outsource the responsibility for supporting the browser infrastructure. In the Selenium ecosystem, a cloud provider is a company or product that supplies managed services for automated testing. These companies typically offer commercial solutions for web and mobile testing. The users of a cloud provider request on-demand browsers of different types, versions, and operating systems. Also, these providers typically offer additional services for easing the testing and monitoring activities, such as access to session recordings or analysis capabilities, to name a few. Some of the most relevant cloud providers for Selenium nowadays are [Sauce Labs](https://saucelabs.com/), [BrowserStack](https://www.browserstack.com/), [LambdaTest](https://www.lambdatest.com/), [CrossBrowserTesting](https://crossbrowsertesting.com/), [Moon Cloud](https://aerokube.com/moon-cloud), [TestingBot](https://testingbot.com/), [Perfecto](https://www.perfecto.io/), or [Testinium](https://testinium.com/).

Another solution we can use to support the browser infrastructure for Selenium is [*Docker*](https://www.docker.com/). Docker is an open source software technology that allows users to pack and run applications as lightweight, portable containers. The Docker platform has two main components: the *Docker Engine*, a tool for creating and running containers, and the [*Docker Hub*](https://hub.docker.com/), a cloud service for distributing Docker images. In the Selenium domain, we can use Docker to pack and execute containerized browsers.

## Community

Due to its collaborative nature, software development needs the organization and interaction of many participants. In the open source domain, we can measure the success of a project by the relevance of its community. Selenium is supported by a large community of many different participants worldwide.

# Software Testing Fundamentals

Software testing (or simply *testing*) consists of the dynamic evaluation of a piece of software, called *System Under Test* (SUT), through a finite set of test cases (or simply *tests*), giving a verdict about it. Testing implies the execution of SUT using specific input values to assess the outcome or expected behavior.

At first glance, we distinguish two separate categories of software testing: manual and automated. On the one hand, in *manual testing*, a person (typically a software engineer or the final user) evaluates the SUT. On the other hand, in *automated testing*, we use specific software tools to develop tests and control their execution against the SUT. Automated tests allow the early detection of defects (usually called *bugs*) in the SUT while providing a large number of additional benefits (e.g., cost savings, fast feedback, test coverage, or reusability, to name a few). Manual testing can also be a valuable approach in some cases, for example, in *exploratory testing* (i.e., human testers freely investigate and evaluate the SUT).

###### NOTE

There is no universal classification for the numerous forms of testing presented in this section. These concepts are subject to continuous evolution and debate, just like software engineering. Consider it a proposal that can fit into a large number of projects.

## Levels of Testing

Depending on the size of the SUT, we can define different *levels of testing*. These levels define several categories in which software teams divide their testing efforts. In this book, I propose a stacked layout to represent the different levels. The lower levels of this structure represent the tests aimed at verifying small pieces of software (called *units*). As we ascend in the stack, we find other tiers (e.g., *integration*, *system*, etc.) in which the SUT integrates more and more components.

![hosw 0105](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781098109998/files/assets/hosw_0105.png)

###### Figure 1-5. Stack representation of the different levels of testing

The lowest level of this stack is *unit testing*. At this level, we assess individual units of software. A unit is a particular observable element of behavior. For instance, units are typically methods or classes in object-oriented programming and functions in functional programming. Unit testing aims to verify that each unit behaves as expected. Automated unit tests usually run very fast since each test executes a small amount of code in isolation. To achieve this isolation, we can use *test doubles*, pieces of software that replace the dependent components of a given unit. For example, a popular type of test double in object-oriented programming is the *mock object*. A mock object mimics an actual object using some programmed behavior.

The next level in Figure 1-5 is *integration testing*. At this level, different units are composed to create composite components. Integration testing aims to assess the interaction between the involved units and expose defects in their interfaces.

Then, at the *system testing* and *end-to-end* (E2E) levels, we test the software system as a whole. We need to deploy the SUT and verify its high-level features to carry out these levels. The difference between system/end-to-end and integration testing is that the former involves all the system components and the final user (typically impersonated). In other words, system and end-to-end testing assess the SUT through the User Interface (UI). This UI can be graphical (GUI) or nongraphical (e.g., text-based or other types).

##### THE TEST PYRAMID

The *test pyramid* is a classical representation of the levels of testing. Mike Cohn first coined this concept in 2009. In his original conception, Cohn recommended a large number of unit tests as the basis of testing efforts. The following levels (e.g., integration tests) are less numerous in each stage but typically more expensive (in terms of development and maintenance effort) and slow (in terms of execution time). This proposal might be impractical for many projects because unit tests are not always from a comprehensive testing suite. For this reason, other authors define different shapes for the levels of testing, such as the *testing trophy* (in which the intermediate layer, i.e., the integration test, is the largest). Since the relevance of the different test categories can vary from one project to another, I use a basic stack structure to represent the different levels of testing.

Figure 1-6 illustrates the difference between system and end-to-end testing. As you can see, on the one hand, end-to-end testing involves the software system and its dependent subsystems (e.g., database or external services). On the other hand, system testing comprises only the software system, and these external dependencies are typically mocked.

![hosw 0106](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781098109998/files/assets/hosw_0106.png)

###### Figure 1-6. Component-based representation of the different levels of testing

*Acceptance testing* is the top tier of the presented stack. At this level, the final user participates in the testing process. The objective of acceptance testing is to decide whether the software system meets end-user expectations. As you can see in Figure 1-6, like end-to-end testing, acceptance testing validates the whole system and its dependencies. Therefore, acceptance tests also use the UI to carry out the SUT validation.

###### TIP

The primary purpose of Selenium WebDriver is to implement end-to-end tests. Nevertheless, we can use WebDriver to carry out system testing when mocking the backend calls made by the website under test. Moreover, we can use Selenium WebDriver in conjunction with a Behavior-Driven Development (BDD) tool to implement acceptance tests.

##### VERIFICATION AND VALIDATION

The down levels of the test stack we have seen (unit, integration, system, and end-to-end testing) belong to *development testing*. Development testing is a process carried out by the team that produces the software system (i.e., developers, testers, etc.) during the construction phase of the software development lifecycle. Development testing is a type of *verification* since we assess that the software meets its stated functional and nonfunctional requirements (i.e., its specification). Using the classical definition stated by Barry Boehm in 1984, verification allows answering the following question: “*Are we building the product right?*”

The top level of the test stack represented in Figure 1-5 (i.e., acceptance testing) belongs to *user testing* since it involves the final user in the testing process. Acceptance testing is a type of *validation* because its objective is to prove the software system meets end-user expectations. Validation is a more general process than verification since the system specification does not always reflect the user’s real wishes or needs. Thus, according to Boehm, validation allows answering the question “*Are we building the right product?*”

## Types of Testing

Depending on the strategy for designing test cases, we can implement different types of tests. The two principal types of testing are:

*Functional testing* (also known as behavioral or *closed-box testing*)

Evaluates the compliance of a piece of software with the expected behavior (i.e., its functional requirements).

*Structural testing* (also known as *clear-box testing*)

Determines if the program-code structure is faulty. To that aim, testers should know the internal logic of a piece of software.

The difference between these testing types is that functional tests are responsibility-based, while structural tests are implementation-based. Both types can be performed at any test level (unit, integration, system, end-to-end, or acceptance). Nevertheless, structural tests are commonly done at the unit or integration level since these levels enable more direct control of the code execution flow.

###### WARNING

*Black-box* and *white-box* testing are other names for functional and structural testing, respectively. Nevertheless, these designations are not recommended since the tech industry is trying to adopt more inclusive terms and use neutral terminology instead of potentially harmful language.

There are different flavors of functional testing. For example:

*UI testing* (known as *GUI testing* when the UI is graphical)

Evaluates if the visual elements of an application meet the expected functionality. Note that UI testing is different from the system and end-to-end testing levels since the former tests the interface itself, and the latter evaluates the whole system through the UI.

*Negative testing*

Evaluates the SUT under unexpected conditions (e.g., expected exceptions). This term is the counterpart of the regular functional testing (sometimes called *positive testing*), in which we assess if the SUT behaves as expected (i.e., its *happy path*).

*Cross-browser testing*

This is specific for web applications. It aims to verify the compatibility of websites and applications in different web browsers (types, versions, or operating systems).

A third miscellaneous testing type, *nonfunctional testing*, includes testing strategies that assess the quality attributes of a software system (i.e., its nonfunctional requirements). Common methods of nonfunctional testing include, but are not limited to:

*Performance testing*

Assesses different metrics of software systems, such as response time, stability, reliability, or scalability. The objective of performance testing is not finding bugs but finding system bottlenecks. There are two common subtypes of performance testing:

*Load testing*

Increases the usage on the system by simulating multiple concurrent users to verify if it can operate in the defined boundaries.

*Stress testing*

Exercises a system beyond its operational capacity to identify the actual limits at which the system breaks.

*Security testing*

Tries to evaluate security concerns, such as confidentiality (disclosure of information protection), authentication (ensuring the user identity), or authorization (determining user rights and privileges), among others.

*Usability testing*

Evaluates how user-friendly a software application is. This assessment is also called User eXperience (UX) testing. A subtype of usability testing is:

*A/B testing*

Compares different variations of the same application to determine which one is more effective for its end users.

*Accessibility testing*

Evaluates if a system is usable by people with disabilities.

###### TIP

We use Selenium WebDriver primarily to implement functional tests (i.e., interacting with a web application UI to assess the application behavior). It is unlikely to use WebDriver to implement structural tests. In addition, although it is not its principal usage, we can use WebDriver to implement nonfunctional tests, e.g., for load, security, accessibility, or localization (assessment of specific locale settings) testing.

## Testing Methodologies

The *software development lifecycle* is the set of activities, actions, and tasks required to create software systems in software engineering. The moment at which software engineers design and implement test cases in the overall development lifecycle depends on the specific development process (such as iterative, waterfall, or agile, to name a few). Two of the most relevant testing methodologies are:

Test Driven Development (TDD)

TDD is a methodology in which we design and implement tests before the actual software design and implementation. At the beginning of the 21st century, TDD became popular with the rise of *agile* software development methodologies, such as Extreme Programming (XP). In TDD, a developer first writes an (initially failing) automated test for a given feature. Then, the developer creates a piece of code to pass that test. Finally, the developer refactors the code to achieve or improve readability and maintainability.

Test Last Development (TLD)

TLD is a methodology in which we design and implement tests after implementing the SUT. This practice is typical in traditional software development processes, such as waterfall (sequential), incremental (multi-waterfall), spiral (risk-oriented multi-waterfall), or Rational Unified Process (RUP).

Another relevant testing methodology is *Behavior-Driven Development* (BDD). BDD is a testing practice derived from TDD, and consequently, we design tests at the early stages of the software development lifecycle in BDD. To that aim, conversations occur between the final user and the development team (typically with the project leader, manager, or analysts). These conversations formalize a common understanding of the desired behavior and the software system. As a result, we create acceptance tests in terms of one or more *scenarios* following a *Given-When-Then* structure:

Given

Initial context at the beginning of the scenario

When

Event that triggers the scenario

Then

Expected outcome

###### TIP

TLD is a common practice used to implement Selenium WebDriver. In other words, developers/testers do not implement a WebDriver test until the SUT is available. Nevertheless, different methodologies are also possible. For instance, BDD is a common approach when using WebDriver with Cucumber.

Closely related to the domain of testing methodologies, we find the concept of *Continuous Integration* (CI). CI is a software development practice where members of a software project build, test, and integrate their work continuously. Grady Booch first coined the term CI in 1991. Now it is a popular strategy to create software.

As Figure 1-7 shows, CI has three separate stages. First, we use a *source code repository*, a hosting facility to store and share the source code of a software project. We typically use a *version control system* (VCS) to manage this repository. A VCS is a tool that keeps track of the source code, who made each change, and when (sometimes called *patch*).

![hosw 0107](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781098109998/files/assets/hosw_0107.png)

###### Figure 1-7. CI generic process

*Git*, initially developed by Linus Torvalds, is the preferred VCS today. Other alternatives are a *concurrent versions system* (CVS) or Subversion (SVN). On top of Git, several *code hosting platforms* (such as GitHub, GitLab, or Bitbucket) provide collaborative cloud repository hosting services for developing, sharing, and maintaining software.

Developers synchronize a local repository (or simply, *repo*) copy in their local environments. Then, they do the coding work using that local copy, committing new changes to the remote repository (typically daily). The basic idea of CI is that every commit triggers the build and test of the software with the new changes. The test suite executed to assess that a patch does not break the build is called a *regression test*. A regression suite can contain tests of different types, including unit, integration, end-to-end, etc.

When the number of tests is too large for regression testing, we typically choose only a part of the relevant tests from the whole suite. There are different strategies to select these tests, for instance, *smoke testing* (i.e., tests that ensure the critical functionality) or *sanity testing* (i.e., tests that evaluate the basic functionality). Lastly, we can execute the complete suite as a scheduled task (typically nightly).

We need to use a server-side infrastructure called a *build server* to implement a CI pipeline. The build server usually reports a problem to the original developer when the regression tests fail.

Continuous Delivery (CD)

After CI, the build server deploys the release to a staging environment (i.e., a replica of a production environment for testing purposes) and executes the automated acceptance tests (if any).

Continuous Deployment

The build server deploys the software release to the production environment as the final step.

![hosw 0108](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781098109998/files/assets/hosw_0108.png)

###### Figure 1-8. Continuous Integration, Delivery, and Deployment pipeline

Close to CI, the term DevOps (development and operations) has gained momentum. DevOps is a software methodology that promotes communication and collaboration between different teams in a software project to develop and deliver software efficiently. These teams include developers, testers, QA (quality assurance), operations (infrastructure), etc.

## Test Automation Tools

We need to use some tooling to implement, execute, and control automated tests effectively. One of the most relevant categories for testing tools is the *unit testing framework*. The original framework in the unit testing family (also known as *xUnit*) is SmalltalkUnit (or SUnit). SUnit is a unit test framework for the Smalltalk language created by Kent Beck in 1999. Erich Gamma ported SUnit to Java, creating JUnit. Since then, JUnit has been very popular, inspiring other unit testing frameworks.

Setup

The test case initializes the SUT to exhibit the expected behavior.

Exercise

The test case interacts with the SUT. As a result, the test gets an outcome from the SUT.

Verify

The test case decides if the obtained outcome from the SUT is as expected. To that aim, the test contains one or more assertions. An *assertion* (or predicate) is a boolean-value function that checks if an expected condition is true. The execution of the assertions generates a test verdict (typically, pass or fail).

Teardown

The test case puts the SUT back into the initial state.

![hosw 0109](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781098109998/files/assets/hosw_0109.png)

###### Figure 1-9. Unit test generic structure

###### TIP

We can use unit testing frameworks in conjunction with other libraries or utilities to implement any test type.

The stages of setup and teardown are optional in a unit test case. Although it is not strictly mandatory, verifying is highly recommended. Even if unit testing frameworks include capabilities to implement assertions, it is common to incorporate third-party *assertions libraries*. These libraries aim to improve the test code’s readability by providing a rich set of fluent assertions. In addition, these libraries offer enhanced error messages to help testers understand the cause of a failure. 

As you can see in Figure 1-9, the SUT usually can query another component, named the *Depended-On Component* (DOC). In some cases (e.g., at the unit or system testing level), we might want to isolate the SUT from the DOC(s). We can find a wide variety of mock libraries to achieve this isolation.

The last category of testing tools we analyze in this section is BDD, a development process that creates acceptance tests. There are plenty of alternatives to implement this approach.

# Summary and Outlook

Selenium has come a long way since its inception in 2004. Many practitioners consider it the de facto standard solution to develop end-to-end tests for web applications, and it is used by thousands of projects worldwide. In this chapter, you have seen the foundations of the Selenium project (made up of WebDriver, Grid, and IDE). In addition, Selenium has a rich ecosystem and active community. WebDriver is the heart of the Selenium project, and it is a library that provides an API to control different web browsers (e.g., Chrome, Firefox, Edge, etc.) programmatically.

In the next chapter, you discover how to set up a Java project using Maven or Gradle as build tools. This project will contain end-to-end tests for web applications using JUnit and TestNG as the unit testing frameworks and calls to the Selenium WebDriver API. In addition, you will learn how to control different web browsers (e.g., Chrome, Firefox, or Edge) with a basic test case (the Selenium WebDriver’s version of the classic *hello world*).


