# System-Calls
NOTE: do not run this software on your own machine in order to protect the kernel. Please install a VM in order to run this program.

This program hijacks(intercepts) other system calls and adds a very basic kernel module to the Linux kernel.
Here is what "hijacking (intercepting) a system call" means. A new system call named my_syscall was implemented, which allows you to send commands from userspace, to intercept another pre-existing system call (like read, write, open, etc.). After a system call is intercepted, the intercepted system call would log a message first before continuing performing what it was supposed to do.

For example, if we call my_syscall with command REQUEST_SYSCALL_INTERCEPT and target system call number _NR_mkdir (which is the macro representing the system call mkdir) as parameters, then the mkdir system call would be intercepted; then, when another process calls mkdir, mkdir would log some message (e.g., "muhahaha") first, then perform what it was supposed to do (i.e., make a directory).

But wait, that's not the whole story yet. Actually we don't want mkdir to log a message whenever any process calls it. Instead, we only want mkdir to log a message when a certain set of processes (PIDs) are calling mkdir. In other words, we want to monitor a set of PIDs for the system call mkdir. Therefore, you will need to keep track, for each intercepted system call, of the list of monitored PIDs. Our new system call will support two additional commands to add/remove PIDs to/from the list.

When we want to stop hijacking a system call (let's say mkdir but it can be any of the previously hijacked system calls), we can invoke the interceptor (my_syscall), with a REQUEST_SYSCALL_RELEASE command as an argument and the system call number that we want to release. This will stop intercepting the target system call mkdir, and the behaviour of mkdir should go back to normal like nothing happened.
