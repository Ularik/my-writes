
  
  
To reverse the URL pattern defined in Django, you can use the `reverse()` function along with the name of the URL pattern. Here's how you would do it for the URL pattern you provided:

pythonCopy code

`from django.urls import reverse 

reverse_url = reverse('detail', kwargs={'pk': 123})

This will generate the URL string corresponding to the given view name 'detail' and the provided `pk`