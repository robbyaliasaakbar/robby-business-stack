# My Small Business Stack

Hey, thanks for stopping by.
This is just my small setup for running a tiny business flow at home.
Nothing fancy here. I built it to learn and to keep customer chat in one place.
Feel free to copy it and change it to fit your own needs.

## Why this exists

I wanted chat and data and automation to live together without too much setup.
I also wanted something I can turn off and on with one command.
So I put everything in one compose file and shared it here.

## What lives here

- postgres on 5432 for memory
- evolution-api on 8080 for WhatsApp
- n8n on 5678 for brain

## How it fits together

The automation tool is the brain. You build the flow there.
When a message comes in, the brain reads and writes to the database.
Then it calls the chat tool to send or receive WhatsApp.
The database is the memory. It keeps customers and schedules.
The chat sessions live in the instances folder.

## Quick start

You need Docker plus the compose plugin.
You need your own env file because secrets are not in this repo.

- copy env example to env
- fill in your own password and keys in the new env file
- check config with compose config
- run compose up
- open localhost 5678 for automation
- open localhost 8080 for chat tool

```sh
cp .env-example .env
docker compose config
docker compose up
docker compose ps
docker compose logs
docker compose down
```

## Ports

- 5432 for database
- 5678 for automation UI
- 8080 for chat tool

## Env vars

These live in your env file. The example file has empty values.

- POSTGRES_PASSWORD for database
- AUTHENTICATION_API_KEY for chat auth
- SERVER_PORT for chat port
- SERVER_URL for public URL
- DATABASE_PROVIDER for database type
- DATABASE_CONNECTION_URI for database link

## Files

This part is important. Some files are safe to share. Some files must stay private.

Safe to share:

- README.md
- docker-compose.yml
- versi software.txt
- .gitignore

Keep private:

- .env for keys
- instances for WhatsApp sessions
- credential.json for automation creds
- backup_db_mas.sql for database dump
- My workflow.json for automation flow
- pgdata for database files
- .n8n for automation data
- .n8n-files for automation files

## Common issues

If the brain cannot reach the database, check that Postgres is up and the password matches.
If the chat tool cannot connect, check the port in your env file.
If a port is busy, stop the other app that uses that port and try again.

## Safety notes

I learned this the hard way. Secrets once leaked must be treated as burned.

- never add private files to git
- if you add by mistake, remove from cache, do not just delete the file
- revoke old passwords and keys and scan WhatsApp sessions again
- keep this repo private until only safe files remain
- check tracked files with git ls-files

## Notes

This is for local use and learning. It is not hardened for public servers.
I still tweak it from time to time. Thanks for reading.
