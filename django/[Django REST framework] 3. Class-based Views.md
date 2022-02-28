# [Django REST framework] 3. Class-based Views

### view 세팅

#### restapi > appapi > views.py

```python
class SnippetList(APIView):
    def get(self, request, format=None):
        snippets = Snippet.objects.all()
        serializer = SnippetSerializer(snippets, many=True)
        return Response(serializer.data)

    def post(self, request, format=None):
        serializer = SnippetSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

- `request.method`로 요청을 구분하지 않고 함수 자체로 요청을 구분하는게 fbv랑 차이점이다.

```python
class SnippetDetail(APIView):
    def get_object(self, pk):
        try:
            return Snippet.objecs.get(pk=pk)
        except Snippet.DoesNotExist:
            raise Http404

    def get(self, request, pk, format=None):
        snippet = self.get_object(pk)
        serializer = SnippetSerializer(snippet)
        return Response(serializer.data)

    def put(self, request, pk, format=None):
        snippet = self.get_object(pk)
        serializer = SnippetSerializer(snippet, data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def delete(self, request, pk, format=None):
        snippet = self.get_object(pk)
        snippet.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
```

- `get_objects`로 함수를 만들어서 객체를 불러올 수 있다.
  - 바로 객체를 리턴한다.
  - raise로 에러를 발생시킬 수 있다.
- 함수 안에서 `get_objects`로 객체를 불러온다.

### url 수정

#### restapi > appapi > urls.py

```python
from django.urls import path
from .views import *
from rest_framework.urlpatterns import format_suffix_patterns

urlpatterns = [
    path('snippets/', SnippetList.as_view()),
    path('snippets/<int:pk>/', SnippetDetail.as_view()),
]

urlpatterns = format_suffix_patterns(urlpatterns)
```

- fbv는 `as_view()`를 붙이지 않지만 cbv는 뒤에 붙여준다.

### mixins

- cbv 장점이 다른 것들을 상속받아서 섞어서 사용할 수 있다.

```python
class SnippetList(mixins.ListModelMixin, mixins.CreateModelMixin, generics.GenericAPIView):
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer

    def get(self, request, *args, **kwargs):
        return self.list(request, *args, **kwargs)
    
    def post(self, request, *args, **kwargs):
        return self.create(request, *args, **kwargs)
```

- 각각 함수마다 serializer를 선언해서 리턴하지 않아도 mixin을 사용해서 serializer_class에 사용할 serializer를 선언하면 된다.
- 상속받은 class에서 사용하는 함수를 바로 리턴할 수 있다.

```python
class SnippetDetail(mixins.RetrieveModelMixin, mixins.UpdateModelMixin, mixins.DestroyModelMixin,
                    generics.GenericAPIView):
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer

    def get(self, request, *args, **kwargs):
        return self.retrieve(request, *args, **kwargs)

    def put(self, request, *args, **kwargs):
        return self.update(request, *args, **kwargs)

    def delete(self, request, *args, **kwargs):
        return self.destroy(request, *args, **kwargs)
```

- `get`, `put`,`delete`도 상속받은 클래스에서 가져와 사용할 수 있다.

### generic class based views

- 이미 mixed-in이 탑재되어 있는 view를 사용할 수 있다.

```python
class SnippetList(generics.ListCreateAPIView):
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer


class SnippetDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer
```

- 그러면 굳이 `get`. `post`를 선언하지 않아도 된다.
  - 오버라이딩 하려면 다른 cbv처럼 `get`, `post`를 선언하면 된다.