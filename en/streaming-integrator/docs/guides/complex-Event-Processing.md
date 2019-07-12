# Complex Event Processing

[Gartner’s IT
Glossary](https://www.gartner.com/it-glossary/complex-event-processing)
defines CEP as follows:

!!! info

"CEP is a kind of computing in which **incoming data about events is
distilled into more useful, higher level “complex” event data** that
provides insight into what is happening."

" **CEP is event-driven** because the computation is triggered by the
receipt of event data. CEP is used for highly demanding,
continuous-intelligence applications that enhance situation awareness
and support real-time decisions."


WSO2 SP allows you to detect patterns and trends for decision making via
Patterns and Sequences supported for Siddhi.

#### Patterns

Siddhi patterns allow you to detect patterns in the events that arrive
over time. This can correlate events within a single stream or between
multiple streams. A pattern can be a counting pattern that allows you to
match multiple events received for the same matching condition, or a
logical pattern that match events that arrive in temporal order and
correlate them with logical relationships.

For more information, see [Siddhi Query Guide -
Patterns](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#pattern)
.

#### Sequences

This allows you to carry out analysis based on the sequential order in
which events matching certain conditions arrive.  The difference between
sequences and patterns is, in Siddhi sequence, events arriving
consecutively are compared based on given conditions. For patterns,
events do not have to arrive in consecutive manner.

For more information, see [Siddhi Query Guide -
Sequence](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#sequence)
.
