EXPERIMENT 1: Introduction to Git and Local Repository Management
Step-by-Step
Step 1 — Install Git
Go to git-scm.com → download for your OS → install with default settings
Open VS Code terminal (Ctrl+`) and verify:
git --version
You should see something like git version 2.4x.x
Step 2 — Configure Git with your identity
Git needs to know who you are so it can attach your name to every commit:
git config --global user.name "Your Name"
git config --global user.email "youremail@gmail.com"
Verify it saved:
git config --list
Step 3 — Create a project folder and initialize Git
mkdir MyFirstRepo
cd MyFirstRepo
git init
git init creates a hidden .git folder inside your project. This is where Git stores all history. Your folder is now a Git repository.
Verify:
ls -a
You'll see a .git folder.
Step 4 — Create some files
echo "Hello Git" > hello.txt
echo "This is my project" > readme.txt
Or create them manually in VS Code.
Step 5 — Check the status
git status
Git shows both files as Untracked files (in red). This means Git can see them but is not tracking/saving them yet.
Step 6 — Add files to staging area
git add hello.txt
git status
Now hello.txt is Changes to be committed (in green). It's in the staging area — ready to be committed.
Add all files at once:
git add .
The . means "add everything in the current folder".
Step 7 — Commit the files
git commit -m "Initial commit - added hello and readme files"
-m = message. Always write a clear message describing what you changed. This creates your first permanent snapshot.
Step 8 — Make changes and commit again
Open hello.txt in VS Code and change the text to Hello Git - Updated. Then:
git status            # shows hello.txt as modified
git add hello.txt
git commit -m "Updated greeting in hello.txt"
Step 9 — View commit history
git log
Shows all commits with: commit hash (unique ID), author, date, message.
For a compact view:
git log --oneline
Shows each commit on one line: abc1234 Initial commit - added hello and readme files
Step 10 — See what changed in a commit
git show <commit-hash>
Or see difference between current file and last commit:
git diff hello.txt
Green lines = added, Red lines = removed.
Step 11 — Go back to a previous version
git checkout <commit-hash> -- hello.txt
This restores hello.txt to how it was in that specific commit.

EXPERIMENT 2: Working with Git Branches
What is a Branch?
By default, all your commits go on a branch called main (or master). A branch is like a separate timeline of your project.
Why branches? Imagine you're adding a new feature to your app. You don't want to break the working code while you experiment. So you create a new branch, work on it, and only merge it back to main when it's ready and tested.
Real-world analogy: Think of a tree. The trunk is main. You grow a new branch to experiment. If the experiment works, you merge the branch back into the trunk. If it fails, you just delete the branch — trunk is untouched.
Step-by-Step
Step 1 — Check current branch
git branch
You'll see * main — the * shows your current branch.
Step 2 — Create a new feature branch
git branch feature-login
This creates a new branch called feature-login but you're still on main.
Switch to it:
git checkout feature-login
Or create and switch in one command:
git checkout -b feature-login
Verify:
git branch
Now * feature-login has the asterisk.
Step 3 — Make changes on the feature branch
Create a new file login.txt:
echo "Login feature code here" > login.txt
git add login.txt
git commit -m "Added login feature"
Make another change:
echo "Validation added" >> login.txt
git add login.txt
git commit -m "Added validation to login"
Step 4 — Switch back to main and verify isolation
git checkout main
ls
Notice login.txt is NOT here! Changes on feature-login are isolated from main. This is the power of branches.
Step 5 — Merge the feature branch into main
git merge feature-login
Git takes all commits from feature-login and applies them to main. Now login.txt appears in main.
Step 6 — View branch history
git log --oneline --graph --all
Shows a visual tree of all branches and commits.
Step 7 — Delete the feature branch (after merge)
git branch -d feature-login
Handling Merge Conflicts
A conflict happens when the same line in the same file was changed differently in two branches.
Step 1 — Create a conflict scenario
# On main branch
echo "Line from main" > conflict.txt
git add conflict.txt
git commit -m "Added conflict.txt on main"

# Create and switch to new branch
git checkout -b feature-b

# Modify the same file differently
echo "Line from feature-b" > conflict.txt
git add conflict.txt
git commit -m "Modified conflict.txt on feature-b"

# Switch back to main and try to merge
git checkout main
git merge feature-b
Git will say CONFLICT - Automatic merge failed.
Step 2 — Open the conflicted file
Open conflict.txt in VS Code. You'll see:
<<<<<<< HEAD
Line from main
=======
Line from feature-b
>>>>>>> feature-b

Everything between <<<<<<< HEAD and ======= is from main
Everything between ======= and >>>>>>> feature-b is from the other branch

Step 3 — Resolve the conflict
Edit the file to keep what you want. For example, keep both:
Line from main
Line from feature-b
Delete all the <<<<<<<, =======, >>>>>>> markers.
Step 4 — Mark as resolved and commit
git add conflict.txt
git commit -m "Resolved merge conflict in conflict.txt"
VS Code also has a built-in conflict resolver — when you open a conflicted file, it shows buttons: Accept Current, Accept Incoming, Accept Both. Click the one you want.

EXPERIMENT 3: GitHub Repository Creation and Push
What is GitHub?
Git is a tool that runs on your computer (local). GitHub is a website that hosts your Git repositories in the cloud (remote). It lets you:

Back up your code online
Share code with others
Collaborate on projects
Access your code from any machine

Git ≠ GitHub. Git is the tool. GitHub is a hosting service that uses Git.
Step-by-Step
Step 1 — Create GitHub account

Go to github.com → Sign up (free)
Verify your email

Step 2 — Create a new repository on GitHub

Click the + icon (top right) → New repository
Repository name: MyFirstGitHubRepo
Description: My first GitHub repository
Set to Public
Do NOT check "Initialize with README" (we'll push our own files)
Click Create repository

GitHub shows you a page with instructions. Keep it open.
Step 3 — Link your local repo to GitHub
In your VS Code terminal (inside your local project from Experiment 1):
git remote add origin https://github.com/yourusername/MyFirstGitHubRepo.git
origin is just a nickname for the GitHub URL. You can use any name but origin is the standard convention.
Verify:
git remote -v
Shows the URL linked to origin.
Step 4 — Push your local commits to GitHub
git push -u origin main

-u = sets origin main as the default upstream (so next time you can just type git push)
origin = remote nickname
main = branch name

GitHub may ask for your username and password. For password, use a Personal Access Token (not your actual password):

GitHub → Settings → Developer settings → Personal access tokens → Generate new token
Select repo scope → Generate → copy the token
Use this token as your password when Git asks

Step 5 — Verify on GitHub
Refresh your GitHub repository page. You'll see all your files uploaded!
Step 6 — Clone a repository
Cloning means downloading a complete copy of a GitHub repository to your machine.
First, copy the repository URL from GitHub (green Code button → copy HTTPS URL).
On any machine:
cd Desktop
git clone https://github.com/yourusername/MyFirstGitHubRepo.git
cd MyFirstGitHubRepo
ls
You have a complete copy with full history!
Step 7 — Make a change, push, and pull
Make a change to a file → commit → push:
echo "New line added" >> readme.txt
git add .
git commit -m "Updated readme"
git push
On another machine (or the cloned copy), pull the latest changes:
git pull

EXPERIMENT 4: GitHub Collaboration Using Pull Requests
What is a Pull Request (PR)?
In a team, you don't directly push to the main branch. Instead:

You fork the project (your own copy)
Make changes in a branch
Submit a Pull Request — asking the team: "Please review my changes and pull them into the main project"
Team reviews, comments, approves/rejects
If approved, changes are merged

This is how ALL open-source projects and most companies work.
Key Terms
TermMeaningForkYour personal copy of someone else's repositoryPull RequestA request to merge your changes into the original repoReviewTeam members check your code before it's mergedMergeAccepting the pull request — changes are added to main
Step-by-Step
Step 1 — Fork a repository

Go to any GitHub repository (or use a classmate's repo)
Click Fork (top right) → Create fork
GitHub creates yourusername/repo-name — your own complete copy

Step 2 — Clone YOUR fork
git clone https://github.com/yourusername/forked-repo.git
cd forked-repo
Step 3 — Create a feature branch
git checkout -b my-feature-branch
Always work on a branch, never directly on main.
Step 4 — Make your changes
Create or edit a file:
echo "My contribution" > my-feature.txt
git add my-feature.txt
git commit -m "Added my feature contribution"
Step 5 — Push the branch to YOUR fork
git push origin my-feature-branch
Step 6 — Create a Pull Request on GitHub

Go to your fork on GitHub
You'll see a yellow banner: my-feature-branch had recent pushes → click Compare & pull request
Fill in:

Title: Added my feature
Description: This PR adds a new feature file with my contribution


Make sure the base repo and branch are correct (the original repo's main)
Click Create pull request

Step 7 — Review process

The original repo owner goes to Pull requests tab → sees your PR
They can:

Leave comments on specific lines
Request changes
Approve the PR


You can respond to comments, push more commits to update the PR

Step 8 — Merge the Pull Request

Once approved, click Merge pull request → Confirm merge
Your changes are now in the original repository!
Optionally delete the branch after merge

Step 9 — Keep your fork updated
After the merge, your fork's main is behind the original. Sync it:
git remote add upstream https://github.com/originalowner/repo.git
git fetch upstream
git checkout main
git merge upstream/main
git push origin main

EXPERIMENT 5: Git Tagging and Release Creation
What is a Tag?
A tag is a permanent label on a specific commit. While branch names move forward with every new commit, a tag always points to the same commit forever.
Why tags? When you release version 1.0 of your software, you tag that specific commit as v1.0. Six months later when there are 200 more commits, you can still instantly go back to exactly what was in version 1.0.
Two Types of Tags

Lightweight tag: Just a name pointing to a commit. Like a bookmark.
Annotated tag: A full object with tagger name, email, date, and message. Used for releases. Always use annotated tags for real releases.

Step-by-Step
Step 1 — Make sure you have some commits
From your previous experiments, you should already have commits. Check:
git log --oneline
Step 2 — Create a lightweight tag
git tag v0.1
Just a quick label. No extra info.
Step 3 — Create an annotated tag (proper release tag)
git tag -a v1.0 -m "Version 1.0 - First stable release"

-a = annotated
v1.0 = tag name (convention: use v prefix)
-m = message describing this release

Step 4 — View all tags
git tag
Step 5 — View tag details
git show v1.0
Shows tagger info, date, message, and the commit it points to.
Step 6 — Tag an older commit
You can tag any past commit:
git log --oneline        # find the commit hash
git tag -a v0.5 <hash> -m "Version 0.5 - Beta release"
Step 7 — Push tags to GitHub
By default, git push does NOT push tags. You must push them separately:
git push origin v1.0        # push one specific tag
git push origin --tags      # push ALL tags
Step 8 — Create a GitHub Release

Go to your GitHub repo → Releases (right sidebar) → Create a new release
Click Choose a tag → select v1.0
Release title: Version 1.0 - First Stable Release
Description: Write release notes (what's new, what's fixed, known issues)
You can attach files (compiled .jar, .exe, documentation)
Click Publish release

GitHub Release is more than just a tag — it has a nice page with download links.
Step 9 — Delete a tag (if needed)
git tag -d v0.1               # delete locally
git push origin --delete v0.1  # delete from GitHub
Step 10 — Checkout code at a specific tag
git checkout v1.0
Your code goes back to exactly how it was at v1.0. To go back to latest:
git checkout main

EXPERIMENT 6: Creation and Build of a Java Project Using Apache Maven
What is Maven?
Maven is a build automation and dependency management tool for Java. It does three main things:

Dependency management: You declare what libraries you need in pom.xml, Maven downloads them automatically
Build automation: One command (mvn package) compiles your code, runs tests, and creates a .jar file
Standardized structure: Every Maven project has the same folder structure, so any Java developer can understand it immediately

What is a JAR file?
JAR (Java ARchive) is a packaged version of your compiled Java application — like a .zip file containing all your .class files. You can run it with java -jar app.jar.
Maven Lifecycle Phases
Maven has a sequence of phases. When you run a later phase, all earlier phases run automatically:
validate → compile → test → package → verify → install → deploy
PhaseWhat it doesvalidateChecks pom.xml is correctcompileCompiles .java files to .class filestestRuns unit testspackageCreates the .jar fileinstallCopies .jar to local Maven repository (~/.m2)deployUploads .jar to a remote repository
What is pom.xml?
Project Object Model — XML file that defines:

groupId: Your organization (like a package name, e.g., com.yourname)
artifactId: Your project name (e.g., sample-app)
version: Current version (e.g., 1.0-SNAPSHOT)
dependencies: Libraries your project needs
plugins: Build tools

SNAPSHOT means it's still in development. When you release, you change it to 1.0.
Step-by-Step
Step 1 — Install Maven

Download from maven.apache.org → Binary zip archive
Extract to C:\maven (Windows) or /opt/maven (Linux/Mac)
Add bin folder to PATH:

Windows: Environment Variables → System Variables → Path → Edit → New → C:\maven\bin
Linux/Mac: Add export PATH=/opt/maven/bin:$PATH to ~/.bashrc or ~/.zshrc


Open NEW terminal and verify:

mvn -version
Step 2 — Create Maven project
In VS Code terminal:
mvn archetype:generate -DgroupId=com.lab -DartifactId=sample-app -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
This creates a complete Maven project automatically. Enter the folder:
cd sample-app
Step 3 — Understand the project structure
sample-app/
├── pom.xml
└── src/
    ├── main/
    │   └── java/
    │       └── com/lab/
    │           └── App.java        ← your main application code
    └── test/
        └── java/
            └── com/lab/
                └── AppTest.java    ← your test code
Step 4 — View the generated pom.xml
Open pom.xml. Key parts:
xml<groupId>com.lab</groupId>
<artifactId>sample-app</artifactId>
<version>1.0-SNAPSHOT</version>
This is exactly what the experiment requires: artifactId:sample-app version 1.0-SNAPSHOT.
Step 5 — View the generated App.java
javapackage com.lab;

public class App {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
Step 6 — Add a plugin to make executable JAR
In pom.xml, inside <build><plugins>, add:
xml<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <version>3.2.0</version>
    <configuration>
        <archive>
            <manifest>
                <mainClass>com.lab.App</mainClass>
            </manifest>
        </archive>
    </configuration>
</plugin>
This tells Maven which class has the main() method when packaging the JAR.
Step 7 — Run each lifecycle phase separately
mvn validate        # validates pom.xml
mvn compile         # compiles .java files → creates target/classes/
mvn test            # runs AppTest.java tests
mvn package         # creates the .jar file in target/
Or just run mvn package — it runs all phases up to package automatically.
Step 8 — Find and run the JAR
ls target/
You'll see sample-app-1.0-SNAPSHOT.jar. Run it:
java -jar target/sample-app-1.0-SNAPSHOT.jar
Output: Hello World!
Step 9 — Install to local repository
mvn install
Copies the JAR to ~/.m2/repository/com/lab/sample-app/1.0-SNAPSHOT/. Other local projects can now use this as a dependency.
Step 10 — Clean build artifacts
mvn clean
Deletes the target/ folder. Useful when you want a fresh build.
Combined:
mvn clean package

EXPERIMENT 7: Jenkins Freestyle Job
What is Jenkins?
Jenkins is an open-source automation server. It automates repetitive tasks in software development, mainly:

Building code automatically when pushed
Running tests automatically
Archiving build outputs (JAR files, reports)
Notifying team of failures

What is a Freestyle Job?
The simplest type of Jenkins job. You configure it through a GUI (no scripting needed). It can: pull code from GitHub, run shell commands, archive output files.
Step-by-Step
Step 1 — Install Jenkins

Download jenkins.war from jenkins.io (LTS version)
Run it:

java -jar jenkins.war

Open browser → http://localhost:8080
Find the initial admin password:

cat ~/.jenkins/secrets/initialAdminPassword

Paste it → Install suggested plugins → Create admin user (remember this username/password!)

Step 2 — Install Maven Integration Plugin

Manage Jenkins → Plugins → Available plugins
Search Maven Integration → Install
Search Git plugin → Install (usually already included)
Restart Jenkins

Step 3 — Configure Maven in Jenkins

Manage Jenkins → Tools
Scroll to Maven installations → Add Maven
Name: Maven3
Check Install automatically → select latest version
Save

Step 4 — Configure JDK in Jenkins

Manage Jenkins → Tools
JDK installations → Add JDK
Name: Java17
Either provide JAVA_HOME path or check Install automatically
Save

Step 5 — Make sure your Maven project is on GitHub
Push your sample-app from Experiment 6 to GitHub (follow Experiment 3 steps). Your repo should contain pom.xml and src/ folder.
Step 6 — Create a Freestyle Job

Jenkins dashboard → New Item
Item name: MavenBuildJob
Select Freestyle project
Click OK

Step 7 — Configure Source Code Management

Under Source Code Management, select Git
Repository URL: https://github.com/yourusername/sample-app.git
Credentials: If public repo, leave as None. If private, add credentials.
Branch: */main

Step 8 — Configure Build Steps

Under Build Steps → Add build step → Invoke top-level Maven targets
Maven Version: Maven3
Goals: clean package

Step 9 — Archive the built artifact

Under Post-build Actions → Add post-build action → Archive the artifacts
Files to archive: target/*.jar
This saves the JAR file so you can download it from Jenkins after each build

Step 10 — Save and Build

Click Save
Click Build Now (left sidebar)
A build appears under Build History — click it
Click Console Output to watch Jenkins:

Clone your GitHub repo
Run mvn clean package
Show BUILD SUCCESS
Archive the JAR



Step 11 — Download the archived artifact

After successful build, go to the build page
You'll see Build Artifacts section with a link to download the JAR file


EXPERIMENT 8: Jenkins Build Triggers
What is a Build Trigger?
Currently, Jenkins only builds when you manually click "Build Now". Build triggers make Jenkins build automatically when something happens:

SCM Polling: Jenkins checks GitHub every X minutes for new commits
GitHub Webhook: GitHub instantly notifies Jenkins when you push code (better, faster)

SCM Polling vs Webhook
FeatureSCM PollingWebhookHow it worksJenkins asks GitHub "any changes?" every N minutesGitHub tells Jenkins immediately when push happensSpeedDelayed (depends on poll interval)InstantEfficiencyWastes resources (checks even when nothing changed)Efficient (only triggers when needed)SetupEasy, no network config neededRequires Jenkins to be accessible from internet
Step-by-Step: SCM Polling
Step 1 — Open your existing Freestyle Job

Jenkins dashboard → click MavenBuildJob → Configure

Step 2 — Enable SCM Polling

Under Build Triggers → check Poll SCM
In the Schedule box, enter a cron expression:

H/2 * * * *
This means: every 2 minutes, check GitHub for new commits.
Cron format: minute hour day month day-of-week

* = every
H/2 = every 2 minutes
H * * * * = every hour
Click Save

Step 3 — Test polling

Make a change in your project, commit, push to GitHub
Wait up to 2 minutes
Jenkins will automatically detect the change and start a new build
You'll see a new entry in Build History

Step-by-Step: GitHub Webhook
Step 1 — Make Jenkins accessible from internet
If Jenkins is on localhost, GitHub can't reach it. Options:

Use ngrok for testing:

ngrok http 8080
Ngrok gives you a public URL like https://abc123.ngrok.io that forwards to your localhost:8080.
Step 2 — Configure Jenkins to accept webhooks

Manage Jenkins → Configure System
Under GitHub, click Add GitHub Server
Name: GitHub
Click Save

Step 3 — Set up webhook in GitHub

Go to your GitHub repo → Settings → Webhooks → Add webhook
Payload URL: http://your-jenkins-url:8080/github-webhook/
(or https://abc123.ngrok.io/github-webhook/ if using ngrok)
Content type: application/json
Which events: Just the push event
Check Active
Click Add webhook

GitHub will send a test ping. If Jenkins receives it, you'll see a green checkmark.
Step 4 — Configure the Jenkins Job

Open your job → Configure
Under Build Triggers → check GitHub hook trigger for GITScm polling
Save

Step 5 — Test webhook

Make a change → commit → push to GitHub
Instantly (within seconds) → Jenkins starts a new build automatically!


EXPERIMENT 9: Jenkins Declarative Pipeline
What is a Pipeline?
In Experiment 7, you configured the job through Jenkins' GUI. A Pipeline defines the entire build process as code in a file called Jenkinsfile. This file lives in your repository alongside your code.
Why Pipeline as Code?

Version controlled (changes to pipeline are tracked in Git)
Reviewable (team can review pipeline changes)
Reproducible (same pipeline runs everywhere)
Visible (everyone can see how the build works)

Declarative Pipeline Syntax
A Declarative Pipeline has this structure:
groovypipeline {
    agent any          // where to run (any available Jenkins agent)

    stages {           // the work to do, broken into stages
        stage('Stage Name') {
            steps {    // actual commands to run
                // commands here
            }
        }
    }

    post {             // what to do after pipeline finishes
        success { }
        failure { }
    }
}
Key Sections Explained
SectionPurposeagent anyRun on any available Jenkins workerstagesContainer for all your stagesstage('name')A named step in your pipeline (shows as a block in the UI)stepsThe actual commands inside a stagesh 'command'Run a shell commandecho 'text'Print text to consolepostRuns after all stages — success/failure/always
Step-by-Step
Step 1 — Create Jenkinsfile in your project
Open your sample-app project in VS Code. Create a new file in the root called Jenkinsfile (no extension):
groovypipeline {
    agent any

    tools {
        maven 'Maven3'    // use the Maven we configured in Jenkins
        jdk 'Java17'      // use Java 17
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                checkout scm    // checks out the code from configured Git repo
            }
        }

        stage('Compile') {
            steps {
                echo 'Compiling the project...'
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging into JAR...'
                sh 'mvn package -DskipTests'
            }
        }

        stage('Archive') {
            steps {
                echo 'Archiving artifacts...'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully! JAR is ready.'
        }
        failure {
            echo 'Pipeline failed. Check the logs above.'
        }
        always {
            echo 'Pipeline finished. Cleaning up...'
            cleanWs()    // clean workspace after build
        }
    }
}
Step 2 — Commit and push the Jenkinsfile
git add Jenkinsfile
git commit -m "Added Jenkinsfile for Jenkins pipeline"
git push
Step 3 — Create a Pipeline Job in Jenkins

New Item → name: MavenPipeline → select Pipeline → OK

Step 4 — Configure the Pipeline

Under Pipeline section:

Definition: Pipeline script from SCM
SCM: Git
Repository URL: your GitHub repo URL
Branch: */main
Script Path: Jenkinsfile (this is where Jenkins looks for the file)


Save

Step 5 — Run the Pipeline

Click Build Now
Click the build number → you see the Stage View — each stage shown as a block

Step 6 — Understanding the output
Each stage shows:

Time taken
Green = passed, Red = failed
Click any stage block → see logs for just that stage


EXPERIMENT 10: Jenkins Pipeline Visualization and Logs
What is this experiment about?
Now that you have a working pipeline, you need to know how to READ it, UNDERSTAND it, and DEBUG it when something goes wrong. This is a critical DevOps skill.
What to Look At
Stage View
The Stage View (on the pipeline job page) shows all stages as columns. Each row is a build. Green = pass, Red = fail, Gray = not run. You can see at a glance which stage failed.
Console Output
The full log of everything that happened during the build — every command run, every output line, every error message.
Blue Ocean View
A more visual, modern interface for viewing pipelines.
Step-by-Step
Step 1 — View the Stage View

Go to your MavenPipeline job
You see a table with stages as columns and builds as rows
Hover over any cell to see timing and status

Step 2 — View Console Output

Click on a build number (e.g., #3)
Click Console Output
Read through the full log:

[Pipeline] stage → Jenkins entering a stage
+ mvn compile → the shell command being run
BUILD SUCCESS → Maven succeeded
ERROR or Exception → something failed



Step 3 — Introduce a deliberate failure to practice debugging
Open Jenkinsfile in VS Code. Change one stage to have a wrong command:
groovystage('Compile') {
    steps {
        sh 'mvn compileXXX'    // this is wrong - will fail
    }
}
Commit and push. Jenkins builds → Stage View shows red on Compile stage.
Step 4 — Analyze the failure

Click the red build → Console Output
Scroll to where it failed. You'll see:

[Pipeline] sh
+ mvn compileXXX
[ERROR] Unknown lifecycle phase "compileXXX"
[ERROR] BUILD FAILURE
This tells you exactly what went wrong and on which line.
Step 5 — View logs per stage

Click Build #X → look at the left sidebar for stage logs
Or in Blue Ocean view: click each stage block for its specific log

Step 6 — Install and use Blue Ocean

Manage Jenkins → Plugins → Available → search Blue Ocean → Install
After install, go to Jenkins dashboard → click Open Blue Ocean (left sidebar)
Blue Ocean shows a beautiful visual pipeline with colored stages
Click any stage to expand its logs
Failed stages show the error highlighted in red

Step 7 — Fix the issue and rebuild

Fix the Jenkinsfile (remove the XXX)
Commit and push
Jenkins auto-builds (if webhook configured) or click Build Now
All stages green again

Step 8 — Add JUnit test result publishing to Pipeline
Update your Jenkinsfile's Test stage:
groovystage('Test') {
    steps {
        sh 'mvn test'
    }
    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}
Now after each build, Jenkins shows a Test Result link with pass/fail counts for each JUnit test.
Step 9 — View test trends

Go to your pipeline job page
You'll see a Test Result Trend graph showing pass/fail counts over time
If tests start failing, you can spot the trend immediately

EXPERIMENT 11: Writing Unit Tests using JUnit in VS Code
What is Unit Testing?
Unit testing means testing small individual pieces (units) of your code to make sure each piece works correctly. For example, if you write a function that adds two numbers, you write a test that checks: "does add(2,3) actually give me 5?" If yes → test passes. If no → test fails and you know something is broken.
What is JUnit?
JUnit is the most popular testing framework for Java. It gives you:

@Test annotation → marks a method as a test
assertEquals(expected, actual) → checks if two values are equal
assertTrue(condition) → checks if something is true
A test runner that shows green (pass) or red (fail)

Step-by-Step Setup
Step 1 — Install Java JDK

Go to adoptium.net, download JDK 17, install it
Open VS Code terminal (Ctrl+`) and type:

