# Recipient List Endpoint

A Recipient List endpoint can contain multiple child endpoints or member
elements. It routes cloned copies of messages to each child recipient.

This will assume that all immediate child endpoints are identical in
state (state is replicated) or state is not maintained at those
endpoints.
