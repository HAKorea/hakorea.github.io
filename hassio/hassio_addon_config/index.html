
<!doctype html>
  <!--[if lt IE 7]>      <html lang="en" class="no-js lt-ie9 lt-ie8 lt-ie7"> <![endif]-->
  <!--[if IE 7]>         <html lang="en" class="no-js lt-ie9 lt-ie8"> <![endif]-->
  <!--[if IE 8]>         <html lang="en" class="no-js lt-ie9"> <![endif]-->
  <!--[if gt IE 8]><!--> <html lang="en"> <!--<![endif]-->
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
    <title>Add-On Configuration - 홈어시스턴트</title>
    <meta name="author" content="Home Assistant">
    <meta name="description" content="Open source home automation that puts local control and privacy first.">
    
    <meta name="viewport" content="width=device-width">
    <link rel="canonical" href="https://hakorea.github.io/hassio/hassio_addon_config/">

    <meta property="fb:app_id" content="">
    <meta property="og:title" content="Add-On Configuration">
    <meta property="og:site_name" content="홈어시스턴트">
    <meta property="og:url" content="https://hakorea.github.io/hassio/hassio_addon_config/">
    <meta property="og:type" content="website">
    <meta property="og:description" content="Open source home automation that puts local control and privacy first.">
    <meta property="og:image" content="https://hakorea.github.io/images/default-social.png">
    
    <meta name="twitter:card" content="summary_large_image">
    <meta name="twitter:site" content="@">
    
    <meta name="twitter:title" content="Add-On Configuration">
    <meta name="twitter:description" content="Open source home automation that puts local control and privacy first.">
    <meta name="twitter:image" content="https://hakorea.github.io/images/default-social.png">

    <link href="/stylesheets/prism.css?8acb84bc0387c9548a63998fcfea0139" rel="stylesheet">
    <link href="/stylesheets/screen.css?0a23d70030bf362ca935e76ccc50922c" media="screen, projection, print" rel="stylesheet">
    <link href="/atom.xml" rel="alternate" title="홈어시스턴트" type="application/atom+xml">
    <link rel='shortcut icon' href='/images/favicon.ico' />
    <link rel='icon' type='image/png' href='/images/favicon-192x192.png' sizes='192x192' />
  </head>

  <body >

    <header class='site-header'>
      <div class="grid-wrapper">
  <div class="grid">
    <div class="grid__item three-tenths lap-two-sixths palm-one-whole ha-title">
      <a href="/" class="site-title">
        <img src="/images/home-assistant-logo.png" width="36" height="36" alt="Home Assistant">
        <span>홈어시스턴트</span>
      </a>
    </div>

    <div class="grid__item seven-tenths lap-four-sixths palm-one-whole">
      <nav>
        <input type="checkbox" id="toggle">
        <label for="toggle" class="toggle" data-open="Main Menu" data-close="Close Menu"></label>
        <ul class="menu pull-right">
            
            <li><a href="/getting-started/">시작하기</a></li>
            <li><a href="/integrations/">통합구성요소</a></li>
            <li><a href="/docs/">문서</a></li>
            <li><a href="/cookbook/">예제</a></li>
            <li>KOREAN(not official)</li>
        </ul>
      </nav>

      <div class='search-container' style='display: none'>
        <div class='search'>
          <i class="icon-search"></i>
          <input id='search' placeholder='Search the docs…'>
          <a href='#' class='close'><i class="icon-remove-sign"></i></a>
        </div>
      </div>
    </div>
  </div>
</div>

    </header>

    

    <div class="grid-wrapper">
      <div class="grid grid-center">
        
        <div class="grid__item two-thirds lap-one-whole palm-one-whole">
        

          
            <article class="page">
  
  
    

<div class="edit-github"><a href="https://github.com/hakorea/gitpages_source/blob/master/source/hassio/hassio_addon_config.markdown" rel="external nofollow">깃허브 편집</a></div>


  

  
  <header>
    <h1 class="title indent">
      Add-On Configuration
    </h1>
  </header>
  <hr class="divider">
  

  <p>각각의 애드온 폴더에는 일반적으로 다음과 같은 파일들이 들어있습니다:</p>
<pre><code class="language-text">addon_name/
  apparmor.txt
  build.json
  CHANGELOG.md
  config.json
  Dockerfile
  icon.png
  logo.png
  README.md
  run.sh