java -version
You should see something like openjdk 17.0.x
Step 2 — Install VS Code Extension

Press Ctrl+Shift+X in VS Code
Search for Extension Pack for Java by Microsoft
Install it. This gives you Java support, test runner, everything.

Step 3 — Create Project

Press Ctrl+Shift+P → type Java: Create Java Project
Select No build tools
Choose a folder, name it JUnitLab
VS Code creates a src/ folder automatically

Step 4 — Add JUnit JAR files

Download two files from the internet:

junit-4.13.2.jar
hamcrest-core-1.3.jar


Create a lib/ folder inside your project and paste both files there
In VS Code, look at the bottom-left panel called Java Projects
Expand your project → right-click Referenced Libraries → Add Jar Libraries
Select both jar files

Step 5 — Write Calculator.java
Create src/Calculator.java:
javapublic class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
    public int subtract(int a, int b) {
        return a - b;
    }
    public int multiply(int a, int b) {
        return a * b;
    }
}
Step 6 — Write CalculatorTest.java
Create src/CalculatorTest.java:
javaimport org.junit.Test;
import org.junit.Before;
import static org.junit.Assert.*;

public class CalculatorTest {

    Calculator calc;

    @Before
    public void setup() {
        calc = new Calculator(); // runs before every test
    }

    @Test
    public void testAdd() {
        assertEquals(5, calc.add(2, 3));   // expected=5, actual=add(2,3)
    }

