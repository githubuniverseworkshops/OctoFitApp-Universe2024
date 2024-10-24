# Using the Codespace endpoint to access the Django REST Framework

> NOTE: Skip this step if not using codespaces

Type the following prompt in GitHub Copilot Chat:

```text
Let's do the following step by step

- update #file:views.py to replace the return for the rest api url endpiints with the codespace url http://upgraded-space-happiness-959pr7vpgw3p7vv.github.dev/-8000.app.github.dev for django
- Replace <codespace-name> with [REPLACE-THIS-WITH-YOUR-CODESPACE-NAME]
- Run the Django server

HTTP 200 OK
Allow: GET, HEAD, OPTIONS
Content-Type: application/json
Vary: Accept

{
    "users": "http://localhost:8000/api/users/?format=api",
    "teams": "http://localhost:8000/api/teams/?format=api",
    "activities": "http://localhost:8000/api/activities/?format=api",
    "leaderboards": "http://localhost:8000/api/leaderboards/?format=api",
    "workouts": "http://localhost:8000/api/workouts/?format=api"
}

becomes

HTTP 200 OK Allow: GET, HEAD, OPTIONS Content-Type: application/json Vary: Accept

{ 
    "users": "http://<codespace-name>-8000.app.github.dev/users/api/users/?format=api",
    "teams": "http://<codespace-name>-8000.app.github.dev/api/teams/?format=api",
    "activities": "http://<codespace-name>-8000.app.github.dev/api/activities/?format=api",
    "leaderboards": "http://<codespace-name>-8000.app.github.dev/api/leaderboards/?format=api",
    "workouts": "http://<codespace-name>-8000.app.github.dev/api/workouts/?format=api" 
}
```

[Back :: Previous: Getting started - app frontend and backend creation](../4_BackendSettings/README.md | [Next :: Using the Codespace endpoint to access the Django REST Framework](../6_CodesapceDjangoRESTFramework/)