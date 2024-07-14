---
title: "Python 패키지 관리를 위한 로컬 PyPi 저장소 서버 설정 방법"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-HowtoSetUpYourOwnLocalPyPiRepositoryServerforPythonPackageManagement_0.png"
date: 2024-07-14 23:39
ogImage: 
  url: /TIL/assets/img/2024-07-14-HowtoSetUpYourOwnLocalPyPiRepositoryServerforPythonPackageManagement_0.png
tag: Tech
originalTitle: "How to Set Up Your Own Local PyPi Repository Server for Python Package Management"
link: "https://medium.com/@tamirsuliman/how-to-set-up-your-own-local-pypi-repository-server-for-python-package-management-2d0f33c12c5e"
---


<img src="/TIL/assets/img/2024-07-14-HowtoSetUpYourOwnLocalPyPiRepositoryServerforPythonPackageManagement_0.png" />

파이썬 패키지 저장소는 파이썬 패키지를 호스팅하여 개발자가 라이브러리를 게시하고 배포할 수 있는 중앙 웹 사이트입니다. PyPI(Python Package Index)를 호스팅하거나 미러링하여 네트워크 내에서 Python 패키지에 빠르고 신뢰할 수 있는 액세스를 제공하는 것이 중요합니다. 특히 네트워크 대역폭이 제한되었거나 인터넷 연결이 불안정한 환경에서는 더욱 중요합니다.

# 전제 조건

- 관리 권한이 있는 Red Hat 또는 Rocky Linux 8 시스템이 있어야 합니다.
- 필요한 패키지를 다운로드하려면 인터넷에 액세스해야 합니다.
- PyPI 저장소를 위한 충분한 저장 공간(1TB)이 필요할 수 있으며, 해당 패키지를 저장할 데이터 파티션을 설정할 수 있습니다.

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

# 실행 단계

— Bandersnatch 사용

- 필수 패키지 설치: python3, pip, virtualenv이 설치되어 있는지 확인해주세요.

```bash
sudo dnf install python2 python3 python3-pip python3-virtualenv vim -y
python3 -m venv venv
source venv/bin/activate
```

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

2. Bandersnatch 설치: Bandersnatch는 PyPI를 미러링하는 도구입니다.

```js
pip install --upgrade pip
pip install dataclasses
pip install bandersnatch
```

3. Bandersnatch 구성: 초기 구성 파일을 생성하고 편집하세요.

a. 구성 파일을 생성하려면 처음으로 bandersnatch 미러 명령을 실행하십시오.

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
밴더스내치 미러
2024–07–14 00:04:36,297 경고: 설정 파일 ‘/etc/bandersnatch.conf’이 누락되어 기본 설정이 생성되었습니다.
2024–07-14 00:04:36,297 경고: 설정 파일을 검토한 후 ‘bandersnatch’를 다시 실행해주세요.
```

b. 거울 데이터를 저장할 디렉토리에 대한 구성 파일 매개변수를 업데이트하세요. 제 경우에는 데이터 파티션을 만드는 것이죠. 여러분의 설정은 다를 수 있어요.

```js
vim /etc/bandersnatch.conf

...
directory = /data/
...
```

파일을 저장하세요.


<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

명령을 다시 실행하면 bandersnatch mirror가 실행됩니다.

```js
bandersnatch mirror
2024-07-14 00:09:40,793 INFO: 선택된 스토리지 백엔드: 파일 시스템
2024-07-14 00:09:40,860 ERROR: swift_plugin = bandersnatch_storage_plugins.swift:SwiftStorage 진입점 로드 불가능: 'keystoneauth1' 모듈을 찾을 수 없음
2024-07-14 00:09:40,911 INFO: 미러 디렉토리 설정 중: /data/web/simple
2024-07-14 00:09:40,912 INFO: 미러 디렉토리 설정 중: /data/web/packages
2024-07-14 00:09:40,913 INFO: 미러 디렉토리 설정 중: /data/web/local-stats/days
2024-07-14 00:09:40,913 INFO: 생성 파일이 누락되었습니다. 상태 파일을 다시 초기화합니다.
2024-07-14 00:09:40,914 INFO: status 파일 /data/status 누락됨. 처음부터 다시 시작합니다.
2024-07-14 00:09:40,914 INFO: https://pypi.org와 동기화 중.
2024-07-14 00:09:40,914 INFO: 현재 미러 일련번호: 0
2024-07-14 00:09:40,914 INFO: 모든 패키지 동기화 중.
```

명령이 다시 실행되어 모든 패키지가 pypi 웹사이트에서 다운로드됩니다.

이 데이터를 웹 서버로 복사하거나 웹 서버를 설정해보세요.

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

4. 웹 서버 설치: 아파치 httpd 웹 서버를 설치합니다.

```js
sudo dnf install httpd
sudo systemctl start httpd
sudo systemctl enable httpd
```

5. 웹 서버 구성: 웹 서버를 PyPI 미러가 저장된 디렉토리로 지정합니다.

```js
sudo vim /etc/httpd/conf.d/pypi_repo.conf

Alias /pypi /var/www/html/pypi
<Directory /var/www/html/pypi>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
```

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
sudo systemctl restart httpd
```

6. 클라이언트가 로컬 저장소를 사용하도록 구성하기: 클라이언트 기기에서 pip를 사용하여 로컬 PyPI 미러를 사용하도록 구성합니다.

```js
mkdir -p ~/.config/pip
cat << EOF > ~/.config/pip/pip.conf
[global]
index-url = http://mywebserver.example.com/pypi/simple
EOF
```