    @Test
    public void testSubtract() {
        assertEquals(1, calc.subtract(3, 2));
    }

    @Test
    public void testMultiply() {
        assertEquals(6, calc.multiply(2, 3));
    }
}
Step 7 — Run Tests

You'll see green ▶ icons next to each @Test method
Click the ▶ next to the class name to run all tests
The Testing panel (beaker icon on left sidebar) shows results
Green checkmark = pass, Red X = fail


EXPERIMENT 12: Running JUnit Tests with Maven
What is Maven?
Maven is a build automation tool. Without Maven, you manually download JAR files and add them to your project. With Maven, you just write the dependency name in a file called pom.xml and Maven automatically downloads everything from the internet. It also gives you commands like mvn test to compile and run tests in one shot.
What is pom.xml?
pom.xml stands for Project Object Model. It's the heart of a Maven project — it lists your project name, Java version, and all dependencies (libraries) your project needs.
Step-by-Step
Step 1 — Install Maven

Go to maven.apache.org → download binary zip → extract it (e.g., to C:\maven)
Add C:\maven\bin to your system PATH:

Windows: Search "Environment Variables" → System Variables → Path → Edit → New → paste the path


Verify in VS Code terminal:

mvn -version
Step 2 — Create Maven Project in VS Code

Press Ctrl+Shift+P → Java: Create Java Project → select Maven
Choose maven-archetype-quickstart
Set groupId: com.lab
Set artifactId: JUnitMaven
VS Code generates this structure:

