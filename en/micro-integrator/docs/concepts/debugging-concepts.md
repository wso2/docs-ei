# Mediation Debugging

Message mediation is one of the operational modes of WSO2 Micro Integrator where it receives messages, performs various operations (such as message filterring, message transforming, etc.) according to given parameters, and forwards the message to the intended recepient. The Micro Integrator performs message mediation through various synapse artifacts such as **mediators**, **sequences**, **endpoints**, etc.

**Mediation Debugging** is the process of analysing whether the synapse artifacts in your message flow are operating as intended, and/or whether a combination of these artifacts (taken as a whole) are operating as intended. The development tool of WSO2 Micro Integrator (**WSO2 Integration Studio**) is shipped with the **Mediation debugger** for this purpose. You can also use other methods such as **wire logs** to debug message mediation.
