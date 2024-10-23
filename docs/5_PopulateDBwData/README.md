# Populate database with data via manage.py

## Example of not being specific

> NOTE: This is an example of not being specific skip this prompt and go to the next one

```text
Let's use manage.py to get everything setup we need to create init.py for populate_db command include steps to migrate as well
```

### Example of not being specfic in prompting Copilot Chat

![populate db Code your app name](./5_1_PopulateDbCodeYourAppNameFirst.png)

## Example of being more specific in our prompt

### Let's be more specific and ask Copilot to update the output with the octofit_tracker app name

Type the following prompt in GitHub Copilot Chat:

```text
Let's use manage.py to get the database setup and populated

- Populate the database with super hero users
- Create full setup for a command populate_db.py
- Include steps to migrate in the octofit_tracker project
```

![populate db Code app name octofit_tracker](./5_2_PopulateDbCodeOctoFitAppSecond.png)

### Sample code for populate_db.py

```python
# FILE: octofit-tracker/backend/octofit_tracker/management/commands/populate_db.py

from django.core.management.base import BaseCommand
from octofit_tracker.models import User, Team, Activity, Workout, Leaderboard
from datetime import timedelta

class Command(BaseCommand):
    help = 'Populate the database with initial data'

    def handle(self, *args, **kwargs):
        # Create Superhero Users
        users = [
            {'username': 'superman', 'email': 'superman@heroes.com', 'password': 'superpassword'},
            {'username': 'batman', 'email': 'batman@heroes.com', 'password': 'batpassword'},
            {'username': 'wonderwoman', 'email': 'wonderwoman@heroes.com', 'password': 'wonderpassword'},
        ]

        for user_data in users:
            user, created = User.objects.get_or_create(**user_data)
            if created:
                self.stdout.write(self.style.SUCCESS(f"User {user.username} created"))

        # Add more data population logic here if needed

        self.stdout.write(self.style.SUCCESS('Database populated successfully'))
```

![Migrate and populate db](./5_3_MigratePopulateDb.png)
