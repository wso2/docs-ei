# Debugging mediation

Message mediation mode is one of the operational modes of WSO2 EI where
EI functions as an intermediate message router. A unit of the mediation
flow is a mediator. A sequence is a series of mediators, where each
mediator is a unit entity that can input a message, carry out a
predefined processing task on the message, and output the message for
further processing. Debugging is where you want to know if these units,
which function as separate entities are operating as intended, or if a
combination of these units are operating as a whole as intended.

Message mediation mode is one of the operational modes of the Micro Integrator where the Micro Integrator functions as an intermediate message
router. When operating in this mode, it can filter, transform, drop or
forward messages to an endpoint based on the given parameters. A unit of
the mediation flow is a mediator. Sequences define the message mediation
behavior of the Micro Integrator. A sequence is a series of mediators, where
each mediator is a unit entity that can input a message, carry out a
predefined processing task on the message, and output the message for
further processing.

## What is debugging with respect to mediation?

Debugging is where you want to know if these units, which function as separate entities are operating as intended, or if a combination of
these units are operating as a whole as intended. The Micro Integrator packs the **Mediation debugger** that enables you to debug the message mediation flow in the server. Tooling support for the
Mediation debugger is provided by the **WSO2 Integration Studio Plugin**
which comes out of the box with WSO2 Integration Studio.
