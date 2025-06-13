github url: https://github.com/ETJeanMachine/atproto-scheduling

There are two central parts to this application as I consider it further:
1. An ingestor, listening on the firehose for scheduled records for users it is authenticated to post for. The ingestor stores the scheduled records in a local db and then posts them when it's ready for it to go out.
2. A webapp that allows users to authorise the ingestor to post and remove records on their behalf, and that allows them to publish records to schedule posts.

The point of architecting it this way is that *anyone*, in principle, should be able to host the first part of the system - rather than it being exclusively locked to a central system. 

In principle, the lexicon for the "scheduled record" item should look something akin to:
```json
{
	"sendTimestamp": "",
	"delTimestamp": "",
	"record": {...}
}
```

`postTimestamp` is the time at which the record is to be scheduled to send, and `delTimestamp` is for when the record should be deleted. The latter should *not* be able to precede the former. You can have both or one or the other, but not either to be a valid record. This should be all reflected in the lexicon, schema, somehow, but I am unclear as to how this should work.

Containing the record directly in the lexicon instead of as a `strongref` is preferable I think here - as things like posts will show up on `bsky.app` regardless of whenever their scheduled timestamp says. There isn't a good way to hold this otherwise that I can think of for right now.

I am unclear as to what the namespace for this should be - I may opt to make it a community lexicon, though, out of simplicities sake. I don't think it should be tied to a domain I own, like `jeanmachine.dev`. I may choose to use a personal domain in testing but this shouldn't be a permanent thing that I'm holding exclusivity over.