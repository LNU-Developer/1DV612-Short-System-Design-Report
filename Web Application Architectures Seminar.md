# Task 1

- What is a web application?
  - A web application (or web app) is application software that runs on a web server, unlike computer-based software programs that are run locally on the operating system (OS) of the device
- What does web application architecture mean?
  - The web application architecture describes the interactions between applications, databases, and middleware systems on the web. It ensures that multiple applications work simultaneously.
- What does a web application architecture include?
  - With any typical web application, there are two different codes (sub-programs) running side-by-side. These are:
    - Client-side Code - The code that is in the browser and responds to some user input
    - Server-side Code - The code that is on the server and responds to the HTTP requests
  - UI/UX Web Application Components – This includes activity logs, dashboards, notifications, settings, statistics, etc. These components have nothing to do with the operation of a web application architecture. Instead, they are part of the interface layout plan of a web app
    - Structural Components – The two major structural components of a web app are client and server sides.
    - Client Component - The client component is developed in CSS, HTML, and JS. As it exists within the user’s web browser, there is no need for operating system or device-related adjustments. The client component is a representation of a web application’s functionality that the end-user interacts with.
    - Server Component - The server component can be build using one or a combination of several programming languages and frameworks, including Java, .Net, NodeJS, PHP, Python, and Ruby on Rails. The server component has at least two parts; app logic and database. The former is the main control center of the web application while the latter is where all the persistent data is stored.
- How have architectures for web applications developed over time?
  -
- What architectural (anti) design patterns are there for web applications?
  - Premature Optimization - pay attention to small efficiencies and optimize for them too early in the development process, before we exactly know what we want to do.
  - Reinventing the Wheel - do everything by ourselves and write everything from scratch
  - Dependency Hell - use of too many third party libraries that rely on specific versions of other libraries
  - Spaghetti Code - describes an application that is hard to debug or modify because of the lack of a proper architecture.
  - Programming by Permutation - when we try to find a solution for a problem by successively experimenting with small modifications, testing and assessing them one by one, and finally implementing the one that works at first.
  - Copy and Paste Programming - occurs when we don’t follow the Don’t Repeat Yourself (DRY) coding principle
  - Cargo-Cult Programming - use frameworks, libraries, solutions, design patterns, etc. that worked well for others, without understanding why we do so, or how said technologies exactly work.
  - Lava Flow - deal with code that has redundant or low-quality parts that seem to be integral to the program
  - Hard Coding - store configuration or input data
  - Soft Coding - things that should be in the source code are put into external sources

  - Which is considered the most common architectural design pattern?
    - Layered (n-tier) architecture (MVC)
  - What does a big ball of mud (BBoM) mean?
    - A BIG BALL OF MUD is haphazardly structured, sprawling, sloppy, duct-tape and bailing wire, spaghetti code jungle.

# Task 2
Please see the diagram artifacts attached