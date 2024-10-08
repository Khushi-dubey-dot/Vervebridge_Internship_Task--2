# cms_app/models.py
from django.db import models
from django.contrib.auth.models import User

class Article(models.Model):
    title = models.CharField(max_length=255)
    body = models.TextField()
    image = models.ImageField(upload_to='images/')
    created_at = models.DateTimeField(auto_now_add=True)
    author = models.ForeignKey(User, on_delete=models.CASCADE)

class Image(models.Model):
    file = models.ImageField(upload_to='images/')
    uploaded_at = models.DateTimeField(auto_now_add=True)


    # cms_app/views.py
from rest_framework.permissions import IsAuthenticated

class ContentView(APIView):
    permission_classes = [IsAuthenticated]

    def get(self, request):
        # Return content based on the user’s role
        pass


# cms_app/views.py
from rest_framework import viewsets
from .models import Article
from .serializers import ArticleSerializer

class ArticleViewSet(viewsets.ModelViewSet):
    queryset = Article.objects.all()
    serializer_class = ArticleSerializer


# cms_app/serializers.py
from rest_framework import serializers
from .models import Article

class ArticleSerializer(serializers.ModelSerializer):
    class Meta:
        model = Article
        fields = '__all__'


# cms_project/urls.py
from django.urls import include, path
from rest_framework.routers import DefaultRouter
from cms_app import views

router = DefaultRouter()
router.register(r'articles', views.ArticleViewSet)

urlpatterns = [
    path('', include(router.urls)),
]


# cms_app/admin.py
from django.contrib import admin
from .models import Article

admin.site.register(Article)


DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'cms_db',
        'USER': 'cms_user',
        'PASSWORD': 'yourpassword',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}


# Sample frontend fetching data from the backend API
fetch('/api/articles/')
    .then(response => response.json())
    .then(data => {
        // Render articles dynamically
    });