Bandersnatcher의 문제는 선택적 미러링을 할 수 없다는 것입니다. 왜냐하면 pypi 서버로부터 전체 내용을 가져오기 때문입니다. 이 문제를 해결하기 위해 개발자가 모든 선택적 다운로드를 위한 라이브러리 minirepo를 제공했고, 이를 아래에서 나열해보겠습니다.

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

— Minirepo과 pypi-server를 사용 중

안녕하세요! 라즈베리 파이를 설정한 사람의 정보를 바탕으로 이 라이브러리들을 테스트하고 있는데, 재미있는 일이 있었어요. 여러 오류를 마주쳤지만 그 덕분에 코드에서 버그를 발견하고 고칠 수 있었어요. 저의 수정 사항은 여기에서 확인할 수 있어요. 필요하시면 제 수정 사항을 자유롭게 적용해 보세요.

- Python3 호환성 문제
- XML 구문 분석 오류 문제

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

파이썬 파이프 파이프 파프리리바 리프 파이사이사이 사이브

이제 처음으로 minirepo를 실행하면 json 형식의 .minirepo 파일 이름을 요청합니다.

```js
minirepo

/******** Minirepo ********/
Traceback (most recent call last):
 File "/root/venv/lib64/python3.6/site-packages/minirepo.py", line 206, in get_config
 config = json.load(open(config_file))
FileNotFoundError: [Errno 2] No such file or directory: '/root/.minirepo'
```

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

위의 텍스트를 친근한 톤으로 한국어로 번역하면 다음과 같습니다.

```js
파일을 생성해 봅시다.

cat << EOF > /root/.minirepo
{
  "processes": 10,
  "package_types": [
    "bdist_egg",
    "bdist_wheel",
    "sdist"
  ],
  "extensions": [
    "bz2",
    "egg",
    "gz",
    "tgz",
    "whl",
    "zip"
  ],
  "python_versions": [
    "3.0",
    "3.1",
    "3.2",
    "3.3",
    "3.4.10",
    "3.5.7",
    "3.6.9",
    "any",
    "cp27",
    "py2",
    "py2.py3",
    "py27",
    "source"
  ],
  "repository": "/data"
}
EOF

이제 실행해 봅시다.

minirepo 

(venv) [root@localhost ~]# minirepo 
/******** Minirepo ********/
extensions      = ['bz2', 'egg', 'gz', 'tgz', 'whl', 'zip']
package_types   = ['bdist_egg', 'bdist_wheel', 'sdist']
processes       = 10
python_versions = ['3.0', '3.1', '3.2', '3.3', '3.4.10', '3.5.7', '3.6.9', 'any', 'cp27', 'py2', 'py2.py3', 'py27', 'source']
repository      = /data
설정 파일 사용 중 /root/.minirepo

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

파이썬 코드를 이용하여 pypi 웹사이트를 파싱하고 패키지를 다운로드할 것입니다.

.......
[zzzutils 다운로드](/simple/zzzutils/)
[zzz-web 다운로드](/simple/zzz-web/)
[zzzymobbe 다운로드](/simple/zzzymobbe/)
[zzzz 다운로드](/simple/zzzz/)
[zzzZZZzzz 다운로드](/simple/zzzzzzzzz/)
</body>
</html>
/tmp/tmpvt3vfas8/worker.2565 파일에서 워커 시작...
/tmp/tmpvt3vfas8/worker.2566 파일에서 워커 시작...
/tmp/tmpvt3vfas8/worker.2567 파일에서 워커 시작...
/tmp/tmpvt3vfas8/worker.2568 파일에서 워커 시작...
/tmp/tmpvt3vfas8/worker.2569 파일에서 워커 시작...
/tmp/tmpvt3vfas8/worker.2570 파일에서 워커 시작...
/tmp/tmpvt3vfas8/worker.2571 파일에서 워커 시작...
/tmp/tmpvt3vfas8/worker.2572 파일에서 워커 시작...
/tmp/tmpvt3vfas8/worker.2573 파일에서 워커 시작...
/tmp/tmpvt3vfas8/worker.2574 파일에서 워커 시작...
2024-07-14 12:58:06,098:경고: 다운로드 완료: jira_test-0.0.1.tar.gz                             Ok pid:2567 0% [1/55510.0]
2024-07-14 12:58:06,127:경고: 다운로드 완료: nvitop-1.3.2.tar.gz                                Ok pid:2565 0% [1/55509.0]

이제 PyPi 패키지를 로컬 저장소에 복제했으며, 클라이언트에 설치된 pip를 통해 해당 패키지를 직접 설치할 수 있습니다. 이제 pypiserver가 등장합니다. pypiserver는 로컬 패키지 인덱스를 제공하여 네트워크를 통해 저장소에서 패키지를 찾을 수 있도록 하는 역할을 합니다.

우리는 도커를 사용하여 pypi 서버를 배포할 것입니다.

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

services:
  pypi-server:
    image: pypiserver/pypiserver:latest
    ports:
      - 8082:8080
    volumes:
      - type: volume
        source: pypi-server
        target: /data
    command: -P . -a . /data/
    restart: always
volumes:
  pypi-server:

![How to Set Up Your Own Local PyPi Repository Server for Python Package Management](/TIL/assets/img/2024-07-14-HowtoSetUpYourOwnLocalPyPiRepositoryServerforPythonPackageManagement_1.png)

![Reference](https://miro.medium.com/v2/resize:fit:960/0*6KpBJVtYreNzCL3G.gif)

# 참고 문헌

<!-- TIL 수평 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

[1] https://bandersnatch.readthedocs.io/en/latest/

[2] https://vuyisile.com/pypi-in-a-box-using-a-raspberry-pi-as-a-portable-pypi-server/