JUnitMaven/
├── pom.xml
└── src/
    ├── main/java/com/lab/App.java     ← your actual code goes here
    └── test/java/com/lab/AppTest.java ← your tests go here
Step 3 — Update pom.xml
Open pom.xml. Make sure this dependency is inside <dependencies>:
xml<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.2</version>
    <scope>test</scope>
</dependency>
<scope>test</scope> means this library is only used during testing, not in the final application.
Step 4 — Add your Calculator class
Replace content of src/main/java/com/lab/App.java with Calculator.java code (or create a new file in that folder).
Step 5 — Add your test class
Replace src/test/java/com/lab/AppTest.java with your CalculatorTest.java code. Make sure the package declaration at the top says package com.lab;.
Step 6 — Run tests
mvn test
Maven will:

Download JUnit from the internet (first time only)
Compile your code
Run all tests
Print BUILD SUCCESS or BUILD FAILURE

Step 7 — View Reports
After running, go to target/surefire-reports/. Open the .txt file — it shows each test method, whether it passed or failed, and how long it took.

EXPERIMENT 13: Integrating JUnit with Jenkins
What is Jenkins?
Jenkins is a Continuous Integration (CI) server. Normally, a developer writes code, pushes it to GitHub, and then manually runs tests. Jenkins automates this — when you push code, Jenkins automatically pulls it, runs your Maven tests, and shows a report. This is called CI (Continuous Integration) — you continuously integrate and test code.
What is a CI server good for?
Imagine a team of 10 developers all pushing code to the same project. Jenkins makes sure nobody breaks the project by running all tests automatically after every push.
Step-by-Step
Step 1 — Install Jenkins

