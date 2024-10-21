# Populate database with data via manage.py

## Example of not being specific

Type the following prompt:

```text
Let's use manage.py to get everything setup we need to create init.py for populate_db command include steps to migrate as well
```

### Example of not being specfic in prompting Copilot Chat

![populate db Code your app name](./5_1_PopulateDbCodeYourAppNameFirst.png)

## Example of being more specific in our prompt

### Let's be more specific and ask Copilot to update the output with the octofit_tracker app name

```text
Let's use manage.py to get everything setup we need to create init.py for populate_db command include steps to migrate as well in the octofit_tracker project
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
        # Create some users
        user1 = User.objects.create_user(username='user1', email='user1@example.com', password='password')
        user2 = User.objects.create_user(username='user2', email='user2@example.com', password='password')

        # Create a team
        team = Team.objects.create(name='Team A')
        team.members.set([user1, user2])

        # Create some activities
        Activity.objects.create(user=user1, activity_type='Running', duration=timedelta(minutes=30))
        Activity.objects.create(user=user2, activity_type='Cycling', duration=timedelta(hours=1))

        # Create some workouts
        Workout.objects.create(name='Workout A', description='Description A', suggested_duration=timedelta(minutes=45))
        Workout.objects.create(name='Workout B', description='Description B', suggested_duration=timedelta(hours=1, minutes=15))

        # Create leaderboard entries
        Leaderboard.objects.create(user=user1, total_points=100)
        Leaderboard.objects.create(user=user2, total_points=150)

        self.stdout.write(self.style.SUCCESS('Database populated successfully'))
```

![Migrate and populate db](./5_3_MigratePopulateDb.png)
