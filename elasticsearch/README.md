### 엘라스틱서치 기초

- 엘라스틱서치는 루씬을 기반으로 만들어진 검색엔진이다. 데이터를 분석하고 저장하는 과정(색인)에서 역인덱스(inverted index)를 만들어 검색에 용이한 정보들을 만든다.
- 역인덱스 구조를 만드는데 오래 걸리지 않고, 역인덱스를 통해 빠른 검색을 제공하는게 엘라스틱서치의 검색엔진으로써의 장점이다.
- 색인
    - 엘라스틱서치에 데이터 저장하는 과정에서는 “분석 + 가공 + 저장” 이 이루어지기에 색인한다는 표현을 사용한다.
- 역인덱스
    - RDBMS에서 일반적으로 사용되는 데이터를 찾는 방식은 문서(row)가 원하는 데이터를 갖고있는지 조회하는 방식으로 이루어진다.
        - A가 "hello"를 갖고있는가? O -> B가 "hello"를 갖고있는가? X -> C가 "hello"를 갖고있는가? O -> A와 C 반환
        - 이 방법의 단점은 데이터가 늘어날수록 조회(검색) 대상이 늘어난다는 것이다.
    - 역인덱스 구조는 데이터를 가진 문서 정보를 저장해두는 구조다
        - "hello"를 갖고있는 문서 목록 - A, C를 미리 구성해 둠
        - "hello" 조회 -> A, C 반환
    - 이러한 특징으로 인해 데이터가 늘어나도 빠르게 검색이 가능해진다.

엘라스틱서치는 7.11 버전에서 라이센스가 변경되었다.

- AWS 등 매니지드 데이터베이스를 제공하는 곳에서는 OpenSearch가 사용된다. OpenSearch는 엘라스틱서치 라이센스가 변경되기 전 마지막 버전에서 fork한 프로젝트다.
- SaaS 사용자에게는 영향이 없다.
- Apache 2.0 → SSPL
- https://www.elastic.co/kr/pricing/faq/licensing

### 엘라스틱서치 Analyzer 구조

- Character filter(전처리), Tokenizer(분리), Token filter(후처리) 세 가지 블록으로 구성된다.
- Character filter
    - 문자 스트림이 tokenizer로 이동하기 전 전처리 역할
    - HTML 제거, 문자 매핑, 패턴 문자 변경
        - 예를 들어 “dkssud”을 “안녕“으로 변환하는 작업, <b>안녕</b>에서 HTML 태그를 제거하는 작업 등이 가능하다.
    - 0개 이상의 필터가 존재 가능하며 순차 적용된다.
- Tokenizer
    - 문자 스트림을 개별 토큰(term)으로 분리하는 역할
    - 많이 사용되는 Tokenizer로 whitespace가 있다 (공백 문자 기준 분리)
        - 예) “Hello world” → “Hello”, “world”
    - 1개 존재해야 한다.
- Token filter
    - tokenizer 완료 후 후처리
    - 토큰의 변환/추가/제거 작업을 수행한다. 대표적인 토큰 필터로 N-gram이 있다.
        - N에 해당하는 숫자만큼 단어로 분리한다. min, max 설정이 가능하며 “quick”이 “qu”, “ui”로 검색 가능해진다.
        - 2-gram 예) quick → qu, ui, ic, ck
    - 0개 이상의 필터가 존재 가능하며 순차 적용된다.
- Token filter와 Character filter 필터와의 차이점은 실행 시점(전/후처리)에서 오는 대상, 즉 “전체 텍스트“이냐, “token”이냐 대상의 차이다.
    - 토큰 필터는 토큰 또는 토큰을 구성하는 문자의 순서 변경이 불가능하다
- 한 줄 정리하면 Analyzer는 여러 필터와 한 개의 토크나이저를 조합해서 만들 수 있으며 문자열을 입맛에 맞게 가공한다.

### 엘라스틱서치 검색 - term과 match 쿼리

- term 쿼리
    - 엘라스틱서치는 검색 또는 저장을 수행하기 전에 데이터를 분석한다. 이 term 쿼리에서는 분석과정을 수행하지 않고 "완전일치"에 대한 검색을 수행한다.
    - 즉 term 쿼리를 사용해서 "He"를 검색하면 "Hello World"라는 값을 가진 문서는 검색되지 나오지 않는다.
    - 만약 "Hello World"를 가진 필드가 n-gram 분석기를 통해 색인되었다면 "He"를 검색했을 때 결과가 나온다.
        - [Hello World] -> [He], [el], [ll], [lo] ..... 등으로 분석해서 저장되었기에 "He" 완전일치하는 항목에 해당한다.
- match 쿼리
    - 개인적인 생각으로 엘라스틱서치의 메인 이벤트같은 기능이다.
    - match 쿼리에 담긴 검색어를 분석해서 검색을 시도하고 결과를 반환한다.
        - "Hello"를 검색하면 "Hello World"를 가진 문서를 얻을 수 있다.
    - 쿼리 결과에는 score라는 개념이 존재한다. 다양한 기준(검색어 등장 횟수, 문서의 길이 등)을 통해 계산되는데, 이 스코어가 높은 순으로 문서가 반환된다.
        - match 는 완전일치하는 문서를 찾는 개념이 아니기에 연관이 적은 결과가 나올 수 있다.
- 두 줄 정리
    - term 쿼리는 완전일치하는 문서를 찾는다. (분석기를 거치지 않고, 입력된 검색어를 그대로 사용하여 검색)
    - match 쿼리는 검색어를 분석해서 검색한다. (연관성이 높은 문서를 찾는다)
