# Документация к файлу config.less

### Оглавление:
1. Переменные
2. Миксины и функции
3. Правила наименования селекторов
4. Правила свойств
5. Префиксы в миксинах
6. Цветовая схема

#### Переменные:
Цвет необходимо указывать в названии переменной. Например:  
**@black-color**, **@gray**, **@green-color**

#### Миксины и функции
Все допустимые миксины и функции изложены в файле mixins.less

#### Правила наименования селекторов
* В стилях не должно быть знаков > + ~
* По возможности убираем `!important`. В идеале - их не должно быть вообще
* Если определены стили для классов, которые совпадают по имени хотя бы на 50% - выделяем общий селектор и наследуем элементы. Пример:  

_Плохо:_
```less
.ul-w-review-avatar {     
    border: none !important;    
		top: 25px;    
		object-fit: cover;    
}   
.ul-w-review-titles {   
		border: none;   
}   
```

_Хорошо:_
```less
.ul-w-review {    
  &-avatar {...}    
  &-titles {...}    
}   
```

#### Префиксы в миксинах
* Миксины, которые включают свойства с поддержкой  без префиксов в IE11+, Chrome 67+, Mozilla 58+, Opera 50+, iOS Safari 10+, должны быть без префиксов. Предварительно убедившись в том, что существующие стили не попадают в исключения при использовании CSS свойств.

__Пример 1:__

_Плохо:_
```less
.transition (@duration: 0.3s, @style: ease) {
	-webkit-transition: @duration all @style;
	-moz-transition: @duration all @style;
	-ms-transition: @duration all @style;
	-o-transition: @duration all @style;
	transition: @duration all @style;
}
```

_Хорошо:_
```less
.transition (@duration: 0.3s, @style: ease) {
	transition: @duration all @style;
}
```

![caniuse transition](http://dl3.joxi.net/drive/2018/10/31/0032/1904/2115440/40/cd6cd4190b.jpg)

[caniuse: transition](https://caniuse.com/#search=transition)

__Пример 2:__

_Плохо:_
```less
.textCenter{
	text-align: center;
	text-align: -webkit-center; 
	text-align: -moz-center;
}
```

_Хорошо:_
```less
.textCenter{
	text-align: center;
}
```

![text-align compability](http://dl3.joxi.net/drive/2018/10/31/0032/1904/2115440/40/1846ba3a03.jpg)


[w3: text-align](https://developer.mozilla.org/en-US/docs/Web/CSS/text-align)

#### Цветовая схема

Миксин .blockTheme(@arguments) должен включать __только__ цветовые определения для элементов. Все CSS-правила, которые относятся к блочной модели, позиционированию, контенту элемента должны располагаться в миксине .behavior().

__Пример 1:__

_Плохо:_
```less
.parent {
	&-design1 {
		&-titles {
			border: none

			&:before { content: none; }
			.ul-w-review-name { color: @blockColor; }
			.ul-w-review-extra { color: @textColor; }
		}
	}
}
```

_Хорошо:_
```less
.parent {
	&-design1 {
		&-titles {
			.ul-w-review-name { color: @blockColor; }
			.ul-w-review-extra { color: @textColor; }
		}
	}
}
```