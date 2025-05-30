./manage.py test store.tests.test_logic
coverage run --source='.' ./manage.py test .
coverage report
coverage html