- 로그인 후 이전 페이지로 Redirection 하기

django에 내장되어 있는 로그인 기능을 사용하면 좀 더 쉬운 것 같지만 이번 프로젝트에서는 따로 세션값을 저장하는 방식으로 구현했기 때문에 조금 까다로웠다. 구글링을 한 결과 `request.path`를 쓰면 바로 이전 페이지로 잘 돌아간다고 하는데 이 프로젝트에서는 로그인 버튼 클릭 -> 로그인 폼 -> 이전 페이지 순서로 돌아가야 했기 때문에 request.path이 로그인 폼의 path여서 문제가 있었다.



해결방법 : 로그인 버튼을 누를 때 현재 페이지의 path를 query string으로 보내주고, view에서 전역 변수에 저장한 뒤 로그인 submit 시 redirect 해주었다.

키워드로 검색해서 검색 결과가 나온 페이지에서 로그인 버튼을 눌렀을 경우, 로그인을 완료하고 나면 검색 결과 페이지로 다시 돌아가야 했기 때문에 query string까지 `request.get_full_path()'로 가져와주어야 했다. 쿼리 스트링 value가 2개 이상이면 & 연산자 때문에 첫 번째 query string 값만 가져오는 상황이 발생했는데, urlencode를 함께 사용함으로써 해결되었다.

```
# 로그인 버튼 - base.html
<a class="mb-0" href="{% url 'login' %}?next={{request.get_full_path|urlencode}}">로그인</a>
```

```
# views.py
redirect_path: str = ""

def login(request):
    if request.method == 'GET':
        global redirect_path
        redirect_path = request.GET.get('next', '')
        return render(request, 'registration/login.html')
    if request.method == 'POST':
    ...로그인 처리...
    return HttpResponseRedirect(redirect_path)
```



