
# Linkedin Site Reliability engineer
## Useful Links
* [Interview methodology from LinkedIn](https://engineering.linkedin.com/blog/2017/07/hiring-sres-at-linkedin)
* [Original adapted from github@krishnaramb](https://github.com/krishnaramb/FB_Prep/wiki/linkedin)
* [System Design primer](https://github.com/donnemartin/system-design-primer)
* To prepare: read the networks: top down approach book. For linux, do unix and linux sys admins handbook
## Coding
* Describe the relationship between a class and an object
* How do you write a regular expression for some silly pattern in Python
* Write a program that prints a 12x12 multiplication table matrix.
* Write a program that reverses the contents of a file, byte for byte.
* Write a program that descends through a directory tree and prints all files. (hint: recursion is your friend here.)
### fizz buzz
* [Example](https://ditam.github.io/posts/fizzbuzz/)
* Write a program which prints out all numbers between 1 and 100. When the program would print out a number exactly divisible by 4, print "Linked" instead. When it would print out a number exactly divisible by 6, print "In" instead. When it would print out a number exactly divisible by both 4 and 6, print "LinkedIn."
### log parsing
* [Example](https://stackoverflow.com/questions/30627810/how-to-parse-this-custom-log-file-in-python)
* Write code to parse generic web logs and format it in different ways
* Given an Apache log file, print the timestamp hour, minute, and second, followed by the number of times any log entry occurs during that time. (hint: if you're programming in perl, a hashed array works great here.)
* Parse a log file based on the interviewer's choice (String format is huge)
* Take part of a log file, export to CSV
* Parse a log except, providing a csv of the count of each proc per DTTM.
* pull some stuff out of logs and count it
* Print all statistics of every process name and its log for every minute of a piece of system log. In other word, use minute as row title, print all statistics in the corresponding minutes.
   * people answer is there in glassdoor
### Rest API
* Last one was involved in Linkedin RESTful api calling and recursion
* Recursive call of LinkedIn api using REST.
### Debugging Code
* Review poorly written code and point out the places where it would fail. 
### misc coding
* wanted me to solve a pretty messy problem processing a spreadsheet input with an online algorithm in python to print a set of tables using maps it in linear time,.

## Technology
* Some things about scaling IT infrastructure
* conceptual/design interviews. Many questions on systems knowledge ( networks, OS, linux, ssl, etc). 
* Interviewer describes the full stack setup of a web app, Please list the benefits and disadvantages of this setup. And if you could, how would you improve it?
* high-level asking of questions related to web architecture and how I would go about scaling X or Y.

## Architecture
* Design Twitter / YouTube / Facebook
   * [Twitter](https://blog.twitter.com/engineering/en_us/topics/infrastructure/2017/the-infrastructure-behind-twitter-scale.html)
   * [twitter example](https://medium.com/@narengowda/system-design-for-twitter-e737284afc95)
* How would you design a scale out web service.

## Troubleshooting
* First round consisted of 3 simple coding problems related to sysadmin tasks like log parsing and interacting with a REST api.
* modules ranging from code review, incident management to architecture
### Apache
* The first question was a simple permissions problem on a file being requested by the client. The ownership of the file (a blank text file) was too restrictive, so it was raising an error. You could verify this in the apache web logs.
* The second error was due to a permission problem too, however this time the file was hidden in a sub directory of the main web site. You could only determine this by looking at the apache configuration file to see that the shtml file was located somewhere else. After that, change the permissions to fix.
* The third was a head scratcher. The filename in question was raising a 500 error and showing urlencoded characters in the filename in the web log. Looking at the name of the file on disk though, showed nothing out of the ordinary.
   * It turns out that the unicode representations for the characters in the file name are printed in the terminal as english ascii characters. The only way you can tell that this is the case is to open the file and do a search for the filename itself and see if it matches. For example, if the correct filename is called "challenge1.shtml" you can search for that exact string but NOT find the unicode version of it.
   * Once you find the incorrect file name, delete it and type the correct file name (in this case "challenge3.shtml" into the file and the page works.
* The final question was a segfault occurring in apache. It resulted in no information being returned to the client. You could see this occurring in the apache web logs as well as the Chrome tools.
   * The apache web logs noted that a core file was dumped. This challenge required that you know a little bit about gdb and C programming. Basically, you need to run the core dump through gdb.
   * _gdb /path/to/apache /path/to/core/dump_
   * It will spew out a lot of stuff. In particular, it mentions that there is something happening in an apache module; mod_rewrite or something...it doesnt really matter.
   * The output also points to the C source file for that module which is, conveniently on disk. Open that file in vi and jump to the line number mentioned in the gdb output (line 1861 or something). There you will see that if the filename matches challenge4.shtml to SIGSEGV; there's your smoke gun.
   * They dont ask you to fix the final challenge, only to explain what the strstr is doing. The error in question basically looks like this
   * _ if (strstr($r->filename, "challenge4.shtml") != NULL) { SIGSEGV } _
   * Just point out to them that, yeah, it's segfaulting when I ask for that file.
* There was a paper presented to you with a number of nagios alerts and you had to rate them in the order you would approach fixing them.
   * For example, one of them was a production host being 100% offline.
   * Another was an environment alert about an entire cab that was overheating. Another was the tablet vip being down, another was a load average for the main website being really high.
   * There were also a number of them that were QPS (queries per sec) related and included several security related alerts like XSS QPS and failed logins QPS

## Linux Kernel
Linux kernel internals got pretty in depth, as did questions about system architecture for high-performance websites
### Distribute large file to large number of servers
* You need to distribute a terabyte of data from a single server to 10,000 nodes, and then
* keep that data up to date. 
* It takes several hours to copy the data just to one server. 
* How would you do this so that it didn't take 20,000 hours to update all the servers? 
* Also, how would you make sure that the file wasn't corrupted during the copy?

* Expained everything from fork, exec, dns, tcp, http, etc etc.
* public cryptography.
* Questions on IP addressing , port numbers,TCP headers, basic linux commands,processes.
* How HTTPS works
* Asked about API calls and parsing data returned from them.
* the questions were on bash, tcp, processors
* I focussed more on Linux Fundamentals like learning about fork, runleves, init, docker, application deployment and port numbers.
* data structure, API, and file system I/O).
* basic questions about linux os and networking. Coding screen with actual SRE via collabedit, focused on file system, log parsing, deployment.
* deep dive into all the stuff I knew about networking, UNIX, operating systems, parallelism, security, and other relevant stuff. The next engineer presented a case study, focusing on scalability of a simple system.
* process priority
* Question about shell, tcp/ip and Linux internal( fork, etc.)
* They asked me about Bash Shell, https, IPv4, etc.
* what can you glean from the Apache logs on a webserver
* how would you know how performance was being impacted. (too many users hitting the server, or not enough resources allocated, etc)
* There were also operating system level questions. A few off the top of my head:
* If you have an executable program (a binary) and you made a copy of that program, and then changed permissions on the copy, would a diff show that the file had been changed?
* Asked me how the kernel new to connect to a remote machine . Wasn't too sure if they were asking the lower level c calls, general OS theory, or just basic files / networking components (routing table). Also asked basic monitoring questions like how to monitor query times (bonked on this, just couldn't think well being nervous)

## Networking
* a list of IP addresses and asked which ones are public and which ones are private (it weren't as easy as 127.0.0.1 or 192.168.0.1).
   * [ipv4 private addrs](https://en.wikipedia.org/wiki/Private_network)
* What kind of IP addresses are these lists recognizable ones
* what is the name of the protocols for blahblah...
* Describe the difference between TCP and UDP, advantages and disadvantages of both.
* Name as many TCP flags as you can. (URG, ACK, PSH, RST, SYN, FIN - mnemonic: Unskilled Attackers Pester Real Security Folks.)
* What protocol(s) do/does DNS use when you run an nslookup. (answer: normally UDP, but TCP is used for zone transfers and if a record is too long to be returned via UDP)
* Describe what happens when you do "curl".?

### SSH
* When I try to connected to a remote machine using (for example) ssh, how does ssh know how to get to that remote machine. (be able to describe routing, default routes, and host name lookup.) 
* "You type ssh . Can you talk for 30 minutes about what is going on between you hit Enter and you get the login prompt?"
* Talk me through what happens when you make an ssh connection to a remote machine. Be able to be specific, such as the identification string exchange, algorithm negotiation, key exchange, etc.

## Linux
* sysadmin basics, how's the boot process for a linux machine.
* System Design questions revolving around implementing a file server system.
* When you run a program from the shell, why doesn't the program log you out when it's done running? If you wanted this behavior, how would you run the program? (answer: exec)
* Questions on DBMS, OS, Linux/Unix and projects mentioned in resume.
* Questions on Inode, bootstrap, filesystems, congestion control etc.
* Multiple Choice questions included questions from Data structures, DBMS, OS and Computer Networks.
* First we started with operating system - questions about paging, swapping, what is virtual memory etc. Then Computer Networks - 
* What is difference between tcp/udp, which one is used in live streaming and paid subscriptions. 
* How connection is established between nodes etc. 
* In Java, he asked me the internal working of garbage collector and finally how 
* B-Tree works internally and need of threading.
* He has also asked some basic DNS lookup question, 
* linux questions about what system calls related to listing files and 
* what tool you can use to check I/O load. 
* HTTP browser request, 
* how to monitor a website, 
## generic
* What is the most rewarding problem that you've overcome in your work?
* What problem did you solve in your career that you are most proud of.