</code></pre>
<h2>
<a class="title-link" name="-" href="#-"></a> 애드온 스크립트</h2>
<p>모든 도커 컨테이너는 컨테이너를 시작할때 실행되는 스크립트가 존재합니다. 사용자가 다수의 애드온을 운영한다면 모든 일을 간단하게 처리하기 위해 Bash 스크립트를 사용하게 됩니다.</p>
<p>HA가 제공하는 모든 도커 이미지에는 기본적으로 <a href="https://github.com/hassio-addons/bashio" rel="external nofollow">bashio</a>가 들어있습니다. bashio는 애드온을 개발하는데 있어 편리한 기능과 코드의 중복을 줄이기 위해 사용하는 공통의 기능들을 미리 제작해 둔 스크립트입니다.</p>
<p>만일 직접 스크립트를 짠다면:</p>
<ul>
<li>
<code>/data</code> 폴더를 저장 공간으로 삼고</li>
<li>
<code>/data/options.json</code> 파일이 애드온의 설정옵션을 담고 있어서 bashio를 활용하여 처리하거나 <code>jq</code> 명령어로 데이터처리를 위한 쉘 스크립트를 만들면됩니다.</li>
</ul>
<p><code>options</code>에는 다음과 같은 설정이 있다고 가정하고:</p>
<pre><code class="language-json">{ "target": "beer" }
</code></pre>
<p>쉘 스크립트를 아래와 같이 만들면:</p>
<pre><code class="language-shell">CONFIG_PATH=/data/options.json

TARGET="$(jq --raw-output '.target' $CONFIG_PATH)"
</code></pre>
<p><code>TARGET</code>에는 <code>beer</code> 가 저장되어 이후 bash 스크립트로 사용할 수 있게 됩니다.</p>
<h2>
<a class="title-link" name="-" href="#-"></a> 애드온 도커파일</h2>
<p>모든 애드온은 알파인 리눅스 최신 버전을 기초로 만듭니다. Hass.io는 컴퓨터(machine architecture)에 따라 적당한 알파인 리눅스의 베이스 이미지를 사용합니다. 타임존의 경우 필요에 따라 <code>tzdata</code>를 설정할 수도 있으며 HA가 제공하는 베이스 이미지에는 타임존에 따라 미리 설정되어있습니다.</p>
<pre><code class="language-dockerfile">ARG BUILD_FROM
FROM $BUILD_FROM

ENV LANG C.UTF-8

# Install requirements for add-on
RUN apk add --no-cache jq

# Copy data for add-on
COPY run.sh /
RUN chmod a+x /run.sh

