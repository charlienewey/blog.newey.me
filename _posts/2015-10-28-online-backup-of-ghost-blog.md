---
layout: post
title: Online Backup of Ghost Blog
date: 2015-10-28 13:17:48.000000000 +00:00
---

I've been running a Ghost blog and some other pieces of software on my server
for the last couple of years, and to my shame, until recently I hadn't sorted
out a proper backup scheme. Being paranoid, I set up
[Tarsnap](https://www.tarsnap.com) to perform daily backups of my Git
repositories and home directory, and weekly backups of my Ghost blog, GitLab,
and other stuff.

<!-- more -->

Annoyingly, to back up the database for my Ghost blog, I kept having to issue
`service ghost stop` - that is, I had to stop my blog serving temporarily while
I backed up the database. Even though backups were scheduled to take place very
late at night (when nobody is likely to be trying to access my blog), it was
still annoying to have to take my blog offline for 10 minutes per week while it
was backed up.

Why is this? By default, Ghost blogs use an SQLite3 database to store data.
SQLite3 is a good database technology well-suited to running a small blog, but
you can't copy the file to another location while the database is being used.
Doing so would mean there may still be pending write operations on the database
- copying the original database file before transactions have been committed
means that the backup database could end up being corrupted.

As usual, I'm not the first person to encounter this problem, and the easiest
way of avoiding this happening is with shared locks (there's a nice analogy on
this [StackOverflow answer](http://stackoverflow.com/a/11837714)). Essentially,
you write a short script to connect to the SQLite3 database and establish a
shared lock (meaning that the database is temporarily read-only for any
connected applications, *including* your still-running Ghost blog), and copy
the database while the shared lock is in effect.

Here's a short Ruby script that [I found on
GitHub](https://gist.github.com/willb/3518892) to demonstrate what I mean;

```ruby
#!/usr/lib/env ruby

# Acquires a shared lock on a SQLite database file and copies it to a backup
# usage:  backup-db.rb DBFILE.db BACKUPFILE.db
# author:  William Benton (willb@redhat.com)
# Public domain.

require 'sqlite3'
require 'fileutils'

def backup_db(db_file, backup_file)
  begin
    db = SQLite3::Database.new(db_file)

    db.transaction(:immediate) do |ignored|
      # starting a transaction with ":immediate" means we get a shared lock
      # and thus any db writes (unlikely!) complete before we copy the file
      FileUtils.cp(db_file, backup_file, :preserve=>true)
    end
  ensure
    db.close
  end
end

backup_db(ARGV[0], ARGV[1])
```

All you need to do is run `backup-db.rb /path/to/original.db
/path/to/backup.db` and you can take a backup while the database is still being
used. Now I can back my blog up without any downtime!

Much better.