Go to jenkins.io → download LTS .war file
Open terminal and run:

java -jar jenkins.war

Open browser → go to http://localhost:8080
First time: it shows a path to an unlock password file. Copy that password and paste it.
Install suggested plugins when asked
Create an admin username and password

Step 2 — Install Maven Plugin in Jenkins

Jenkins dashboard → Manage Jenkins → Plugins → Available plugins
Search Maven Integration → Install

Step 3 — Configure Maven in Jenkins

Manage Jenkins → Tools → Maven installations → Add Maven
Give it a name like Maven3, check Install automatically

Step 4 — Push your project to GitHub
In VS Code terminal (inside your Maven project folder):
git init
git add .
git commit -m "First commit"
git remote add origin https://github.com/yourusername/yourrepo.git
git push -u origin main
Step 5 — Create a Jenkins Job

Jenkins dashboard → New Item
Enter name: JUnitTest → select Freestyle project → OK
Under Source Code Management: select Git → paste your GitHub repo URL
Under Build: Add build step → Invoke top-level Maven targets

Maven Version: Maven3
Goals: test


Under Post-build Actions: Add → Publish JUnit test result report

Test report XMLs: **/surefire-reports/*.xml


Click Save

Step 6 — Build and View Results

Click Build Now
Click the build number (e.g., #1) that appears
Click Test Result → you see a full JUnit report inside Jenkins
Each test method shows pass/fail. Jenkins keeps history of all builds.


EXPERIMENT 14: Test Coverage using JaCoCo
What is Code Coverage?
Code coverage answers: "How much of my code did the tests actually run/check?"
Example: You have 10 lines of code. Your tests only exercise 6 of those lines. Your coverage is 60%. The other 4 lines are "untested" — bugs hiding there won't be caught.
What is JaCoCo?
JaCoCo (Java Code Coverage) is a tool that runs alongside your tests, watches which lines of code get executed, and generates a beautiful HTML report showing:

Green lines = covered by tests
Red lines = not covered
Yellow = partially covered (e.g., only one branch of an if-else)

Step-by-Step
Step 1 — Add JaCoCo plugin to pom.xml
Inside <build><plugins> section of your pom.xml, add:
xml<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.8.11</version>
    <executions>
        <execution>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
        <execution>
            <id>report</id>
            <phase>test</phase>
            <goals>
                <goal>report</goal>
            </goals>
        </execution>
    </executions>
</plugin>
prepare-agent → tells JaCoCo to watch code as tests run. report → generates the HTML report after tests finish.
Step 2 — Run tests
mvn test
Step 3 — Open the report

Go to target/site/jacoco/
Open index.html in your browser
You'll see a table with each class, showing % of instructions covered, % of branches covered
Click on a class name → see the actual source code highlighted green/red

Step 4 — Improve coverage

If you see red lines, write new test cases that exercise those lines
Run mvn test again → coverage should go up


EXPERIMENT 15: Introduction to Docker and Basic Commands
What is Docker? (Simple Analogy)
Imagine you build an app on your laptop. It works perfectly. But when you send it to your friend, it doesn't work because they have a different Java version, different OS, different settings.
Docker solves this by packaging your app + all its requirements (Java version, libraries, config) into one box called a container. This container runs identically on any machine that has Docker installed.
Key Concepts

Image: A read-only template/blueprint (like a class in Java). You build an image once.
Container: A running instance of an image (like an object created from a class). You can run many containers from one image.
Docker Hub: A public registry where pre-built images are stored (like GitHub for code, but for Docker images).

Step-by-Step
Step 1 — Install Docker Desktop

Go to docker.com → download Docker Desktop for your OS → install it
Start Docker Desktop. You'll see a whale icon in the system tray.
Open VS Code terminal → verify:

docker --version
docker info
Step 2 — Run your first container
docker run hello-world
Docker pulls the hello-world image from Docker Hub and runs it. It prints a success message explaining how Docker works.
Step 3 — Pull and run Ubuntu
docker pull ubuntu
docker run -it ubuntu bash
-it = interactive terminal. You're now inside a Linux Ubuntu container! Try:
ls
pwd
echo "I am inside Docker!"
exit
Step 4 — Practice all basic commands
bashdocker ps              # show running containers
docker ps -a           # show all containers (including stopped)
docker images          # show all downloaded images
docker stop <id>       # stop a running container
docker start <id>      # start a stopped container
docker rm <id>         # delete a container
docker rmi <image>     # delete an image
docker logs <id>       # see output/logs of a container
docker inspect <id>    # detailed info about a container

EXPERIMENT 16: Creating Docker Images using Dockerfile
What is a Dockerfile?
A Dockerfile is a text file with step-by-step instructions that tells Docker how to build a custom image. Every line in a Dockerfile is an instruction.
Dockerfile Instructions Explained
InstructionMeaningFROMBase image to start fromCOPYCopy files from your machine into imageWORKDIRSet the working directory inside imageRUNExecute a command while building the imageCMDCommand to run when container startsEXPOSEDocument which port the app usesENVSet environment variables
Step-by-Step
Step 1 — Create a project folder
Make a new folder called DockerJava. Inside it, create Main.java:
javapublic class Main {
    public static void main(String[] args) {
        System.out.println("Hello from inside Docker!");
        System.out.println("Java is running in a container!");
    }
}
Step 2 — Create Dockerfile
In the same DockerJava folder, create a file named exactly Dockerfile (capital D, no extension):
dockerfile# Start from official Java 17 image
FROM openjdk:17

# Copy everything from current folder to /app inside the image
COPY . /app

# Set working directory inside image to /app
WORKDIR /app

# Compile the Java file
RUN javac Main.java

# When container starts, run the compiled class
CMD ["java", "Main"]
Step 3 — Build the image
Open terminal inside the DockerJava folder:
docker build -t my-java-app .

-t my-java-app → give the image the name my-java-app
. → look for Dockerfile in current directory

Watch Docker execute each step. Each step creates a layer.
Step 4 — Verify image was created
docker images
You'll see my-java-app listed.
Step 5 — Run the container
docker run my-java-app
Output: Hello from inside Docker!
Step 6 — Experiment
Try modifying Main.java to print something else. Rebuild the image (docker build) and run again to see the change.

EXPERIMENT 17: Docker Volumes and Port Mapping
Part A: Port Mapping
Why do we need port mapping?
A container is completely isolated from your machine. If you run a web server inside a container listening on port 80, you can't access it from your browser because your browser talks to your machine's ports, not the container's ports.
Port mapping creates a bridge: your machine's port 8080 → container's port 80. Now when you go to localhost:8080, it forwards to the container.
Syntax: -p hostPort:containerPort
Step 1 — Run nginx web server with port mapping
docker run -d -p 8080:80 nginx

-d → detached mode (runs in background, doesn't block terminal)
-p 8080:80 → your port 8080 maps to container's port 80
nginx → image name (Docker downloads it automatically from Docker Hub)

Step 2 — Test it
Open browser → go to http://localhost:8080
You should see the Nginx welcome page!
Step 3 — Stop it
docker ps                    # get container ID
docker stop <container_id>
Part B: Volumes
Why do we need volumes?
By default, when you delete a container, ALL data inside it is deleted. This is a problem for databases — you don't want to lose all your data when restarting a container.
Volumes store data on your actual machine (not inside the container). The container reads/writes to the volume. Even if the container is deleted, the data on your machine remains.
Step 1 — Create a named volume
docker volume create mydata
Step 2 — Run container with volume mounted
docker run -it -v mydata:/data ubuntu bash
-v mydata:/data → mount volume mydata to path /data inside container
Step 3 — Write data inside container
bashecho "This data will persist" > /data/important.txt
cat /data/important.txt
exit
Step 4 — Delete the container and run a new one
docker run -it -v mydata:/data ubuntu bash
cat /data/important.txt
You still see This data will persist — even though it's a brand new container!
Step 5 — Inspect the volume
docker volume inspect mydata
docker volume ls

EXPERIMENT 18: Docker Image Push to Docker Hub
What is Docker Hub?
Docker Hub is like GitHub but for Docker images. You push your image to Docker Hub so:

Others can pull and use your image on any machine
Your images are stored in the cloud
You can deploy your app anywhere just by pulling the image

Step-by-Step
Step 1 — Create Docker Hub account

Go to hub.docker.com → Sign up (free)
Note your username (e.g., johnsmith)

Step 2 — Login from terminal
docker login
Enter your Docker Hub username and password. You'll see Login Succeeded.
Step 3 — Build or use existing image
If you already have my-java-app from Experiment 16, use that. Otherwise build it again:
docker build -t my-java-app .
Step 4 — Tag the image
Docker Hub requires your image to be named as yourusername/imagename:tag:
docker tag my-java-app johnsmith/my-java-app:v1

johnsmith = your Docker Hub username
my-java-app = repository name
v1 = tag/version

Step 5 — Push to Docker Hub
docker push johnsmith/my-java-app:v1
Wait for the upload. You'll see progress bars for each layer.
Step 6 — Verify on Docker Hub website

Go to hub.docker.com → Login → Repositories
You'll see my-java-app with tag v1

Step 7 — Test pulling from Docker Hub
docker rmi johnsmith/my-java-app:v1        # delete local copy
docker pull johnsmith/my-java-app:v1       # pull from hub
docker run johnsmith/my-java-app:v1        # run it
This proves your image works from Docker Hub on any machine!

EXPERIMENT 19: Jenkins and Docker Integration
What is this experiment about?
You're connecting Jenkins (CI tool) with Docker (containerization tool). Goal: every time you push code, Jenkins automatically builds a Docker image and runs it. This is the foundation of modern DevOps.
Step-by-Step
Step 1 — Install Docker Plugin in Jenkins

Jenkins dashboard → Manage Jenkins → Plugins → Available
Search Docker Pipeline → Install
Search Docker → Install Docker plugin
Restart Jenkins after installation

Step 2 — Give Jenkins permission to use Docker (Linux/Mac)
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
This adds Jenkins to the Docker group so it can run Docker commands.
Step 3 — Prepare your project
Your project folder needs:

Main.java (or any Java file)
Dockerfile
Jenkinsfile (new file — the pipeline script)

Step 4 — Create Jenkinsfile
In your project root, create Jenkinsfile:
groovypipeline {
    agent any
    
    stages {
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t jenkins-app .'
            }
        }
        
        stage('Run Docker Container') {
            steps {
                echo 'Running the container...'
                sh 'docker run jenkins-app'
            }
        }
    }
    
    post {
        success {
            echo 'Build and run successful!'
        }
        failure {
            echo 'Something went wrong!'
        }
    }
}
Step 5 — Push to GitHub
git add .
git commit -m "Added Jenkinsfile and Dockerfile"
git push
Step 6 — Create Pipeline Job in Jenkins

New Item → name it DockerPipeline → select Pipeline → OK
Under Pipeline section:

Definition: Pipeline script from SCM
SCM: Git
Repository URL: your GitHub URL
Script Path: Jenkinsfile


Save → Build Now

Step 7 — Watch the pipeline
Jenkins shows each stage as a block. Green = success, Red = failure. Click any stage to see its console output.

EXPERIMENT 20: Simple End-to-End CI/CD Pipeline
What is CI/CD?

CI (Continuous Integration): Every code push → automatically run tests → report results
CD (Continuous Deployment/Delivery): After tests pass → automatically build Docker image → deploy/run it

This experiment combines everything: GitHub → Jenkins → Maven Test → Docker Build → Docker Run. This is exactly how real companies deploy software!
Full Flow Diagram
You push code to GitHub
        ↓
GitHub notifies Jenkins (via webhook)
        ↓
Jenkins pulls latest code
        ↓
Jenkins runs: mvn test (JUnit tests)
        ↓
If tests pass → Jenkins builds Docker image
        ↓
Jenkins runs the Docker container
        ↓
Your app is live!
Step-by-Step
Step 1 — Prepare project structure
Your project should have all of these:
MyProject/
├── src/
│   ├── main/java/com/lab/Calculator.java
│   └── test/java/com/lab/CalculatorTest.java
├── pom.xml
├── Dockerfile
└── Jenkinsfile
Step 2 — Dockerfile for Maven project
dockerfileFROM maven:3.9-openjdk-17 AS build
COPY . /app
WORKDIR /app
RUN mvn package -DskipTests

FROM openjdk:17
COPY --from=build /app/target/*.jar /app/app.jar
CMD ["java", "-jar", "/app/app.jar"]
This uses a two-stage build: first stage compiles with Maven, second stage runs the compiled jar with just Java (smaller final image).
Step 3 — Full Jenkinsfile
groovypipeline {
    agent any
    
    stages {
        stage('Clone Repository') {
            steps {
                echo 'Pulling code from GitHub...'
                git branch: 'main',
                    url: 'https://github.com/yourusername/yourrepo.git'
            }
        }
        
        stage('Run Tests') {
            steps {
                echo 'Running JUnit tests...'
                sh 'mvn test'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t myapp:latest .'
            }
        }
        
        stage('Run Docker Container') {
            steps {
                echo 'Deploying container...'
                sh 'docker run -d -p 9090:8080 myapp:latest'
            }
        }
    }
    
    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs above.'
        }
    }
}
Step 4 — Set up GitHub Webhook (auto-trigger)

Go to your GitHub repo → Settings → Webhooks → Add webhook
Payload URL: http://YOUR_JENKINS_IP:8080/github-webhook/
Content type: application/json
Select: Just the push event
Click: Add webhook

Step 5 — Configure Jenkins job

New Item → FullCICD → Pipeline → OK
Build Triggers → check GitHub hook trigger for GITScm polling
Pipeline → Pipeline script from SCM → Git → your repo URL → Script Path: Jenkinsfile
Save

Step 6 — Test the full pipeline

Make any change in your code in VS Code
Save the file → commit → push:

git add .
git commit -m "Test CI/CD pipeline"
git push

Watch Jenkins automatically trigger and run all 4 stages
Check results at http://localhost:9090

Step 7 — Verify everything

Jenkins dashboard shows green pipeline with all 4 stages
docker ps shows your container running
Your app is accessible in the browser
