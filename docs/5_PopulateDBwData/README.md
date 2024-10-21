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
from octofit_tracker.models import User, Team, Activity, Leaderboard, Workout

class Command(BaseCommand):
    help = 'Populate the database with sample data'

    def handle(self, *args, **kwargs):
        user1 = User.objects.create(
            username='user1',
            email='user1@example.com',
            password='password'
        )
        user2 = User.objects.create(
            username='user2',
            email='user2@example.com',
            password='password'
        )

        team_member1 = {
            'id': str(user1.id),
            'user': user1.to_dict()
        }
        team_member2 = {
            'id': str(user2.id),
            'user': user2.to_dict()
        }

        team1 = Team.objects.create(
            name='team1',
            members=[team_member1, team_member2]
        )
        team2 = Team.objects.create(
            name='team2',
            members=[team_member2]
        )

        Activity.objects.create(
            user=user1,
            activity_type='Running',
            duration=30
        )
        Activity.objects.create(
            user=user2,
            activity_type='Cycling',
            duration=45
        )

        Leaderboard.objects.create(
            user=user1,
            score=100
        )
        Leaderboard.objects.create(
            user=user2,
            score=150
        )

        Workout.objects.create(
            user=user1,
            workout_type='Strength',
            duration=60
        )
        Workout.objects.create(
            user=user2,
            workout_type='Cardio',
            duration=40
        )

        self.stdout.write(
            self.style.SUCCESS('Successfully populated the database with sample data')
        )
```

![Migrate and populate db](./5_3_MigratePopulateDb.png)
