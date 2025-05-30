import os 

- os.getcwd() 
print(os.getcwd())    # return current directory path
```
/home/ular/PycharmProjects/parsingtest
```

```print(__file__) or print(os.path.abspath(__file__)) ```  # abspath is normalize path

```
/home/ular/PycharmProjects/parsingtest/some.py    # file name
```

- os.path.dirname(path)
print(os.path.dirname(os.path.abspath(__file__)))
```
/home/ular/PycharmProjects/parsingtest
```