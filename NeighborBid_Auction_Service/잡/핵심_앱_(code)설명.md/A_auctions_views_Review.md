```
from .services import place_bid
from wallet.models import Wallet, Transaction
from django.db.models import Q
```
---
```
from django.shortcuts import render, get_object_or_404, redirect
from django.contrib.auth.decorators import login_required
from django.contrib import messages
render: HTML 파일을 화면에 보여줄 때 사용합니다. (가장 기본!)
```

> 파이썬(장고 포함)에서는
“내 파일에 없는 걸 쓰려면 무조건 import 해야 한다.”

> views.py에 다 적으면 코드가 너무 길어지고 지저분해집니다. 그래서 services.py라는 별도의 파일에 로직을 만들어두고 여기서는 가져다 쓰기만 하는 것이다.
입찰(place_bid)은 로직이 복잡합니다. (가격 비교, 시간 체크, 즉시 구매가 확인, 내 지갑 차감 등)

> 돈 관리는 항상 지갑(wallet) 앱으로 분리

> 데이터베이스에서 검색할 때 'OR' 조건을 쓰기 위한 도구입니다.

- get_object_or_404: 데이터를 찾다가 없으면 에러 페이지 대신 깔끔하게 '404 없음' 페이지를 보여주는 안전장치입니다. (예: 존재하지 않는 경매 번호로 접속했을 때)

- redirect: 처리가 끝나고 다른 주소로 튕겨 보낼 때 씁니다. (예: 입찰 성공 후 다시 상세 페이지로 이동)

- login_required: "로그인 안 한 사람은 못 들어옴!" 하고 막아주는 문지기(데코레이터)입니다. 경매 입찰은 로그인한 사람만 해야 하니까요.

- messages: 화면 상단에 잠깐 떴다 사라지는 알림창("입찰 성공!", "잔액이 부족합니다" 등)을 띄워주는 도구입니다.

- 재귀 함수 사용

> 성능 고려 (Performance) 이 방식은 코드는 직관적이고 깔끔하지만, 데이터가 아주 많아지면 DB 성능 이슈가 발생할 수 있습니다.

> 문제점: 함수가 재귀 호출될 때마다 DB에 쿼리를 날립니다. (N+1 문제와 유사)

> 해결책(나중에): 지금처럼 지역 데이터가 수백 개 수준이면 전혀 문제없습니다. 하지만 수만 개가 넘어가면 django-mptt 같은 라이브러리를 쓰거나, 처음부터 모든 지역을 한 번에 가져와서 파이썬 메모리 상에서 정렬하는 방식을 고려해야 합니다.
