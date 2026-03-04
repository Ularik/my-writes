

<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <link type="text/css" href="{% static 'comix/css/basic.css' %}" rel="stylesheet" />  
    <title>Title</title>  
  <style>
      
	 nav {  
	     background-color: #333; /* Цвет фона навигации */  
	     color: #fff; /* Цвет текста навигации */  
	 }  
	  
	 nav ul {  
	     list-style-type: none; /* Убираем маркеры списка */  
	     margin: 0;  
	     padding: 0;  
	     overflow: hidden;  
	 }  
	  
	 nav ul li {  
	     float: left; /* Размещаем элементы в строку */  
	 }  
	  
	 nav ul li a {  
	     display: block; /* display: block;: Это свойство display устанавливает элементы <a> как блочные, что означает, что они будут отображаться в виде блоков, занимающих всю доступную ширину. */  
	     color: inherit;  
	     text-align: center;  
	     padding: 14px 16px; /* Отступы вокруг текста ссылки */  
	     text-decoration: none; /* убирает подчёркивание у ссылок */  
	 }  
	  
	 nav ul li a:hover {  
	     background-color: #555; /* Цвет фона при наведении */  
	 }  
	  
	  
	  
	  
	  
	body {  
	     background-color: #f0f0f0;  
	}  
	  
	 .comics {  
	     font-family: Arial, sans-serif;   /* шрифт для всего что внутри body */  
	     margin: 0;                        /* отступы от края к центру =0 */  
	     padding: 0;                       /* отступы от края к центру  */  
	     display: flex;                    /* используют модель Flexbox для центрирования дочерних элементов по горизонтали и вертикали */  
	     justify-content: center;          /* центрирует по горизонтали */  
	 }  
	  
	 .gallery {  
	     text-align: center;  
	 }  
	  
	 .gallery-item {  
	     margin-bottom: 20px;            /* отступ снизу */  
	     border: 2px solid #ccc;         /* граница рамки */  
	     padding: 10px;                  /* отступ от края к центру */  
	     max-width: 200px; /* Ширина блока с изображением */  
	     margin: 0 auto;  
	 }  
	  
	 .gallery-item img {  
	     max-width: 100%; /* Сделать изображение внутри блока адаптивным */  
	     display: block;  
	     margin: 0 auto;  
	 }  
	  
	 .gallery-item p {  
	     margin-top: 10px;  
	 }
  </style>
</head>  
<body>  
  
<div class="comics">  
    <header>        <h1>Our comics!</h1>  
    </header>    <div class="gallery">  
        {% for record in comix %}  
            <div class="gallery-item">  
                {% if record.image %}  
                <a href="{% url 'detail' record.id %}"><img src="{{ record.image.url }}"></a>  
                {% else %}  
                {% endif %}  
                <p>{{ record.description }}</p>  
            </div>        {% endfor %}  
    </div>  
</div>
  
</body>  
</html>