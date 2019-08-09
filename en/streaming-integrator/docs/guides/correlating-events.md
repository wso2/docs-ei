#Correlating Data

The streaming integrator can correlate data in order to detect patterns and trends in streaming data. Correlating can be done via patterns as well as sequences.

The difference between patterns and sequence is that sequences require all the matching events to arrive consecutively to
 match the sequence condition, whereas patterns identify events that match the pattern condition irrespective of the order in which they arrive.

## Correlating events to find a pattern

This section explains how you can use Siddhi patterns to detect trends and patterns. There are two types of Siddhi patterns as follows:

- Counting Patterns: These count the number of intances that match the given pattern condition.
- Logical Patterns: These identify logical relationships between events.

### Count and match multiple events for a given pattern condition
### Combine several patterns logically and match events

## Find non-occurance of events

This section explains how to detect non-occurring events.

##Correlating events to find a trend(sequence)

This section explains how you can use Siddhi sequences to detect trends in events that arrive in a specific order. There are two types of Siddhi sequences as follows:

- Counting Sequences: These count the number of intances that match the given sequence condition.
- Logical Sequences: These identify logical relationships between events.

### Count and match multiple events for a given trend
### Combine several trends logically and match events

##Correlating two streams of data and unify

For a detailed explanation, see [Enriching Data - Enrich data by connecting with another stream of data](enriching-data/#enrich-data-by-connecting-with-another-stream-of-data)

## Correlate a stream and a static datasource to enrich
For a detailed explanation, see[Enriching Data - Enrich data by connecting with a data store](enriching-data/#enrich-data-by-connecting-with-a-data-store)
