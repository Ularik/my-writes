## Firefox
```
from selenium import webdriver  
from selenium.webdriver.support import expected_conditions as EC  
from selenium.webdriver.common.by import By  
from time import sleep  
from selenium.webdriver.firefox.firefox_profile import FirefoxProfile  
  
profile_path = '/home/ular/user-profiles/seleniumProfile'  # profile path
  
options = webdriver.FirefoxOptions()  
options.add_argument(f'-profile {profile_path}')   
  
driver = webdriver.Firefox(options=options)  

url = "https://web.whatsapp.com/"  # example
driver.get(url)  
sleep(60)  
driver.quit()
```

