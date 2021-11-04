---
layout: post
title:  "Docker Compose and Ruby on Rails"
date:   2021-11-02 15:11:00 -0700
categories: ruby docker linux
---
One of my goals in keeping up with technology news is to do web development
with the best tools available. One criterion I have for "best" is *stability*.
What software do I expect to continue to be supported?

Here, I show a setup using **Docker compose** and **Ruby on Rails**. Ruby on
Rails continues to be a popular web framework. Rails is a complete "full-stack"
framework (contrast this with choices like Flask for python or Express for
node) which is convenient for replicating common tasks&mdash;integrating many
independent packages to build your own "full-stack" web application can take a
lot of work&mdash;much of it *not* being the "fun" sort of problem solving, but
rather the painful kind of digging through StackOverflow threads and looking at
software-specific quirks, bugs, hacks, and work-arounds.

Docker compose is cool because it takes away a lot of the hassle of setting up
a local development environment. Personally, I found this to be one of the
biggest blockers preventing me from learning more web development. Setting up a
new database, managing versions of programming languages, and so on can be very
confusing. Worse yet, you don't want to mess up and break things that already
work on your own computer.  With a containerized workflow, we can develop with
software dependencies conveniently enumerated in **configuration files**. So,
when it is time to ship our projects to production or bring on other developers
to help, it is easy to reproduce the correct environments.


Go ahead and make sure you have `docker-compose` installed. Then grab my
repository [here](https://github.com/hred1723/compose-rails).

After downloading the repository, just navigate to the directory and run:

`docker-compose up`

You can now work on your app as it is running in a container (network) on your
local machine.


## Getting inside your container

Find out the id of your container with `docker ps`. Then use `docker exec` to
start a bash prompt within your container.

```bash
docker exec -it 792269b05c79 bash
```

From within the container, you can run all the bash and ruby commands given in
the [official "Getting
Started"](https://guides.rubyonrails.org/getting_started.html).

When trying to edit files locally (in VS Code) I got some permissions errors
trying to save files. You can see the permissions of the files within your
container with `ls -la` from within the container bash prompt. You'll notice
that by default files have user `1000` and group `1000`. However, new files
generated may have user `root` and group `root`. Just change the users and
groups to match everything else.

```
root@a6299f284249:/myapp# chown -R 1000 app/
root@a6299f284249:/myapp# chgrp -R 1000 app/

```

### Troubleshooting a database issue

While writing this tutorial, I got an error! In the browser at
`localhost:3000/articles`, Rails gave me a message like this:

> Migrations are pending. To resolve this issue, run: bin/rails db:migrate RAILS\_ENV=development 

Checking the server console, it looked like the relation (table) `articles`
already existed; I think I made this when I was trying to figure out some
stuff. So there was nothing important in it&mdash;must delete so I can run the
migration scripts successfully!

Connect to the postgres container (find it with `docker ps` and then `docker
exec CONTAINER_ID bash` to get inside). If anyone knows better ways to do this,
let me know!  Using the `psql` program, you can list all databases with `\l`.
See [this page](https://www.postgresqltutorial.com/postgresql-show-databases/)
for more ways to list databases in PostgreSQL.

I then changed to my app's development database with `\c myapp_development`. 
Use `\dt` to show tables. See [this
page](https://www.postgresqltutorial.com/postgresql-show-tables/) for more
information on that.

Now I can do a

```sql
DROP TABLE articles;
```

## Continuing the tutorial: using a model

In the official tutorial, we continue from [section
5.3](https://guides.rubyonrails.org/getting_started.html). 
From the web server container, get into a Ruby console:

```bash
bin/rails console
```


