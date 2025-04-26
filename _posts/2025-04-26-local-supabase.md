---
published: true
layout: post
author: eugene
categories: GameGen
tags:
  - GameGen
  - Supabase
  - Docker
  - Local Development
  - AI
  - LLM
  - Database
---

# Gamegen day N - Local Supabase

This blog will contain information I did to create a new project called Gamegen, which is a simple idea where we generate a game completely out of one prompt. I will go through multiple steps using all AI models on device, which allows anyone to develop and run this project nicely. 

It is "day-N" is because I have already started this project in quite a while, and I didn't keep count of the days. I hope the viewers can learn something about using AI tools to generate contents and using LLM to generate and write code for any use cases.

## Local Supabase Setup (Docker)

I figured out that I needed one local Supabase instance, when the 7-day non-active limits on supabase is getting annoying. What will I do next is to develop on a local database.

Read: https://supabase.com/docs/guides/self-hosting/docker

```bash
# Get the code
git clone --depth 1 https://github.com/supabase/supabase

# Make your new supabase project directory
mkdir supabase-project

# Tree should look like this
# .
# ├── supabase
# └── supabase-project

# Copy the compose files over to your project
cp -rf supabase/docker/* supabase-project

# Copy the fake env vars
cp supabase/docker/.env.example supabase-project/.env

# Switch to your project directory
cd supabase-project

# Pull the latest images
docker compose pull

# Start the services (in detached mode)
docker compose up -d
```

Paste this into the terminal and we should get a supabase instance locally.

I am also using sudoless docker which means I need to update this part of the `.env` file:

```bash
# .env
DOCKER_SOCKET_LOCATION=docker_socket_location
```


## Exporting Data from Supabase

I know sooner or later we would need to copy the contents of the supabase out of the docker container, and to keep them as a backups too.

I will soon update on this, in the meantime I am wating for the `docker compose pull` which takes forever.

## Note, before running the Supabase in Production

Go aheaad and edit the `.env` file in the `supabase-project` directory. 

```bash
# .env

POSTGRES_PASSWORD=your-super-secret-and-long-postgres-password
JWT_SECRET=your-super-secret-jwt-token-with-at-least-32-characters-long
ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyAgCiAgICAicm9sZSI6ICJhbm9uIiwKICAgICJpc3MiOiAic3VwYWJhc2UtZGVtbyIsCiAgICAiaWF0IjogMTY0MTc2OTIwMCwKICAgICJleHAiOiAxNzk5NTM1NjAwCn0.dc_X5iR_VP_qT0zsiyj_I_OZ2T9FtRU2BBNWN8Bu4GE
SERVICE_ROLE_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyAgCiAgICAicm9sZSI6ICJzZXJ2aWNlX3JvbGUiLAogICAgImlzcyI6ICJzdXBhYmFzZS1kZW1vIiwKICAgICJpYXQiOiAxNjQxNzY5MjAwLAogICAgImV4cCI6IDE3OTk1MzU2MDAKfQ.DaYlNEoUrrEn2Ig7tqibS-PHK5vgusbcbo7X36XVt4Q
DASHBOARD_USERNAME=supabase
DASHBOARD_PASSWORD=this_password_is_insecure_and_should_be_updated
SECRET_KEY_BASE=UpNVntn3cDxHJpq99YMc1T1AQgQpc8kfYTuRgBiYa15BLrx8etQoXz3gZv1/u2oq
VAULT_ENC_KEY=your-encryption-key-32-chars-min
```