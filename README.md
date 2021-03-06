![License MIT](https://img.shields.io/badge/license-MIT-blue.svg)
![Python version](https://img.shields.io/badge/python-3.5.2-blue.svg)
# Simple Toggl HTML report generator

Generates graphs of daily time logged per project and per user.
Projects might span several workspaces so we call them metaprojects to distinguish from Toggl projects.


## Configuration

The generator is configured through `settings.py` and two data files.

First of all you must get your Toggl API token and put it into
`api_token.txt`. Without API token nothing will work.

In `settings.py` you can configure number of days to generate report for:
```py
timeframe = datetime.timedelta(days=30)
```
This parameter heavily affects running time.

Finally you should specify mapping from workspaces to metaprojects in `workspaces.json`. It might be
one-to-one or many-to-one mapping. Technically nothing prevents you from specifying
one-to-many mapping but I bet this will mess all things up. Correct config looks like this:
```js
{
    "YORSO": "YORSO",
    "YORSO-2": "YORSO"
}
```
Only workspaces mentioned in the mapping get processed.


## Usage

```
$ python3 metaprojects_report.py
```
or
```
$ chmod a+x metaprojects_report.py
$ ./metaprojects_report.py
```

Then open `index.html` in your favorite web-browser.

## Deploy
To simplify deploy on server and provide ability to look on charts via web-browser - `Dockerfile`
 that build nginx with static route to charts is included. Therefore, if you want to see updated charts every day,
 you should rebuild docker every night.
 1. Build docker `docker build -t toggl .`
 2. Run docker `docker run --name toggl_nginx -d -p 80:80 toggl` (change ports according to your needs)

Also we have `build_env.sh` script that remove old container, and build new one.
