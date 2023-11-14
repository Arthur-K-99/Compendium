# Fork Bomb

## About

A fork bomb is a form of denial-of-service (DoS) attack against a Linux system that exploits the fork system call, which is used to create a new process. The concept of a fork bomb is quite simple: it's a process that continuously replicates itself to deplete the available system resources, causing the system to slow down or crash as a result of resource exhaustion.

## Scope

Any Unix based system like Linux or MacOS can be vulnerable to a DOS attack using fork calls unless configured against it. Windows does not use the fork system call, but similar concepts can be applied using different scripting or programming methods to spawn processes rapidly.

## Implementation

```bash
:(){ :|:& };:
```

Here's a breakdown of how it works:

1. `:` - Defines a function named `:`.
2. `()` - Indicates the start of the function definition.
3. `{ :|:& }` - The function body. The function : calls itself, pipes its output to another call of itself `:|:`, and then puts the process in the background & to run concurrently.
4. `;` - Separates the function definition from the invocation of the function.
5. `:` - The final `:` calls the function, setting off the chain reaction.

Once this command is executed, it will keep creating new processes infinitely, quickly overwhelming the system. Because it doesn't require any special permissions to run, any user able to execute shell commands can potentially execute a fork bomb.

## Countermeasures
1. **Limit User Processes:** Use the ulimit command to restrict the number of processes that a user can spawn. For example, `ulimit -u 100` will limit the user to 100 processes. You can make this setting permanent for a user by adding it to the user's profile script.

2. **Configure PAM Limits:** Set up limits in /etc/security/limits.conf for users by adding lines like:

3. **Restrict Access:** Only give shell access to trusted users. If a user does not need to run shell scripts or commands, they should not have access to a shell.

4. **Kernel-level Protection:** The Linux kernel can be configured with certain security features that can help prevent fork bombs by enabling process number limiting for users/groups.

5. **Monitoring and Alerts:** Implement monitoring to detect unusual levels of process creation and configure alerts so that an administrator can take action if necessary.

6. **Education:** Educate users about the risks of running untrusted code and the potential impacts of a fork bomb.

7. **Regular Updates:** Keep your system updated, as newer versions of the kernel and system utilities might provide better mechanisms for handling resource exhaustion.

8. **cgroups:** Use control groups (cgroups) to isolate and limit resources used by users or services.

By setting these configurations, even if a fork bomb is initiated, it will only be able to create a limited number of processes, thus mitigating the potential damage and keeping the system stable.