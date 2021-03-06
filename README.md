drf-docs-from-tests
===================

API documentation generator based on tests


## INSTALL ##

```sh
$ pip install djangorestframework drf-docs-from-tests
$ mkdir docs
```

```python
INSTALLED_APPS = (
    ...
    'rest_framework',
    'docsfromtests'
)

TEST_RUNNER = 'docsfromtests.testcase.DocDiscoverRunner'
```


## SETTINGS ##

Template for wiki-page (default: gitlab.tpl)

```DOCSFROMTESTS_BASE_TEMPLATE = 'gitlab.tpl'```

Template for request-response (default: gitlab_request.tpl)

```DOCSFROMTESTS_REQUEST_TEMPLATE = 'gitlab_request.tpl'```

Path for saving generated docs (default: docs)

```DOCSFROMTESTS_DOCS_PATH = 'docs'```

Dict of extra request kwargs (SERVER_NAME='example.com')

```DOCSFROMTESTS_REQUEST_KWARGS = {}```

Format of generated doc (default: md)

```DOCSFROMTESTS_FILE_FORMAT = 'md'```


## USAGE ##

Write test using DocAPITestCase.

example/apps/myapp/tests.py:

```python
# -*- coding: utf-8 -*-

from django.core.urlresolvers import reverse
from rest_framework import status
from rest_framework.test import APITestCase
from docsfromtests.testcase import DocAPITestCase


class UserTests(DocAPITestCase):
    caption = u'Header of the page'
    description = u'Text for some explains'

    def test_get_users(self):
        u''' Header of the section about getting user data '''
        self.doc(u'Some extra text')
        url = reverse('user-list')
        response = self.client.get(url, format='json')
        self.assertEqual(response.status_code, status.HTTP_200_OK)

    def test_create_user(self):
        u''' Header of the section about creating users '''
        url = reverse('user-list')
        data = {'username': 'test', 'password': '123'}
        response = self.client.post(url, data, format='json')
        self.assertEqual(response.status_code, status.HTTP_201_CREATED)
```

```sh
$ python manage.py test apps.myapp
```

docs/apps_myapp_tests_UserTests.md:
```
Header of the page


Text for some explains


- - -



##  Header of the section about getting user data  ##

Some extra text


**Request**

&gt; **CONTENT_LENGTH:** 4
&gt;
&gt; **CONTENT_TYPE:** application/json
&gt;
&gt; **REQUEST_METHOD:** GET
&gt;
&gt; **PATH_INFO:** /user
&gt;
&gt; **SERVER_PORT:** 80
&gt;
&gt; **BODY**:

```
!#json


```


**Response**

&gt; **Status:** 200
&gt;
&gt; **BODY**:

```
!#json


```



- - -


##  Header of the section about creating users  ##


**Request**

&gt; **CONTENT_LENGTH:** 36
&gt;
&gt; **CONTENT_TYPE:** application/json; charset=None
&gt;
&gt; **REQUEST_METHOD:** POST
&gt;
&gt; **PATH_INFO:** /user
&gt;
&gt; **SERVER_PORT:** 80
&gt;
&gt; **BODY**:

```
!#json

{
&quot;password&quot;: &quot;123&quot;,
&quot;username&quot;: &quot;test&quot;
}
```


**Response**

&gt; **Status:** 201
&gt;
&gt; **BODY**:

```
!#json

{
&quot;password&quot;: &quot;123&quot;,
&quot;username&quot;: &quot;test&quot;
}
```



- - -


Autogenerated at 2015-09-14 11:31
```
