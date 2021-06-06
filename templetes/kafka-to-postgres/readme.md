1. Kafka topic의 Json은 객체를 serialize한 것이라 many-to-one 관계에 객체의 이름(eqp1Tr)이 들어가 있다.
1. 이를 sql로 전환하기 위해 reference 컬럼 이름(tr_id)으로 변경해야 한다.
1. 이를 위해 1.0에서는 Json에서 eqp1Tr의 id값을 찾기위해, 그리고 eqp1TrDets의 eqp1Tr을 eq_id로 변경하기 위해 ExtractText + ReplaceText Processor를 사용했다
1. 1.1에서는 두개의 Processor를 UpdateRecord Processor 1개로 변경했다.
---
1. UpdateRecord
    * Properties
      - JsonPath Expression : $.eqp1TrDets
      - /eqp1TrDets[*]/tr_id : /id  -> 직접추가, eqp1TrDets가 배열이다
1. JsonTreeReader
1. JsonRecordSetWriter
    * Properties
      - Output Grouping : One Line Per Object  
1. SplitJson
    * Properties
      - JsonPath Expression : $.eqp1TrDets