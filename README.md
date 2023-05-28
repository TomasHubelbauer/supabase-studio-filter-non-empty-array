# Supabase Studio filter non-empty array

I am a big fan of Supabase and also their Supabase Studio web UI for database
management.
It is not yet completely competitive with something like Postico, but it is a
very solid core and continually improved.

Recently I was using it to debug an issue in the work app and one of the things
I needed to do to investigate was to trim down the table listing to just rows
whose certain column had a non-empty array value.
The Supabase Studio has a Filter button for this, where you select your column
and comparator/condition and the RHS value to use.

I am not very strong at Postgres, I am definitely more on the side of learning
by doing than learning by studying, so my mind didn't immediately go to the PSQL
syntax for an empty array.

I mucked around with `[]` (because that's how empty arrays are displayed in the
Supabase Studio.
I mucked around with `ARRAY()` because I figured comparing against an empty
array might work through some type of type coercion / structural equality stuff
in Postgres (plain assumption here).
I mucked around with comparing to `NULL` with the same logic, maybe there is a
loose comparison that will treat an empty array as `NULL`?

Nothing worked so I went ahead and asked in the Supabase GitHub Discussions:
<https://github.com/orgs/supabase/discussions/14092>

Turns out I was close-ish!
The right answer is to use `{}` as the syntax for an empty array.
The `ARRAY()` constructor expression did not work because looking back at the
docs now the right syntax is `ARRAY[]` anyway, so it couldn't have worked.

I went back to check if comparing against `ARRAY[]` would work now but it looks
as though that is crashing the BE call Supabase Studio makes to execute the
filter. :D
Oh well!
