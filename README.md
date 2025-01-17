# top90-backend

Top90 is a website that populates with soccer goals in real time as they happen around the world

https://top90.io

## Applications in this repo
- server - The server is the API for the website

- goal_poller - The goal poller is a script that runs as a cron job and polls reddit.com/r/soccer for new goals/videos to store in db/s3

- apifootball_ingest - Script to store apifootball data in the database

# Contributing Guide
Anyone is welcome to submit a PR. PRs should be tested and verified locally first if possible.

## Development Environment Setup
1. Install Go
2. Create a `.env` in the root directory and add the contents of `.env.sample` to it with real values
3. Start dev db and s3 in docker
```
$ docker-compose up
```
4. Install awscli
```
$ brew install awscli
```
5. Create s3 bucket in local s3. This is needed for storing goal videos locally.
```
$ aws --endpoint-url=http://localhost:4566 s3 mb s3://reddit-soccer-goals
```
6. Install golang-migrate
```
$ brew install golang-migrate
```
7. Seed local data
```
$ make seed
```
8. Go to http://localhost:8090/?pgsql=db
9. Login with the following credentials
```
username: admin
password: admin
database: redditsoccergoals
```
10. Look at the tables in the UI and verify they have data
11. (Optional) You can run the front end locally to see the goals https://github.com/wweitzel/top90-frontend. Note: Make sure you switch it to connect to local backend!

## Debugging
For vscode, make a `.vscode/launch.json` file and paste the following in it.
```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug Server",
            "type": "go",
            "request": "launch",
            "mode": "auto",
            "program": "./cmd/server",
            "cwd": "./"
        },
        {
            "name": "Debug Poller",
            "type": "go",
            "request": "launch",
            "mode": "auto",
            "program": "./cmd/poller",
            "cwd": "./"
        },
        {
            "name": "Debug Playground",
            "type": "go",
            "request": "launch",
            "mode": "auto",
            "program": "./cmd/playground",
            "cwd": "./"
        }
    ]
}
```
The above configuration will give you the options to "Debug Server" and "Debug Poller" in the "Run and Debug" tab of vscode.

## Resetting data
If you want to start fresh, you can easily wipe all your data by deleting the two folders in `docker-data`.

## Remaining Work
Finish the internal/apifootball client in order to:
- Add Premier League only capability
- Add true search capability based on team on player by matching goals to a player/team stored in db
- Show team schedules and rosters and click them to see the goals

## Leagues Supported
- England - Premier League
- Italy - Serie A
- Spain - La Liga
- Germany - Bundesliga
- France - Ligue 1
- World - UEFA Champions League
- World - UEFA Europa League
- World - World Cup

## Linux Commands

#### Run docker container in background restarting automatically unless stopped
```
$ docker run -p 7171:7171 -d --restart unless-stopped top90-server-v0
```

#### Renew cert
```
$ docker-compose run --rm certbot renew
```

fixtures
id
referee
date
home_team_id
away_team_id
league_id
season
