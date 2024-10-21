# Setup database in settings.py, models, views, serializers, and populate data

## Use Copilot Chat and paste the following

```text
In our next steps lets think step by step and setup the following in this order

1. settings.py in our django project for mongodb octofit_db database including localhost and the port
2. settings.py in our django project setup for all installed apps. ex djongo, octofit_tracker
3. In octofit_tracker project setup models, views, and serializers for users, teams, activity, leaderboard, and workouts
```

![octofit tracker backend settings](./4_1_OctoFitTrackerBackendSettings.png)
![create models serializers](./4_2_CreateModelsViewsSerializers.png)
![backend urls populate data](./4_3_OctoFitAppBackendUrls.png)

### Sample code for models.py, serializers.py, and views.py

#### models.py

```python
# FILE: octofit-tracker/backend/octofit_tracker/models.py

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

#### serializers.py

```python
# FILE: octofit-tracker/backend/octofit_tracker/serializers.py

from rest_framework import serializers
from .models import User, Team, Activity, Leaderboard, Workout

class UserSerializer(serializers.ModelSerializer):
    class Meta:
        model = User
        fields = '__all__'

class TeamSerializer(serializers.ModelSerializer):
    class Meta:
        model = Team
        fields = '__all__'

class ActivitySerializer(serializers.ModelSerializer):
    class Meta:
        model = Activity
        fields = '__all__'

class LeaderboardSerializer(serializers.ModelSerializer):
    class Meta:
        model = Leaderboard
        fields = '__all__'

class WorkoutSerializer(serializers.ModelSerializer):
    class Meta:
        model = Workout
        fields = '__all__'
```

#### views.py

```python
# FILE: octofit-tracker/backend/octofit_tracker/views.py

from rest_framework import viewsets
from .models import User, Team, Activity, Leaderboard, Workout
from .serializers import UserSerializer, TeamSerializer, ActivitySerializer, LeaderboardSerializer, WorkoutSerializer

class UserViewSet(viewsets.ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer

class TeamViewSet(viewsets.ModelViewSet):
    queryset = Team.objects.all()
    serializer_class = TeamSerializer

class ActivityViewSet(viewsets.ModelViewSet):
    queryset = Activity.objects.all()
    serializer_class = ActivitySerializer

class LeaderboardViewSet(viewsets.ModelViewSet):
    queryset = Leaderboard.objects.all()
    serializer_class = LeaderboardSerializer

class WorkoutViewSet(viewsets.ModelViewSet):
    queryset = Workout.objects.all()
    serializer_class = WorkoutSerializer
```

[Back :: Previous: Getting started](../3_GettingStarted) | [Next :: Populate database with data via manage.py](../5_PopulateDBwData)
