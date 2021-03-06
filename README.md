# exploreapollo.org-schema
Database management for exploreapollo.org

## Database

All migrations should be run against Postgres 9.4

## Domains

### `reasonable_string`

Text with the constraint: 0 < length <= 254

[254 is the maximum length of an email address.][email_max]

## Tables

### `users`

All fields in this table have a minimum length of 1 and a max length of 254 characters.

field | domain
--- | ---
`id` | [SERIAL][]
`name` | `reasonable_string`
`email` | `reasonable_string`
`password` | [TEXT][]

### `missions`

field | domain | description
--- | --- | ---
`id ` | [SERIAL][]
`name ` | [TEXT][] | Machine readable mission identifier
`title` | [TEXT][] | Display name
`met_start_time` | [TIMESTAMP WITH TIMEZONE][] | Point in time where the MET starts. In ISO 8601 format: `1999-01-08 04:05:06 -8:00`. Use UTC.

### `channels`

field | domain | description
--- | --- | ---
`id` | [SERIAL][]
`mission` | [INTEGER][] | Mission foreign key
`name` | [TEXT][]
`title` | [TEXT][] | Display name
`description` | [TEXT][]

### `channel_chunks`

field | domain | description
--- | --- | ---
`id` | [SERIAL][]
`channel` | [INTEGER][] | Foreign key for channel this chunk belongs to
`url` | [TEXT][] | Full HTTPS URL to audio file
`name` | [TEXT][] | Original file name or helpful label
`met_start` | [BIGINT][] | Mission elapsed time in milliseconds where this chunk begins
`met_end` | [BIGINT][] | Mission elapsed time in milliseconds where this chunk ends

### `stories`

field | domain | description
--- | --- | ---
`id` | [SERIAL][]
`name` | [TEXT][] | Text slug
`title` | [TEXT][] | Display name
`description` | [TEXT][]
`created` | [TIMESTAMP WITH TIMEZONE][] | Time of row addition

### `moments`

field | domain | description
--- | --- | ---
`id` | [SERIAL][]
`name` | [TEXT][] | Text slug
`title` | [TEXT][] | Display name
`description` | [TEXT][]
`met_start` | [BIGINT][] | Mission elapsed time in milliseconds where this moment begins
`met_end` | [BIGINT][] | Mission elapsed time in milliseconds where this moment ends
`created` | [TIMESTAMP WITH TIMEZONE][] | Time of row addition

### `moment_story_join`

Many-to-many relationship table between moments and stories

field | domain | description
--- | --- | ---
`id` | [SERIAL][]
`moment_id` | [INTEGER][] | Foreign key for moment
`story_id` | [INTEGER][] | Foreign key for story
`moment_order` | [INTEGER][] | Integer denoting the order the moment occurs in the story

### `moment_channel_join`

Many-to-many relationship between moments and channels

field | domain | description
--- | --- | ---
`id` | [SERIAL][]
`moment_id` | [INTEGER][] | Foreign key for moment
`channel_id` | [INTEGER][] | Foreign key for channel

### `speakers`

field | domain | description
--- | --- | ---
`id` | [SERIAL][]
`name` | [TEXT][] | Text slug
`title` | [TEXT][] | Display name, perhaps the speaker's full name
`photo_url` | [TEXT][] | HTTPS URL to photo

### `transcript_parts`

field | domain | description
--- | --- | ---
`id` | [SERIAL][]
`message` | [TEXT][] | The spoken text for the part
`met_start` | [BIGINT][] | Mission elapsed time in milliseconds where this moment begins
`met_end` | [BIGINT][] | Mission elapsed time in milliseconds where this moment ends
`speaker_id` | [INTEGER][] | Foreign key for speaker
`channel_id` | [INTEGER][] | Foreign key for channel

[email_max]: http://www.rfc-editor.org/errata_search.php?rfc=3696&eid=1690
[TEXT]: http://www.postgresql.org/docs/9.4/static/datatype-character.html
[SERIAL]: http://www.postgresql.org/docs/9.4/static/datatype-numeric.html#DATATYPE-SERIAL
[INTEGER]: http://www.postgresql.org/docs/9.4/static/datatype-numeric.html#DATATYPE-INT
[BIGINT]: http://www.postgresql.org/docs/9.4/static/datatype-numeric.html#DATATYPE-INT
[TIMESTAMP]: http://www.postgresql.org/docs/9.4/static/datatype-datetime.html
[TIMESTAMP WITH TIMEZONE]: http://www.postgresql.org/docs/9.4/static/datatype-datetime.html
