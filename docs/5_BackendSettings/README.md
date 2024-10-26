# The OctoFit Tracker database and app backend creation

## Initialize the database, setup database and install apps in settings.py, models, serializers, urls, and views

Type the following prompt in GitHub Copilot Chat:

```text
In our next steps lets think step by step and setup the following in this order

1. Initialize the mongo octofit_db database and create a correct table structure for users, teams, activity, leaderboard, and workouts collections
2. Make sure there is a unique id for primary key for the user collection 
   ex. db.users.createIndex({ "email": 1 }, { unique: true })
3. settings.py in our django project for mongodb octofit_db database including localhost and the port
4. settings.py in our django project setup for all installed apps. ex djongo, octofit_tracker, rest_framework
5. In octofit_tracker project setup and use command touch models.py, serializers.py, urls.py, and views.py for users, teams, activity, leaderboard, and workouts
6. Generate code for models.py, serializers.py, and views.py and
7. make sure urls.py has a root, admin, and api endpoints
    urlpatterns = [
        path('', api_root, name='api-root'),  # Root endpoint
        path('admin/', admin.site.urls),  # Admin endpoint
        path('api/', include(router.urls)),  # API endpoint
    ]
```

![OctoFit Tracker backend prompt](./5_1_BackendSettingsPrompt.png)</br>
![OctoFit Tracker backend response step 1](./5_2_BackendSettingsStep1.png)</br>
![OctoFit Tracker backend response step 2 and 3](./5_2_BackendSettingsStep2Step3_1.png)</br>
![OctoFit Tracker backend response step 3 continued](./5_2_BackendSettingsStep3_2.png)</br>
![OctoFit Tracker backend response step 3 continued](./5_2_BackendSettingsStep3_3.png)</br>

### MongoDB commands to setup `octofit_db`

```bash
mongo
use octofit_db
db.createCollection("users")
db.createCollection("teams")
db.createCollection("activity")
db.createCollection("leaderboard")
db.createCollection("workouts")
db.users.createIndex({ "email": 1 }, { unique: true })
db.teams.createIndex({ "name": 1 }, { unique: true })
db.activity.createIndex({ "activity_id": 1 }, { unique: true })
db.leaderboard.createIndex({ "leaderboard_id": 1 }, { unique: true })
db.workouts.createIndex({ "workout_id": 1 }, { unique: true })
exit
```

### Sample settings.py

```json
# FILE: octofit_tracker/settings.py

DATABASES = {
    'default': {
        'ENGINE': 'djongo',
        'NAME': 'octofit_db',
        'HOST': 'localhost',
        'PORT': 27017,
    }
}
```

```json
# FILE: octofit_tracker/settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'djongo',
    'octofit_tracker',
]
```

### Sample code for models.py, serializers.py, views.py, and urls.py

#### models.py

```python
# FILE: octofit-tracker/backend/octofit_tracker/models.py

from djongo import models

class User(models.Model):
    _id = models.ObjectIdField()
    username = models.CharField(max_length=100)
    email = models.EmailField(unique=True)
    password = models.CharField(max_length=100)

class Team(models.Model):
    _id = models.ObjectIdField()
    name = models.CharField(max_length=100)
    members = models.ArrayReferenceField(to=User, on_delete=models.CASCADE)

class Activity(models.Model):
    _id = models.ObjectIdField()
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    activity_type = models.CharField(max_length=100)
    duration = models.DurationField()

class Leaderboard(models.Model):
    _id = models.ObjectIdField()
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    score = models.IntegerField()

class Workout(models.Model):
    _id = models.ObjectIdField()
    name = models.CharField(max_length=100)
    description = models.TextField()
```

#### serializers.py

```python
# FILE: octofit-tracker/backend/octofit_tracker/serializers.py

from rest_framework import serializers
from .models import User, Team, Activity, Leaderboard, Workout
from bson import ObjectId

class ObjectIdField(serializers.Field):
    def to_representation(self, value):
        return str(value)

    def to_internal_value(self, data):
        return ObjectId(data)

class UserSerializer(serializers.ModelSerializer):
    _id = ObjectIdField()

    class Meta:
        model = User
        fields = '__all__'

class TeamSerializer(serializers.ModelSerializer):
    _id = ObjectIdField()
    members = UserSerializer(many=True)

    class Meta:
        model = Team
        fields = '__all__'

class ActivitySerializer(serializers.ModelSerializer):
    _id = ObjectIdField()
    user = ObjectIdField()

    class Meta:
        model = Activity
        fields = '__all__'

class LeaderboardSerializer(serializers.ModelSerializer):
    _id = ObjectIdField()
    user = ObjectIdField()

    class Meta:
        model = Leaderboard
        fields = '__all__'

class WorkoutSerializer(serializers.ModelSerializer):
    _id = ObjectIdField()

    class Meta:
        model = Workout
        fields = '__all__'
```

#### views.py

```python
# FILE: octofit-tracker/backend/octofit_tracker/views.py

from rest_framework import viewsets
from rest_framework.decorators import api_view
from rest_framework.response import Response
from rest_framework.reverse import reverse
from .serializers import UserSerializer, TeamSerializer, ActivitySerializer, LeaderboardSerializer, WorkoutSerializer
from .models import User, Team, Activity, Leaderboard, Workout

@api_view(['GET'])
def api_root(request, format=None):
    return Response({
        'users': reverse('user-list', request=request, format=format),
        'teams': reverse('team-list', request=request, format=format),
        'activity': reverse('activity-list', request=request, format=format),
        'leaderboard': reverse('leaderboard-list', request=request, format=format),
        'workouts': reverse('workout-list', request=request, format=format),
    })

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

#### urls.py

```python
# FILE: octofit-tracker/backend/octofit_tracker/urls.py

from django.contrib import admin
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import UserViewSet, TeamViewSet, ActivityViewSet, LeaderboardViewSet, WorkoutViewSet, api_root

router = DefaultRouter()
router.register(r'users', UserViewSet)
router.register(r'teams', TeamViewSet)
router.register(r'activities', ActivityViewSet)
router.register(r'leaderboards', LeaderboardViewSet)
router.register(r'workouts', WorkoutViewSet)

urlpatterns = [
    path('', api_root, name='api-root'),  # Root endpoint
    path('admin/', admin.site.urls),  # Admin endpoint
    path('api/', include(router.urls)),  # API endpoint
]
```

## GitHub Copilot Chat commands to help debug issues

```text
/help

#selection - The current selection in the active editor
#codebase - Searches through the codebase and pulls out relevant information for the query.
#editor - The visible source code in the active editor
#terminalLastCommand - The active terminal's last run command
#terminalSelection - The active terminal's selection
#file - Choose a file in the workspace
```

[:arrow_backward: Previous: Let's work on front end](../4_FrontEndWork/README.md) | [Next: Populate the database with sample data :arrow_forward:](../6_PopulateDBwData/README.md)
