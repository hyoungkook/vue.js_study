
{
    "todos": [
            {
                "id": 1,
                "content":"hi"
            },
            {
                "id": 2,
                "content":"hi2"
            }
    ]
}

<!-- 
일단 노트에 json-server 설치방법이 있음.

위 내용은 db.json에 있는 내용.

http://localhost:3000/todos/1 이라고 입력하면 

id:1
content : hi 를 불러오고

http://localhost:3000/todos/2 이라고 입력하면 

id:2
content : hi2 를 불러온다

<큰 오류 예시>
db.json 파일을 제일 바탕화면에도 하나 만들고, src폴더안에도 만들어서 db가 돌아가긴 하는데 db.json파일이 움직이지 않았었다.

-->