CMD [ "/run.sh" ]
</code></pre>
<p>만일 Hass.io가 설치된 기기에서 빌드를 하지 않거나 제공된 빌드 스크립트를 사용하지 않는다면(다른 컴퓨터에서 애드온을 만든다면), Dockerfile 안에는 다음과 같은 레이블을 포함시켜야 합니다:</p>
<pre><code>LABEL io.hass.version="VERSION" io.hass.type="addon" io.hass.arch="armhf|aarch64|i386|amd64"
</code></pre>
<p><code>build.json</code> 파일을 통해 직접 베이스 이미지를 사용할 수도 있고 다양한 아키텍쳐에 대한 지원이 필요없다면 토커파일의 <code>FROM</code>에 베이스 이미지에 대한 명칭을 바로 적용할 수도 있습니다.</p>
<h3>
<a class="title-link" name="build-args" href="#build-args"></a> Build Args</h3>
<p>다음과 같은 도커의 빌드 아규먼트를 사용할 수 있습니다:</p>
<table>
<thead>
<tr>
<th>ARG</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>BUILD_FROM</td>
<td>컴퓨터 시스템에 따라서 동적으로 베이스 이미지를 할당합니.</td>
</tr>
<tr>
<td>BUILD_VERSION</td>
<td>버전명 (<code>config.json</code> 파일에 명시된 버전).</td>
</tr>
<tr>
<td>BUILD_ARCH</td>
<td>현재 빌드하는 컴퓨터 시스템의 아키텍쳐명.</td>
</tr>
</tbody>
</table>
<h2>
<a class="title-link" name="-" href="#-"></a> 애드온 설정파일</h2>
<p>애드온에 대한 도커 설정은 <code>config.json</code>파일에 기록합니다.</p>
<div class="note">
괄호 쌍의 매칭과 컴마( , )의 위치(중괄호를 닫기 전 "}" 앞에는 컴마가 없음) 그리고 오타에 주의하세요.
</div>
<pre><code class="language-json">{
  "name": "xy",
  "version": "1.2",
  "slug": "folder",
  "description": "long description",
  "arch": ["amd64"],
  "url": "website with more information about add-on (ie a forum thread for support)",
  "startup": "application",
  "boot": "auto",
  "ports": {
    "123/tcp": 123
  },
  "map": ["config:rw", "ssl"],
  "options": {},
  "schema": {},
  "image": "repo/{arch}-my-custom-addon"
}
</code></pre>
<table>
<thead>
<tr>
<th>Key</th>
<th>Type</th>
<th>Required</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>name</td>
<td>string</td>
<td>yes</td>
<td>애드온 명칭</td>
</tr>
<tr>
<td>version</td>
<td>string</td>
<td>yes</td>
<td>애드온 버전 번호</td>
</tr>
<tr>
<td>slug</td>
<td>string</td>
<td>yes</td>
<td>애드온 저장 디렉토리명(소문자만 허용)</td>
</tr>
<tr>
<td>description</td>
<td>string</td>
<td>yes</td>
<td>애드온에 대한 상세 설명</td>
</tr>
<tr>
<td>arch</td>
<td>list</td>
<td>yes</td>
<td>지원하는 아키텍쳐 리스트: <code>armhf</code>, <code>armv7</code>, <code>aarch64</code>, <code>amd64</code>, <code>i386</code>.</td>
</tr>
<tr>
<td>machine</td>
<td>list</td>
<td>no</td>
<td>Default it support any machine type. You can select that this add-on run only on specific machines.</td>
</tr>
<tr>
<td>url</td>
<td>url</td>
<td>no</td>
<td>애드온의 웹사이트 주소. 애드온에 대한 설명과 옵션에 대한 설명 등을 기술한 홈페이지</td>
</tr>
<tr>
<td>startup</td>
<td>string</td>
<td>yes</td>
<td>
<code>initialize</code>은 Hass.io가 시작되기 전에 실행, <code>system</code> 은 데이터베이스와 같이 다른 프로그램과 독립적으로 동작, <code>services</code>는 홈어이스턴트가 시작되기 전에 실행, <code>application</code>은 홈어이스턴트가 시작된 이후에 실행, <code>once</code>는 데몬이 아닌 한번만 실행</td>
</tr>
<tr>
<td>webui</td>
<td>string</td>
<td>no</td>
<td>애드온이 내부 웹페이지를 갖고 있을때 접근할 주소. <code>http://[HOST]:[PORT:2839]/dashboard</code>와 같이 설정하는데 포트는 내부 포트로 이후 유효한 포트로 변경됩니다. proto라는 명칭으로 프로토콜 옵션을 config 파일에 설정할 수도 있습니다: <code>[PROTO:option_name]://[HOST]:[PORT:2839]/dashboard</code> 이 설정에 따라 해당 프로토콜을 연결하거나 <code>https</code>로 연결합니다.</td>
</tr>
<tr>
<td>boot</td>
<td>string</td>
<td>yes</td>
<td>시스템에서 자동 실행이면 <code>auto</code>, 사용자가 수동으로 실행하면 <code>manual</code>
</td>
</tr>
<tr>
<td>ports</td>
<td>dict</td>
<td>no</td>
<td>애드온 컨테이터가 외부로 연결되는 포트 설정. <code>"container-port/type": host-port</code>와 형식. 포트를 <code>null</code>로 설정하면 외부 연결 없음</td>
</tr>
<tr>
<td>ports_description</td>
<td>dict</td>
<td>no</td>
<td>네트워크 포트에 대한 상세 설명 형식은 <code>"container-port/type": "description of this port"</code>
</td>
</tr>
<tr>
<td>host_network</td>
<td>bool</td>
<td>no</td>
<td>이 설정을 true로 하면 애드온은 호스트 네트워크를 사용</td>
</tr>
<tr>
<td>host_ipc</td>
<td>bool</td>
<td>no</td>
<td>기본값은 false. IPC namespace 공유하는 옵션</td>
</tr>
<tr>
<td>host_dbus</td>
<td>bool</td>
<td>no</td>
<td>기본값은 false. 호스트의 dbus service를 애드온에 매핑</td>
</tr>
<tr>
<td>host_pid</td>
<td>bool</td>
<td>no</td>
<td>기본값은 false. 애드온 컨테이너가 호스트의 PID 네임스페이스를 사용하도록 허용. Work only for not protected add-ons.</td>
</tr>
<tr>
<td>devices</td>
<td>list</td>
<td>no</td>
<td>디바이스 리스트를 애드온에 매핑. 형식은  <code>&lt;path_on_host&gt;:&lt;path_in_container&gt;:&lt;cgroup_permissions&gt;</code>와 같다. 사용예: <code>/dev/ttyAMA0:/dev/ttyAMA0:rwm</code>
</td>
</tr>
<tr>
<td>udev</td>
<td>bool</td>
<td>no</td>
<td>기본값은 false. Set this True, if your container runs a udev process of its own.</td>
</tr>
<tr>
<td>auto_uart</td>
<td>bool</td>
<td>no</td>
<td>기본값은 false. 호스트의 UART/Serial 디바이스를 애드온에서 사용할 수 있게 허용</td>
</tr>
<tr>
<td>homeassistant</td>
<td>string</td>
<td>no</td>
<td>애드온이 동작하는 홈어시스턴트 최소 버전을 표기. <code>0.91.2</code>와 같은 버전 표기 문자열</td>
</tr>
<tr>
<td>hassio_role</td>
<td>str</td>
<td>no</td>
<td>기본값은 <code>default</code>. Hass.io API에 대한 접근 역할을 표기. 가능한 역할: <code>default</code>, <code>homeassistant</code>, <code>backup</code>, <code>manager</code>, <code>admin</code>.</td>
</tr>
<tr>
<td>hassio_api</td>
<td>bool</td>
<td>no</td>
<td>true로 설정하면 Hass.io REST API에 대한 접근을 허용. 호스트 명칭은 <code>hassio</code>. <code>http://hassio/api</code>.</td>
</tr>
<tr>
<td>homeassistant_api</td>
<td>bool</td>
<td>no</td>
<td>true로 설정하면  Home-Assistant REST API에 접근을 허용. <code>http://hassio/homeassistant/api</code>로 호출</td>
</tr>
<tr>
<td>docker_api</td>
<td>bool</td>
<td>no</td>
<td>애드온이 도커API에 대해 읽기 권한으로 접속 가능. Work only for not protected add-ons.</td>
</tr>
<tr>
<td>privileged</td>
<td>list</td>
<td>no</td>
<td>하드웨어나 시스템에 대한 접근을 허용. <code>NET_ADMIN</code>, <code>SYS_ADMIN</code>, <code>SYS_RAWIO</code>, <code>SYS_TIME</code>, <code>SYS_NICE</code>, <code>SYS_RESOURCE</code>, <code>SYS_PTRACE</code>, <code>SYS_MODULE</code>, <code>DAC_READ_SEARCH</code> 가능.</td>
</tr>
<tr>
<td>full_access</td>
<td>bool</td>
<td>no</td>
<td>도커의 privileged run 옵션과 동일하게 하드웨어에 대해 모든 접근 권한을 가짐. Work only for not protected add-ons.</td>
</tr>
<tr>
<td>apparmor</td>
<td>bool/string</td>
<td>no</td>
<td>Enable or disable AppArmor support. If it is enable, you can also use custom profiles with the name of the profile.</td>
</tr>
<tr>
<td>map</td>
<td>list</td>
<td>no</td>
<td>Hass.io의 기본 폴더들에 대한 접근을 허용.  <code>config</code>, <code>ssl</code>, <code>addons</code>, <code>backup</code>, <code>share</code> 폴더를 허용할 수 있으며 기본값은 읽기 전용인 <code>ro</code>이고 <code>:rw</code>를 붙여 읽기/쓰기로 설정 가능</td>
</tr>
<tr>
<td>environment</td>
<td>dict</td>
<td>no</td>
<td>A dict of environment variable to run add-on.</td>
</tr>
<tr>
<td>audio</td>
<td>bool</td>
<td>no</td>
<td>애드온이 내장 오디오 사용여부를 설정. The ALSA configuration for this add-on will be mount automatic.</td>
</tr>
<tr>
<td>gpio</td>
<td>bool</td>
<td>no</td>
<td>true로 설정하면 커널의 GPIO 인터페이스가 애드온의 <code>/sys/class/gpio</code> 매핑되어 사용 가능. Some library need also <code>/dev/mem</code> and <code>SYS_RAWIO</code> for read/write access to this device. On system with AppArmor enabled, you need disable AppArmor or better for security, provide you own profile for the add-on.</td>
</tr>
<tr>
<td>devicetree</td>
<td>bool</td>
<td>no</td>
<td>Boolean. If this is set to True, <code>/device-tree</code> will map into add-on.</td>
</tr>
<tr>
<td>kernel_modules</td>
<td>bool</td>
<td>no</td>
<td>Map host kernel modules and config into add-on (readonly).</td>
</tr>
<tr>
<td>stdin</td>
<td>bool</td>
<td>no</td>
<td>Boolean. If that is enable, you can use the STDIN with Hass.io API.</td>
</tr>
<tr>
<td>legacy</td>
<td>bool</td>
<td>no</td>
<td>Boolean. If the docker image have no hass.io labels, you can enable the legacy mode to use the config data.</td>
</tr>
<tr>
<td>options</td>
<td>dict</td>
<td>yes</td>
<td>애드온의 옵션 값들을 설정</td>
</tr>
<tr>
<td>schema</td>
<td>dict</td>
<td>yes</td>
<td>옵션의 값을 검사하는 스키마를 설정. It can be <code>False</code> to disable schema validation and use custom options.</td>
</tr>
<tr>
<td>image</td>
<td>string</td>
<td>no</td>
<td>For use with Docker Hub and other container registries.</td>
</tr>
<tr>
<td>timeout</td>
<td>integer</td>
<td>no</td>
<td>Default 10 (second). The timeout to wait until the docker is done or will be killed.</td>
</tr>
<tr>
<td>tmpfs</td>
<td>string</td>
<td>no</td>
<td>Mount a tmpfs file system in <code>/tmpfs</code>. Valide format for this option is : <code>size=XXXu,uid=N,rw</code>. Size is mandatory, valid units (<code>u</code>) are <code>k</code>, <code>m</code> and <code>g</code> and <code>XXX</code> has to be replaced by a number. <code>uid=N</code> (with <code>N</code> the uid number) and <code>rw</code> are optional.</td>
</tr>
<tr>
<td>discovery</td>
<td>list</td>
<td>no</td>
<td>A list of services they this Add-on allow to provide for Home Assistant. Currently supported: <code>mqtt</code>
</td>
</tr>
<tr>
<td>services</td>
<td>list</td>
<td>no</td>
<td>A list of services they will be provided or consumed with this Add-on. Format is <code>service</code>:<code>function</code> and functions are: <code>provide</code> (this add-on can provide this service), <code>want</code> (this add-on can use this service) or <code>need</code> (this add-on need this service to work correctly).</td>
</tr>
<tr>
<td>auth_api</td>
<td>bool</td>
<td>no</td>
<td>Allow access to Home Assistent user backend.</td>
</tr>
<tr>
<td>ingress</td>
<td>bool</td>
<td>no</td>
<td>애드온의 인그레스 기능을 사용 가능하게 설정</td>
</tr>
<tr>
<td>ingress_port</td>
<td>integer</td>
<td>no</td>
<td>기본 포트는 <code>8099</code>. For Add-ons they run on host network, you can use <code>0</code> and read the port later on API.</td>
</tr>
<tr>
<td>ingress_entry</td>
<td>string</td>
<td>no</td>
<td>기본 실행 URL은 <code>/</code>. 실행 URL을 설정</td>
</tr>
<tr>
<td>panel_icon</td>
<td>string</td>
<td>no</td>
<td>패널에 나타나는 기본 아이콘은 mdi:puzzle. MDI icon for the menu panel integration.</td>
</tr>
<tr>
<td>panel_title</td>
<td>string</td>
<td>no</td>
<td>기본은 애드온 명칭을 사용</td>
</tr>
<tr>
<td>panel_admin</td>
<td>bool</td>
<td>no</td>
<td>Default True. Make menu entry only available with admin privileged.</td>
</tr>
<tr>
<td>snapshot_exclude</td>
<td>list</td>
<td>no</td>
<td>List of file/path with glob support they are excluded from snapshots.</td>
</tr>
</tbody>
</table>
<h3>
<a class="title-link" name="options--schema" href="#options--schema"></a> Options / Schema</h3>
<p><code>config.json</code>파일의 <code>options</code>은 애드온 안에서 <code>/data/options.json</code> 파일에 저장됩니다. 애드온을 실행하기전 사용자가 값을 입력해야 하고 기본값은 <code>null</code>로 초기화됩니다. nested arrays나 딕셔너리는 2단계까지만 허용됩니다. 옵션을  필수값이 아닌 것으로 설정할때는 스키마에서 데이터 형식 끝에 <code>?</code>를 표시합니다. 물음표가 없는 스키마의 옵션 값은 필수로 지정해야 하며 데이터 형식이 일치해야 합니다.</p>
<pre><code class="language-json">{
  "message": "custom things",
  "logins": [
    { "username": "beer", "password": "123456" },
    { "username": "cheep", "password": "654321" }
  ],
  "random": ["haha", "hihi", "huhu", "hghg"],
  "link": "http://example.com/",
  "size": 15,
  "count": 1.2
}
</code></pre>
<p><code>options</code>의 <code>schema</code>는 아래와 같으며 사용자가 설정한 값이 유효한지 여부를 체크합니다:</p>
<pre><code class="language-json">{
  "message": "str",
  "logins": [
    { "username": "str", "password": "str" }
  ],
  "random": ["match(^\w*$)"],
  "link": "url",
  "size": "int(5,20)",
  "count": "float",
  "not_need": "str?"
}
</code></pre>
<p>스키마에 설정 가능한 데이터 형식:</p>
<ul>
<li>str / str(min,) / str(,max) / str(min,max)</li>
<li>bool</li>
<li>int / int(min,) / int(,max) / int(min,max)</li>
<li>float / float(min,) / float(,max) / float(min,max)</li>
<li>email</li>
<li>url</li>
<li>port</li>
<li>match(REGEX)</li>
<li>list(val1|val2|…)</li>
</ul>
<h2>
<a class="title-link" name="add-on-extended-build" href="#add-on-extended-build"></a> Add-on extended build</h2>
<p>애드온의 추가적인 빌드옵션은 <code>build.json</code> 파일에 설정합니다. 이 파일은 해쇼의 빌드 시스템이 확인하고 빌드시 사용합니다. 제공하는 기본 이미지가 아닌 다른 베이스 이미지를 사용한다면 아래와 같이 설정하세요.</p>
<pre><code class="language-json">{
  "build_from": {
    "armhf": "mycustom/base-image:latest"
  },
  "squash": false,
  "args": {
    "my_build_arg": "xy"
  }
}
</code></pre>
<table>
<thead>
<tr>
<th>Key</th>
<th>Required</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>build_from</td>
<td>no</td>
<td>A dictionary with the hardware architecture as the key and the base Docker image as value.</td>
</tr>
<tr>
<td>squash</td>
<td>no</td>
<td>Default <code>False</code>. Be carfully with this option, you can not use the image for caching stuff after that!</td>
</tr>
<tr>
<td>args</td>
<td>no</td>
<td>Allow to set additional Docker build arguments as a dictionary.</td>
</tr>
</tbody>
</table>
<p>해쇼가 제공하는 <a href="https://github.com/home-assistant/hassio-base" rel="external nofollow">베이스 이미지</a>는 다양한 기능을 포함하고 있으며, 만일 다른 알파인 리눅스 버전을 쓰고 싶거나 특정 이미지를 사용하려면 <code>build_from</code> 옵션에 명시적으로 작성하면 됩니다.</p>
<h3><a href="/hassio/hassio_addon_communication/">다음 과정: 통신 방법 »</a></h3>


