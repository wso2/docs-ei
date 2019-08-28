# FIX Examples

Update the following configurations in the ei.toml file:

```toml
[transport.fix]
listener.enabled=false
sender.enabled=false
```

The transport implementation is based on Quickfix/J open source FIX engine and hence the following additional dependencies are required to enable the FIX transport.

-   `          mina-core.jar         `
-   `          quickfixj-core.jar         `
-   `          quickfixj-msg-fix40.jar         `
-   `          quickfixj-msg-fix41.jar         `
-   `          quickfixj-msg-fix42.jar         `
-   `          quickfixj-msg-fix43.jar         `
-   `          quickfixj-msg-fix44.jar         `
-   `          slf4j-api.jar         `
-   `          slf4j-log4j12.jar         `

Download [Quickfix/J](https://www.quickfixj.org/) and in the
distribution archive you will find all the dependencies listed above.
Also please refer to Quickfix/J documentation on configuring FIX
acceptors and initiators.