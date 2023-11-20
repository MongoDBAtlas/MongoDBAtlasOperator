<img src="https://companieslogo.com/img/orig/MDB_BIG-ad812c6c.png?t=1648915248" width="50%" title="Github_Logo"/> <br>


# MongoDB Atlas Hands-on Training

## Index and Aggreagation
생성한 컬렉션에 인덱스를 생성하여 빠른 데이터 엑세스가 되는 것을 확인 합니다.

### [&rarr; Index on Movies](#Index)

<br>


### Index

sample_mflix.movies 에서 2000년 이후에 개봉된 영화 중 "Bill Murray"가 출연한 영화 리스트를 검색 하고 제목 순서로 출력 합니다.   

Compass에서 movies 컬렉션을 선택 하고 Explain Plan 에서 실행 합니다.   
````
db.movies.find(
	{
    	"cast":"Bill Murray",
    	"year":{$gte:2000}
	}
).sort(
	{"title":1}
)
````
<img src="/03.Index and tips/images/image01.png" width="100%" height="100%">     

No index available for this query 로 인덱스가 사용 되지 않은 것을 확인 할 수 있으며 Dcouments Examined의 갯수가 23530으로 전체 문서가 스캔 된 것을 확인 할 수 있습니다.    
또한 Documnets Returned 가 12인 것으로 전체 문서 중 12개 문서가 리턴된 것으로 12개 문서를 찾기 위해 23530 문서를 검색한 것으로 비효율적인 것을 알 수 있습니다.

E-S-R 규칙에 맞추어 인덱스를 생성 하고 Explain에서 개선된 사항을 확인 합니다.


#### Index 생성

테스트를 위해 cast - year - title 순서로 인덱스를 생성 하고 테스트 합니다.   

<img src="/03.Index and tips/images/image02.png" width="50%" height="50%">     


동일한 쿼리를 수행 하여 봅니다.    

<img src="/03.Index and tips/images/image03.png" width="90%" height="90%">     

문서 스캔이 Index 스캔으로 변경 되고 기존에 비해 성능이 개선된 것을 확인 합니다.  

첫 번째에서 IXSCAN으로 생성한 인덱스를 이용하여 12개의 문서가 검색된 것을 확인 할 수 있습니다. 이후 정렬 과정을 거친 후 데이터가 반환 되는 것을 확인 할 수 있습니다. 

인덱스를 ESR 순서로 작성합니다. (cast-title-year)   
동일한 쿼리를 실행 하여 플랜을 확인 합니다.    

<img src="/03.Index and tips/images/image04.png" width="90%" height="90%">     

Projection 항목에 title만을 출력 하도록 하고 Plan을 확인 합니다.


