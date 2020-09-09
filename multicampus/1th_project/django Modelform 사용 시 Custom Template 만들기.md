- django Modelform 사용 시 Custom Template 만들기

```
# forms.py
class UserForm(forms.ModelForm):
    class Meta:
        model = User
        fields = (
            'first_name',
            'email', 'username', 'password')
        labels = {
            "first_name": "이름",
            "email": "이메일",
            "username": "아이디",
            "password": "비밀번호"
        }
        widgets = {
            "first_name": forms.TextInput,
            "email": forms.EmailInput,
            "username": forms.TextInput,
            "password": forms.PasswordInput
        }
```

CSS Style 적용을 위한 django-widget-tweaks 설치

```
$ pip install django-widget-tweaks
```

Settings.py에 추가

```
INSTALLED_APPS = [
    ...
    'widget_tweaks',
    ...
]
```

```
# signup.html
{% load widget_tweaks %}
...
# input100 이라는 class의 css 적용
{{user_form.first_name |add_class:"input100" }}
```

