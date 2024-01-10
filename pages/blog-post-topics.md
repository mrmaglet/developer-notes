# Operational errors vs. programmer errors

It’s helpful to divide all errors into two broad categories:

## Operational errors

Represent run-time problems experienced by correctly-written programs. These are not bugs in the program. In fact, these are usually problems with something else: the system itself (e.g., out of memory or too many open files), the system’s configuration (e.g., no route to a remote host), the network (e.g., socket hang-up), or a remote service (e.g., a 500 error, failure to connect, or the like).

- failed to connect to server
- failed to resolve hostname
- invalid user input
- request timeout
- server returned a 500 response
- socket hang-up
- system is out of memory

## Programmer errors

These are bugs in the program. They can always be avoided by changing the code. They can never be handled properly (since by definition the code in question is broken).

- tried to read property of “undefined”
- called an asynchronous function without a callback
- passed a “string” where an object was expected
- passed an object where an IP address string was expected

People use the term “errors” to talk about both operational and programmer errors, but they’re really quite different. Operational errors are error conditions that all correct programs must deal with, and as long as they’re dealt with, they don’t necessarily indicate a bug or even a serious problem. “File not found” is an operational error, but it doesn’t necessarily mean anything’s wrong. It might just mean the program has to create the file it’s looking for first.
By contrast, programmer errors are bugs. They’re cases where you made a mistake, maybe by forgetting to validate user input, mistyping a variable name, or something like that. By definition there’s no way to handle those. If there were, you would have just used the error handling code in place of the code that caused the error!
This distinction is very important: operational errors are part of the normal operation of a program. Programmer errors are bugs.

Sometimes, you have both operational and programming errors as part of the same root problem. If an HTTP server tries to use an undefined variable and crashes, that’s a programmer error. Any clients with requests in flight at the time of the crash will see an ECONNRESET error, typically reported in Node as a “socket hang-up”. For the client, that’s a separate operational error. That’s because a correct client must handle a server that crashes or a network that flakes out.
Similarly, failure to handle an operational error is itself a programmer error. For example, if a program tries to connect to a server but it gets an ECONNREFUSED error, and it hasn’t registered a handler for the socket’s 'error' event, then the program will crash, and that’s a programmer error. The connection failure is an operational error (since that’s something any correct program can experience when the network or other components in the system have failed), but the failure to handle it is a programmer error.
The distinction between operational errors and programmer errors is the foundation for figuring out how to deliver errors and how to handle them. Make sure you understand this before reading on.

[Source - joyent.com](https://www.joyent.com/node-js/production/design/errors)

<hr>