</article>

          
        </div>

        
        <aside id="sidebar" class="grid__item one-third lap-one-whole palm-one-whole">
          <div class="grid">
  
  
    <section class="aside-module grid__item one-whole lap-one-half">
  <div class='section'>
    <h1 class="title delta">목차</h1>
    <ul class='divided sidebar-menu'>
      <li>
        <a  href='/hassio/'>Hass.io </a>
        <ul>
          <li><a  href='/hassio/installation/'>설치 </a></li>
          <li><a  href='/addons/'>애드온 </a></li>
          <li><a  href='/hassio/installing_third_party_addons/'>서드파티 애드온 </a></li>
        </ul>
      </li>
    </ul>
    <ul class='divided sidebar-menu'>
      <li>
        고급 설정
        <ul>
          <li><a  href='/hassio/commandline/'>명령어 모드 </a></li>
          <li><a  href='/hassio/zwave/'>Z-Wave </a></li>
          <li><a  href='/hassio/enable_i2c/'>I2C 사용 </a></li>
        </ul>
      </li>
    </ul>
    <ul class='divided sidebar-menu'>
      <li><a  href='/hassio/developing_addon/'>애드온을 제작하려면? </a>
        <ol>
          <li><a  href='/hassio/hassio_addon_tutorial/'>무작정 따라해보기 </a></li>
          <li><a class='active' href='/hassio/hassio_addon_config/'>애드온 설정 </a></li>
          <li><a  href='/hassio/hassio_addon_communication/'>통신 방법 </a></li>
          <li><a  href='/hassio/hassio_addon_testing/'>테스트 </a></li>
          <li><a  href='/hassio/hassio_addon_publishing/'>배포하기 </a></li>
          <li><a  href='/hassio/hassio_addon_presentation/'>홍보하기 </a></li>
          <li><a  href='/hassio/hassio_addon_repository/'>저장소 관리 </a></li>
          <li><a  href='/hassio/hassio_addon_security/'>보안 검수 </a></li>
        </ol>
      </li>
    </ul>
  </div>
