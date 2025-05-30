check settings:

	LANGUAGE_CODE = "en-us"  
	  
	LANGUAGES = [  
	    ('de', 'German'),  
	    ('en', 'English'),  
	    ('ru', 'Russian')  
	]  
	  
	LOCALE_PATHS = [  
	    os.path.join(BASE_DIR, 'locale'),  
	]  
	
	  
	USE_I18N = True
	

create middleware.py file:

	from django.utils.translation import activate  
	from django.conf import settings  
	from pprint import pprint  
	  
	class LanguageMiddleware:  
	    def __init__(self, get_response):  
	        self.get_response = get_response  
  
	    def __call__(self, request): 
	        if request.user.is_authenticated:  
	            user_language = request.GET.get('language')  
	            activate(user_language)  
	        else:                                      
	            activate(settings.LANGUAGE_CODE)  
	        response = self.get_response(request)  
	        return response

add this middleware in settings:

MIDDLEWARE = [  
    "django.middleware.security.SecurityMiddleware",  
    "django.contrib.sessions.middleware.SessionMiddleware",  
    'django.middleware.locale.LocaleMiddleware',      # add
    "django.middleware.common.CommonMiddleware",  
    "django.middleware.csrf.CsrfViewMiddleware",  
    "django.contrib.auth.middleware.AuthenticationMiddleware",  
    "django.contrib.messages.middleware.MessageMiddleware",  
    "django.middleware.clickjacking.XFrameOptionsMiddleware",  
    'myuser.middleware.LanguageMiddleware',       # add
]

from django.utils.translation import gettext_lazy as _

class MyUser(AbstractBaseUser, PermissionsMixin):  
    email = models.EmailField(  
        unique=True  
    )  
    name = models.CharField(  
        max_length=30,  
        verbose_name=_('Name')  
    )  
    language = models.ForeignKey(  
        Languages,  
        on_delete=models.PROTECT,  
        verbose_name=_('languages')  
    )  
    country = models.ForeignKey(  
        Country,  
        on_delete=models.PROTECT,  
        verbose_name=_('Country'),  
        default=1  
    )  
    city = models.ForeignKey(  
        Cities,  
        on_delete=models.PROTECT,  
        verbose_name=_('City'),  
        default=1  
    )  
    phone_number = PhoneNumberField(  
        region='RU',  
        blank=True,  
        null=True  
    )  
    category = models.ForeignKey(  
        Category,  
        on_delete=models.PROTECT,  
        default=1,  
        verbose_name=_('Category')  
    )

## also

1. core/urls
	urlpatterns += i18n_patterns(  
	    path('i18n/', include('django.conf.urls.i18n')),  
	    path("language/", include('myuser.urls', namespace='myuser')),  
	    path('user/', include('myuser.urls', namespace='myuser'))  
	)
2. templates/language.html:
<form action="{% url 'set_language' %}" method="post">{% csrf_token %}  
    <input name="next" type="hidden" value="{{ redirect_to }}">  
    <select name="language">  
        {% get_current_language as LANGUAGE_CODE %}  
        {% get_available_languages as LANGUAGES %}  
        {% get_language_info_list for LANGUAGES as languages %}  
        {% for language in languages %}  
            <option value="{{ language.code }}"{% if language.code == LANGUAGE_CODE %} selected{% endif %}>  
                {{ language.name_local }} ({{ language.code }})  
            </option>  
        {% endfor %}  
    </select>  
    <input type="submit" value="Go">  
</form>

3. views.py:
	def get_language(request):  
	    return render(request, 'myuser/language.html')

1. `python manage.py makemessages -l ru --ignore ..\templates --extension html,txt -v 2`

## model translation

pip install django-modeltranslation

1. settings:
		LANGUAGE_CODE = "en"  
		LOCALE_PATHS = (  
		    os.path.join(BASE_DIR, 'locale/'),  
		)  
		  
		gettext = lambda s: s  
		LANGUAGES = (  
		    ('en', gettext('English')),  
		    ('ru', gettext('Russian')),  
		)  
		  
		TEMPLATE_CONTEXT_PROCESSORS = (  
		    'django.core.context_processors.i18n', # this one  
		)  
		  
		TIME_ZONE = "UTC"
2. middleware:
		MIDDLEWARE = [  
    ...
    "django.contrib.sessions.middleware.SessionMiddleware",  
    'django.middleware.locale.LocaleMiddleware',       # add this
    "django.middleware.common.CommonMiddleware",
3. apps:
		INSTALLED_APPS = [  
	    'modeltranslation',     # add
4. create translation.py in app:
		from modeltranslation.translator import translator, TranslationOptions  
		from .models import MyUser  
		  
		  
		class MyUserTranslationOptions(TranslationOptions):  
		    fields = ('username', 'created_date')  # fields which you want translate
		  
		translator.register(MyUser, MyUserTranslationOptions)
5. add in admin.py:

		from modeltranslation.admin import TranslationAdmin
		
		class UserAdmin(BaseUserAdmin, **==TranslationAdmin==**):
		
		
		