# MongoDB ๐
<h4>โ๋๊ฐ์ ์ปฌ๋ ์ ์กฐ์ธ(inner join)</h4>
<p>
//์ปฌ๋ ์ test1 ๊ณผ test2๋ฅผ ์กฐ์ธํ๋ ๋ฐฉ๋ฒ -> ํด๋น๋ฐฉ๋ฒ์ ํ์ด๋ธ ํํ๋ก ์ถ๋ ฅ๋จ<br/>
// localField, foreignField ๋ ๋ชจ๋ index ๊ฑธ๋ ค์์ด์ผ ๋น ๋ฆ<br/>
db.getCollection('test1').aggregate([ <br/>
    { <br/>
        $lookup: { <br/>
            from:               'test2',      // ์กฐ์ธํ  ํ์ด๋ธ<br/>
            localField:     'no',           // test1์ join key <br/>
            foreignField: 'no',          // ์กฐ์ธํ  ํ์ด๋ธ์  join key <br/>
            as:                     'test22'   // ์๋ก ์์ฑ๋๋ ํ๋๋ช<br/>
        } <br/>
    },<br/>
    {<br/>
      $replaceRoot: { newRoot: { $mergeObjects: [ '$$ROOT' , { $arrayElemAt: [ '$test22', 0 ] } ] } }           // as์ ์์ฑํ ์ด๋ฆ์ ์ ์<br/>
    },<br/>
    { <br/>
	  $project: { fromItems: 0, 'test22' : 0 }        // as์ ์์ฑํ ์ด๋ฆ์ ์ ์<br/>
    },<br/>
    {<br/>
        $match: {<br/>
		$and: [<br/>
		                { 'age': { $exists: true}} ,          // ์กฐ๊ฑด์ ์์ฑ( inner join์ผ ๊ฒฝ์ฐ ๋ค์๊ณผ ๊ฐ์ ์กฐ๊ฑด์ ์ถ๊ฐํด์ค)       <br/>       
		                { 'settleYm' : "202106"}<br/>
        }<br/>
    },<br/>
    { $out : "test_20210715" }			            // ๊ฒฐ๊ณผ๋ฅผ test_20210715 ์ปฌ๋ ์ ์์ฑ ๋ฐ ๋ฐ์ดํฐ insert<br/>
])<br/>
</p>
