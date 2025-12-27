auctions/ urls.py

`from django.urls import path` : Django 프레임워크가 제공하는 path 함수를 가져온다

`path('auction/<int:auction_id>/', views.auction_detail, name='auction_detail'),`

> 주소: auction/숫자/ 형태입니다. (예: auction/1/, auction/55/)

> 주소창에 적힌 숫자를 auction_id라는 이름의 변수로 잡아서 views.auction_detail 함수에 넘겨줍니다.

path 변환기

<int:auction_id>가 뷰 함수의 매개변수 이름과 일치해야 한다

유지보수 
path(. . . name='auction_detail')
name='auction_detail' 
> HTML(템플릿)에서 <a href="/auction/{{ auction.id }}/"> 처럼 하드코딩하지 않고, {% url 'auction_detail' auction.id %} 처럼 씁니다.