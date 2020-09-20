# webtoon_comment_classifier
네이버 웹툰 댓글을 크롤링 하고, 머신러닝을 이용해 3개 클래스로 분류한다.
=

주제 : 
-
c.f.) 본 프로젝트는 KAU 4차산업혁명 공모전을 위한 ML 모델 Back End 개발과정 설명을 위한 쥬피터 노트북 입니다.
API 배포를 위해 모듈화 된 코드는 아래 레포지토리에서 볼 수 있습니다.(아직 모듈화 하지 않음)

[웹툰플랫폼] : https://github.com/gyu-young-park/WebToonPlatforn_DS

큰 기능은 두 가지로 구분된다.
-
>1. webtoon 서버로 부터 네이버 웹툰 댓글 크롤링 하기(NaverWebtoonCommentCrawler.ipynb)
>2. 댓글을 그림과 관련된 댓글, 내용과 관련된 댓글, 무의미한 댓글 세 클래스로 구분하는 머신러닝 모델 만들어 학습 시키기.(Naver_webtoon_learning.ipynb)

과정 설명(자세한 건 쥬피터 노트북 주석을 따라가면 된다. 주석 대신 마크다운을 사용하는 것은 추후에 수정)
-

>댓글 크롤러
>>1. comics 메인 페이지의 전체 웹툰 링크 크롤링 -> 웹툰별 첫 페이지의 모든 회차 링크 크롤링 -> 회차 별 댓글 크롤링 순으로 댓글을 크롤링 했다.
>>2. set을 이용해 중복 댓글을 제거했다.
>>3. utf-16 인코딩 해 csv 파일로 저장했다.(이모티콘과 같은 특수문자의 경우, utf-8을 이용하면 범위 초과가 나온다.)

-requirements
  - Beautiful soup (html 문서 크롤링 대장)
  - Selenium (webDriver 기능이 있어 iframe 내에 형성된 html 문서를 크롤링 할 수 있다.)
  - chromium-chromedriver (크롬 브라우저에서 크롤링 시 셀레니움과 함께 사용됨)
  - Pandas (크롤링 된 데이터를 csv 파일로 저장)



>댓글 분류기 머신러닝 모델
