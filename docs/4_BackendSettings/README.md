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

from django.db import models
from django.contrib.auth.models import AbstractUser, Group, Permission
from djongo import models as djongo_models

class User(AbstractUser):
    id = djongo_models.ObjectIdField(primary_key=True)
    groups = models.ManyToManyField(
        Group,
        related_name='octofit_users',
        blank=True,
        help_text='The groups this user belongs to.',
        verbose_name='groups',
    )
    user_permissions = models.ManyToManyField(
        Permission,
        related_name='octofit_users_permissions',
        blank=True,
        help_text='Specific permissions for this user.',
        verbose_name='user permissions',
    )

class Team(models.Model):
    id = djongo_models.ObjectIdField(primary_key=True)
    name = models.CharField(max_length=100)
    members = models.ManyToManyField(User)

class Activity(models.Model):
    id = djongo_models.ObjectIdField(primary_key=True)
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    activity_type = models.CharField(max_length=50)
    duration = models.DurationField()
    timestamp = models.DateTimeField(auto_now_add=True)

class Workout(models.Model):
    id = djongo_models.ObjectIdField(primary_key=True)
    name = models.CharField(max_length=100)
    description = models.TextField()
    suggested_duration = models.DurationField()

class Leaderboard(models.Model):
    id = djongo_models.ObjectIdField(primary_key=True)
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    total_points = models.IntegerField()
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
