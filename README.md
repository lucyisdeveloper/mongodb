# MongoDB 😎
<h4>☝두개의 컬렉션 조인(inner join)</h4>
<p>
//컬렉션 test1 과 test2를 조인하는 방법 -> 해당방법은 테이블 형태로 출력됨<br/>
// localField, foreignField 는 모두 index 걸려있어야 빠름<br/>
db.getCollection('test1').aggregate([ <br/>
    { <br/>
        $lookup: { <br/>
            from:               'test2',      // 조인할 테이블<br/>
            localField:     'no',           // test1의 join key <br/>
            foreignField: 'no',          // 조인할 테이블의  join key <br/>
            as:                     'test22'   // 새로 생성되는 필드명<br/>
        } <br/>
    },<br/>
    {<br/>
      $replaceRoot: { newRoot: { $mergeObjects: [ '$$ROOT' , { $arrayElemAt: [ '$test22', 0 ] } ] } }           // as에 작성한 이름을 적음<br/>
    },<br/>
    { <br/>
	  $project: { fromItems: 0, 'test22' : 0 }        // as에 작성한 이름을 적음<br/>
    },<br/>
    {<br/>
        $match: {<br/>
		$and: [<br/>
		                { 'age': { $exists: true}} ,          // 조건을 작성( inner join일 경우 다음과 같은 조건을 추가해줌)       <br/>       
		                { 'settleYm' : "202106"}<br/>
        }<br/>
    },<br/>
    { $out : "test_20210715" }			            // 결과를 test_20210715 컬렉션 생성 및 데이터 insert<br/>
])<br/>
</p>
