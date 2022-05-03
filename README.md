# python_socket
개발환경: 언어는 파이썬을 활용하였다. 그리고 pycharm을 활용하여 편집을 하고 실행을 하였다. 

1. Head
HEAD 메서드는 GET 메서드의 요청과 동일한 응답을 요구하지만, 응답 본문을 포함하지 않습니다.
Head에 관한 client의 입력

![image](https://user-images.githubusercontent.com/89904421/166408581-793db9e9-3b7d-415a-9332-d729e79cc998.png)

서버에서 온 요청

![image](https://user-images.githubusercontent.com/89904421/166408835-8739950d-62ce-46ec-9d29-9877be048ebe.png)

서버의 응답

![image](https://user-images.githubusercontent.com/89904421/166408612-598a35af-f305-4536-8702-2a16b14d035d.png)

2.Get





3.Post 
POST 메서드는 특정 리소스에 엔티티를 제출할 때 쓰입니다. 이는 종종 서버의 상태의 변화나 부작용을 일으킵니다.
Post은 body에서 입력한 데이터를 서버에 저장한다. 이 때, 지정된 경로를 잘 입력해야 한다. 
POST에 관한 client에서의 입력





4. PUT 
PUT 메서드는 목적 리소스 모든 현재 표시를 요청 payload로 바꿉니니다.
PUT에 관한 client에서의 입력
PUT은 body에서 입력한 데이터를 서버에 저장한다. 지정된 경로를 잘 입력해야 저장이 가능하다.  

![image](https://user-images.githubusercontent.com/89904421/166409565-9e3383d1-bd66-42c1-9589-8c59c814eecc.png)

서버에서 온 요청 
![image](https://user-images.githubusercontent.com/89904421/166409623-19323b5b-ac4c-4692-993f-866cc1727c2a.png)

서버의 응답

![image](https://user-images.githubusercontent.com/89904421/166409651-cfe28c57-6387-4159-9f8b-967ea23133bf.png)

이후 csv파일을 확인하면 csv에 create라는 칼럼에서 client의 body에서 작성한 데이터가 잘 작성된 것을 볼 수 있다. 

![image](https://user-images.githubusercontent.com/89904421/166409749-4a62a7ce-585b-46fb-a18d-9de6fd2e8981.png)



5. 잘못된 요청


