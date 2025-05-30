...

urlpatterns = [  
    path('swagger/', schema_view.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),  
    path('admin/', admin.site.urls),  
    path('user/', include('user.urls')),  
    path('product/', include('product.urls')),  
    path('basket/', include('basket.urls'))  
]  
...
  
def page_not_found(request, exception):  
    return HttpResponseNotFound('Страница не найдена')  
  
handler404 = page_not_found