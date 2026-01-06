# Explain proxy pattern in detail in java

The proxy pattern is a structural design pattern where one object (the **proxy**) controls access to another object (the **real subject**), often adding extra behavior like security, logging, caching, or lazy loading. In Java, this is typically implemented either with a hand-written proxy class or with JDK dynamic proxies using `java.lang.reflect.Proxy` and `InvocationHandler`.[^2_1][^2_8]

## Core idea and when to use

- A proxy implements the same interface as the real object and holds a reference to it, so clients can use the proxy instead of the real object without changing their code.[^2_5][^2_8]
- Use it when:
    - Object creation or use is expensive (lazy loading, remote service).[^2_7][^2_10]
    - You need access control (security checks, rate limiting).[^2_2][^2_6]
    - You want cross-cutting behavior (logging, caching, transactions) without modifying the real implementation.[^2_8][^2_5]


## Structure in Java

Typical participants:[^2_10][^2_8]

- **Subject**: Interface or abstract class that defines the operations, e.g. `Image`, `CommandExecutor`, `UserProvider`.[^2_2][^2_7]
- **RealSubject**: The real implementation that does the actual work, e.g. `RealImage`, `CommandExecutorImpl`, `UserProviderImpl`.[^2_7][^2_2]
- **Proxy**: Implements the same interface, keeps a reference to `RealSubject`, adds behavior, and delegates calls.[^2_5][^2_8]
- **Client**: Works with `Subject` and is unaware whether it is talking to the proxy or the real implementation.[^2_8][^2_5]

Conceptual example:

```java
public interface Image {
    void displayImage();
}

public class RealImage implements Image {
    private final URL url;

    public RealImage(URL url) {
        // heavy loading
        loadImage(url);
    }

    private void loadImage(URL url) {
        // resource-intensive load
    }

    @Override
    public void displayImage() {
        // actually display
    }
}

public class ProxyImage implements Image {
    private final URL url;
    private RealImage realImage; // lazily created

    public ProxyImage(URL url) {
        this.url = url;
    }

    @Override
    public void displayImage() {
        if (realImage == null) {
            realImage = new RealImage(url); // lazy initialization
        }
        realImage.displayImage(); // delegate
    }
}
```

This demonstrates **virtual proxy**: delay creation of `RealImage` until it is needed.[^2_7][^2_8]

## Common proxy types

- **Virtual Proxy**:
    - Delays creation/loading of expensive objects until first use, like `ProxyImage` deferring `RealImage` construction.[^2_10][^2_7]
- **Protection (Access) Proxy**:
    - Adds security checks before delegating, e.g. a `CommandExecutorProxy` that allows `rm` only for admin.[^2_6][^2_2]
- **Remote Proxy**:
    - Represents an object in another JVM / machine (historically RMI; conceptually similar to HTTP clients, gRPC stubs).[^2_10]
- **Logging/Monitoring Proxy**:
    - Logs method calls, arguments, and timing without changing the real implementation.[^2_3][^2_5]


## Detailed access-control example

Simplified version of the classic `CommandExecutor` example:[^2_2]

```java
public interface CommandExecutor {
    void runCommand(String cmd) throws Exception;
}

public class CommandExecutorImpl implements CommandExecutor {
    @Override
    public void runCommand(String cmd) throws IOException {
        Runtime.getRuntime().exec(cmd);
        System.out.println("'" + cmd + "' command executed.");
    }
}

public class CommandExecutorProxy implements CommandExecutor {
    private final boolean isAdmin;
    private final CommandExecutor executor;

    public CommandExecutorProxy(String user, String pwd) {
        this.isAdmin = "admin".equals(user) && "secret".equals(pwd);
        this.executor = new CommandExecutorImpl();
    }

    @Override
    public void runCommand(String cmd) throws Exception {
        if (isAdmin) {
            executor.runCommand(cmd); // full access
        } else {
            if (cmd.trim().startsWith("rm")) {
                throw new Exception("rm command is not allowed for non-admin users.");
            }
            executor.runCommand(cmd); // restricted access
        }
    }
}
```

- Client code uses `CommandExecutor` interface and can be given either real or proxy implementation.[^2_8][^2_2]
- Proxy encloses security logic and decides whether to forward the call.[^2_2][^2_8]


## Dynamic proxy in Java

Java provides built-in support for dynamic proxies for interfaces via `Proxy` and `InvocationHandler`.[^2_1][^2_5]

- **InvocationHandler**: single method `invoke(Object proxy, Method method, Object[] args)` where you implement cross-cutting logic and delegate to the real target.[^2_3][^2_5]
- **Proxy.newProxyInstance**: creates a proxy instance at runtime that implements specified interfaces and routes all method calls to the handler.[^2_3][^2_5]

Example logging proxy:

```java
public class LoggingInvocationHandler implements InvocationHandler {
    private final Object target;

    public LoggingInvocationHandler(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("Calling " + method.getName() + " with " + Arrays.toString(args));
        Object result = method.invoke(target, args); // delegate
        System.out.println("Result: " + result);
        return result;
    }
}
```

Usage:

```java
UserProvider real = new DatabaseUserProvider();

UserProvider proxy = (UserProvider) Proxy.newProxyInstance(
        UserProvider.class.getClassLoader(),
        new Class[]{UserProvider.class},
        new LoggingInvocationHandler(real)
);
```

Now any call `proxy.getUser(id)` will log before delegating to `DatabaseUserProvider`.[^2_5][^2_3]

## Proxy vs decorator and typical use in real projects

- A proxy usually focuses on **controlling access** (who/when/how) to an existing object, while a decorator focuses on **adding behavior/feature** to results without necessarily restricting access.[^2_6][^2_1]
- In real Java systems, proxies appear in:
    - Spring AOP and transactional beans (proxies around your services).[^2_1]
    - Security and permission checks in service layers.[^2_6][^2_1]
    - Caching layers that wrap repositories or remote service clients.[^2_5][^2_8]

If you want, the next step can be to map this pattern to a concrete Spring example (e.g., how `@Transactional` is implemented via proxies) or to implement a custom annotation + proxy for logging.
<span style="display:none">[^2_4][^2_9]</span>

<div align="center">⁂</div>

[^2_1]: https://www.baeldung.com/java-proxy-pattern

[^2_2]: https://www.digitalocean.com/community/tutorials/proxy-design-pattern

[^2_3]: https://www.javacodegeeks.com/2020/09/implementing-the-proxy-pattern-in-java.html

[^2_4]: https://www.youtube.com/watch?v=TS5i-uPXLs8

[^2_5]: https://www.mscharhag.com/java/dynamic-proxy-pattern

[^2_6]: https://java-design-patterns.com/patterns/proxy/

[^2_7]: https://dzone.com/articles/design-patterns-proxy

[^2_8]: https://www.geeksforgeeks.org/system-design/proxy-design-pattern/

[^2_9]: https://www.youtube.com/watch?v=cHg5bWW4nUI

[^2_10]: https://www.tutorialspoint.com/design_pattern/proxy_pattern.htm

