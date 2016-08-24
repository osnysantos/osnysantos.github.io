---
title:  "How to reset Postgres id sequence in a Ruby on Rails app"
date:   2016-08-20 14:27:18 -0300
categories: rails postgres
---

Although it is not something so common to happen, sometimes we came across a table on the database where the id of the primary key column are not synchronized, for example, the last record saved is **id=200**, but when we try to create a new record, Postgres tries to create with a different id from the default order, which would be **id=201**, in this case.

In my case, what made the sequence to get lost was a Ruby task that I’ve created to import data from another applicaton. Moreover, in order to not lose reference of links, I set the ids according to the old table and after import, the new records were trying to use an id that was already being saved, giving me the following error:

<script src="https://gist.github.com/osnysantos/acf44ced4dc8ea03a0dbda3e09d29883.js"></script>

Googling this issue, I came across with several solutions using only Postgres, but in this article I want to share with you a solution that works very well, using just Rails console, as it follows:

<script src="https://gist.github.com/osnysantos/07f01135f819d3654e79f21d0826370f.js"></script>

Personaly, I prefer this solution for three reasons:

- Access to Postgres console is not required;
- Although the access to Postgres console is usually easy to have, in some projects/companies, this may be time-consuming;
- It fixes the problems in all tables of the project at once;
- Code easy to read and to understand.

I hope this tip can save you some time :)

[1]: http://stackoverflow.com/questions/244243/how-to-reset-postgres-primary-key-sequence-when-it-falls-out-of-sync
[2]: http://stackoverflow.com/questions/11068800/rails-auto-assigning-id-that-already-exists
