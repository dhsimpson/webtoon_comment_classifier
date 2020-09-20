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
>>2. set을 이용해 중복 댓글을 제거했다. -> 총 4만7천 개 댓글
>>3. utf-16 인코딩 해 csv 파일로 저장했다.(이모티콘과 같은 특수문자의 경우, utf-8을 이용하면 범위 초과가 나온다.)

- requirements
  - Beautiful soup (html 문서 크롤링 대장)
  - Selenium (webDriver 기능이 있어 iframe 내에 형성된 html 문서를 크롤링 할 수 있다.)
  - chromium-chromedriver (크롬 브라우저에서 크롤링 시 셀레니움과 함께 사용됨)
  - Pandas (크롤링 된 데이터를 csv 파일로 저장)

>중간 노가다(1000개의 임의의 데이터를 직접 눈과 손으로 라벨링 했다. 친구가 인턴쉽 때 한달내내 라벨링만 했다는데, 왜 인턴 돈 주고 라벨링 시키는지 알게 됐다.(개발자가 직접 하기엔 진짜 고되다.))
>>크롤러에서 저장했던 csv 파일을 utf-16으로 저장했더니 내 컴터의 엑셀이 ',' 를 기준으로 데이터를 분리해주지 않는다.. 에라잇. 그래서 구글 스프레드시트로 작업함. 이 경우 엑셀로 저장이 되는데, csv로 저장하기가 안 되더라??
>>여튼 그런 이유로 머신러닝할 때는 pd.read_csv 가 아닌 pd.read_excel 이다.   

>댓글 분류기 머신러닝 모델
>>1. 전체 댓글로 vocab과 Tokenizer 를 만들어 준다. -> 음소 단위의 토큰화 했으며, 3417개 토큰 중 등장 빈도 기준으로 3200만 사용한다. (특수문자는 의미 없는 경우가 대부분 이므로)
>>2. 크롤링 된 데이터 중 라벨링 된 1000개로만 학습 시킨다. -> 직접 라벨링 할 때도 클래스 분류가 애매한 경우가 꽤나 있었다. (optimal error가 체감상 10% 정도)
>>3. 학습 된 모델을 이용해 나머지 4만6천 개의 댓글을 라벨링 한다. -> 데이터 라벨링이 정확도가 좀 떨어진다. (사실 4만 7천개 댓글이 이미 라벨링 된 상태였다면 2~3 과정은 없었겠지..)
>>4. 새로 모델을 생성하고 전체 데이터로 학습 시킨다. -> 1000 epoch 기준, 현재 dev set 정확도 86% 

- requirements
  - Pytorch (딥러닝 프레임워크)
  - sklearn (데이터를 학습+평가셋 으로 나눠줌, 실제는 SVM과 같은 여러 머신러닝도 지원하지만 여기선 데이터셋 split 용도로만 사용)
  - Pandas (데이터 불러오기, 저장하기)
  - Collections (문자열을 split & 빈도순으로 정렬해줌. 토크나이저, vocab 생성할 때 사용)
