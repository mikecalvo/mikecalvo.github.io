footer: © Citronella Software Ltd 2015
slidenumbers: true

# Tools
## Mike Calvo
### mike@citronellasoftware.com

---

# Overview
- Java
- Groovy/Grails
- IDE
- Source Code Control
- Tomcat

---

# Java SDK
- Groovy is a JVM (Java Virtual Machine) language
- Java SDK required
- Download available from Oracle
- Look for Java SE Downloads
- Be sure to set your JAVA_HOME environment variable
- Add $JAVA_HOME/bin to your path

^ Google Java SE Downloads and show them the links

---

# GVM
- [http://gvmtool.net/](Groovy Environment Manager)
- Helps manage version of Groovy-based tools
- Groovy
- Grails
- Gradle
- Install GVM:
  `$ curl -s get.gvmtool.net | bash`

---

# Use GVM
- Install Groovy:
  `$ gvm install groovy`
- Install Grails:
`$ gvm install grails`
- Confirm:
  `$ gvm current`
  `$ groovy -version`
  `$ grails -version`

^ go out to command line and show gvm in action

---

# Groovy
- Groovy install not required for grails
- Groovy Console
- Handy interactive debugging tool
`$ groovyConsole`

^ show Groovy Console in action

---

# Grails
- Use the grails command line application to:
- Create a new project
- Build your project
- Run your project tests
- Run/Debug your project

---

# IDE Options
- Strongly recommend IntelliJ IDEA
- Best Grails support
- Normally $249 developer license
- Free license
https://www.jetbrains.com/estore/students/academic

---

# Other options
- SpringSource Tool Suite (Eclipse)
- Net Beans
- Plain 'Ol Text Editor
- Atom
- Sublime
- TextMate

---

# Source Code Control
- Working in a team you will need to share code with each other
- This course requires submission of projections using Git
- Installing Git
- Mac: Install Xcode Command Line Tools
- Windows: Download Windows Git installer
- Linux: `$ yum inbstall git`

---

# Git Tools
- Command line
- IntelliJ
- Tower (Mac)
- SourceTree (Mac/Windows)

---

# Git Recommendations
- Get comfortable with the command line
- Commit often
- Work in branches
- Tag your releases/submissions

---

# Free Git Hosting Options
- GitHub
- Bitbucket
- CloudForge
- Assembla
- Kiln
- code.google.com

---

# Tomcat
- Grails comes with an embedded app server for deployment
- Optionally install Tomcat locally
- Confirm your installation
- Download and unzip distribution
- Start Tomcat:
  - $ ./startup.sh

^ Show this working locally

---

# Recommendations for this week
1. Get your local development environment setup today
2. Select your git host and create a project