</section>

  
</div>

        </aside>
        
      </div>
    </div>

    <footer>
      <div class="grid-wrapper">
  <div class="grid">
    <div class="grid__item">
      <div class="copyright grid">
        <div class='company grid__item one-third lap-one-half palm-one-whole'>
          <div class="title">
            <img src="/images/home-assistant-logo.png" width="36" height="36" alt="Home Assistant"> 홈어시스턴트
          </div>
        </div>

        <div class='grid__item one-third lap-one-half palm-one-whole'>
          <ul>
            <li><a href='https://alerts.home-assistant.io'>Home Assistant Alerts</a></li>
            <li><a href='https://cafe.naver.com/koreassistant'>HA 네이버 카페</a></li>
          </ul>
        </div>

        <div class='grid__item one-third lap-one-half palm-one-whole'>
          이 웹사이트는 <a href='https://jekyllrb.com/'>Jekyll</a>과
          <a href='https://github.com/coogie/oscailte'>Oscalite 테마</a>를 사용합니다.
        </div>
      </div>
    </div>
  </div>
</div>

    </footer>

    <script>
var _gaq=[['_setAccount',''],['_trackPageview']];
(function(d,t){var g=d.createElement(t),s=d.getElementsByTagName(t)[0];
g.src=('https:'==location.protocol?'//ssl':'//www')+'.google-analytics.com/ga.js';
s.parentNode.insertBefore(g,s)}(document,'script'));
</script>

<script src="/javascripts/prism.js?5d6619066a1fc5cd819a93c132b539ac" type="text/javascript"></script>

<link rel="stylesheet" href="https://cdn.jsdelivr.net/docsearch.js/2/docsearch.min.css" />
<script type="text/javascript" src="https://cdn.jsdelivr.net/docsearch.js/2/docsearch.min.js"></script>
<script type="text/javascript">
docsearch({
  apiKey: 'ae96d94b201c5444c8a443093edf3efb',
  indexName: 'home-assistant',
  inputSelector: '#search',
  debug: false // Set debug to true if you want to inspect the dropdown
});
document.querySelector('.search .close').addEventListener('click', function(ev) {
  ev.preventDefault();
  document.querySelector('.search-container').style.display = 'none';
});
document.querySelector('.show-search').addEventListener('click', function(ev) {
  ev.preventDefault();
  document.querySelector('.search-container').style.display = 'block';
  document.getElementById('toggle').checked = false;
  document.querySelector('.search-container input').focus();
});
</script>

  </body>
</html>
