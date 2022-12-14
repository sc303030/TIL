# db에서 models 삭제하기

- player 모델에서 name이 중복된 선수와 현역 선수를 제거한다.

1. name 필드를 기준으로 값을 가져오기 위해 values를 사용한다.
   1. values를 사용하면 `{'name' : 'curry'}`  형식으로 값을 반환한다.
2. 1번에서 name을 기준으로 가져온 값을 annotate를 사용해서 id로 group by를 수행한다.
3. 2번에서 수행한 값 `cnt` 를 기준으로 2개 이상인 obj만 반환한다.
4. 3번의 값으로 for loop를 돌면서 이름을 기준으로 데이터를 가져오고
   1. retire_year로 오름차순하여 맨 처음 값만 가져온다.
5. 해당 obj를 삭제한다.
6. 현역 선수 이름을 리스트로 담는다.
7. 선수 이름이 들어간 모든 obj를 가져온다.
8. 다 삭제한다.
   1. 여기서는 1개라 그냥 사용했고 2개 이상이면 for loop로 돌면서 삭제하면 된다.

```python
import os

os.environ['DJANGO_SETTINGS_MODULE'] = 'nba_predict_django.settings'
import django

django.setup()
from nba_app.models import Player
from django.db.models import Count

player_obj = Player.objects.values('name').annotate(cnt=Count('id')).filter(cnt__gte=2)
for i in player_obj:
    name = i['name']
    obj = Player.objects.filter(name=name).order_by("retire_year")[0]
    obj.delete()
    
activate_player = ["Ray Allen"]
activate_player_obj = Player.objects.filter(name__in=activate_player)
activate_player_obj.delete()
```

