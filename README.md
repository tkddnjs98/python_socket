# python_socket
개발환경: 언어는 파이썬을 활용하였다. 그리고 pycharm을 활용하여 편집을 하고 실행을 하였다. 


server코드 구현
```

```



```
import socket
import csv
from collections import OrderedDict
serverSocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
serverSocket.bind((socket.gethostname(), 8080)) #Port = 8080
serverSocket.listen(5)
while True:
    clientSocket, address = serverSocket.accept()
    print(f"Connection established from address {address}")
    clientSocket.send(bytes("Welcome to the server!!", "utf-8"))

```
서버에서 client와 연결하는 부분이다. 포트 부분은 8080으로 설정하였다. 


```
accepted_request = clientSocket.recv(2048).decode('utf-8')
    print(accepted_request)
    request = accepted_request.split("\n")
    print(request)
    method = request[0].split()[0]
``` 
요청형식으로 도착한 요청에서 메소드와 경로, 그리고 body를 가져오기 위해
요청에서 \ㅜ에 따라 분리하였습니다. 
['{method} / HTTP/1.1\r', 'Host: {url}\r', 'Content-Type: text/html\r',
  'Connection: keep-alive\r', 'Content-Length: {len(body)}\r', '', '{body}']
 
이런 형식으로 요청이 나눠집니다.
나눠진 요소중에 제일 앞에 있는 요소를 method라는 변수에 저장을 하였습니다. 
그다음 요소중 6번째 글짜부터 나머지 요소를 url이라는 변수에 저장을 하였습니다.
마지막으로 리스트의 제일 뒤에 있는 원소를 body라는 변수에 저장을 하였습니다.  


```
    if "/" in url:
        host, path = url.split('/')
        host = host
        path = path
    else:
        host = url
        path = None

```

url에서 /가 있으면 host와 path로 나누어서 저장, 아니면 host에 url을 저장하였습니다. 


```
        if method == "HEAD":
            clientSocket.send(bytes(
                f"HTTP/1.1 100 CONTINUE\r\nContent-Type: text/html\r\nConnection: keep-alive\r\nContent-Length: {body_length}\r\n\n{body}",
                encoding='utf8'))

```
HEAD method에 대한 응답부분입니다. 
body에 추가적인 데이터가 없고 100 CONTINUE를 보냈습니다. 




```
 elif method == "GET":
            data_df = pd.read_csv('data_server.csv')
            if data_df.empty:
                clientSocket.send(bytes(
                    f"HTTP/1.1 404 NOT_FOUND\r\nContent-Type: text/html\r\nConnection: keep-alive\r\nContent-Length: {body_length}\r\n\n{No data}",
                    encoding='utf8'))
                break

            else:
                print(data_df)
                generated_data = data_df[data_df.columns[0]]
                body2 = {generated_data[0]: generated_data[1]}
                clientSocket.send(bytes(f"HTTP/1.1 200 OK\r\nContent-Type: text/html\r\nConnection: keep-alive\r\nContent-Length: {body_length}\r\n\n{body2}",
                    encoding='utf8'))
            break
```
GET method에 대한 응답부분입니다. data_server이란 csv파일을 판다스를 활용하여 열었습니다. 
만약에 csv파일에 아무것도 존재하지 않으면 body에 No data라는 말을 보냈습니다. 
만약에 데이터가 존재한다면 csv파일에서 칼럼을 받고 칼럼에서 저장된 데이터를 
딕셔너리 형태로 클라이언트에게 보냈습니다. 





```
        elif method == "PUT":
            body = body.split(":")
            body = ['update'] + body
            if "update" in path and len(body) == 3:
                with open ('data_server.csv', "w") as file:
                    writers = csv.writer(file)
                    writers.writerow([body[0]])
                    writers.writerow([body[1]])
                    writers.writerow([body[2]])
                    file.close()
                clientSocket.send(bytes(
                    f"HTTP/1.1 200 OK\r\nContent-Type: text/html\r\nConnection: keep-alive\r\nContent-Length: {body_length}\r\n\n{body}",
                    encoding='utf8'))
                break
            else:
                clientSocket.send(bytes(
                    f"HTTP/1.1 400 BAD_REQUEST\r\nContent-Type: text/html\r\nConnection: keep-alive\r\nContent-Length: {body_length}\r\n\n{body}",encoding='utf8'))
                break
```
PUT을 수현한 부분이다. 
PUT을 받고 body에서 나온 정보를 나눴다. body에서 PUT을 하려는 데이터가 올바른 형식으로 보내졌는지 그리고 PUT을 하는 경로가 
맞는지 확인을 한 후, 경로가 맞으면 csv를 열어서 body에서 입력한 데이터를 저장하였습니다. 
잘 보냈으면 200 OK가 가지만, 잘못 보낸 경우 400 BAD REQUEST가 나옵니다. 


```     elif method == "POST":
            body = body.split(":")
            body = ['create'] + body
            if "create" in path and len(body) == 3:
                with open('data_server.csv', "w") as file:
                    writers = csv.writer(file)
                    writers.writerow([body[0]])
                    writers.writerow([body[1]])
                    writers.writerow([body[2]])
                    file.close()
                clientSocket.send(bytes(
                    f"HTTP/1.1 201 CREATED\r\nContent-Type: text/html\r\nConnection: keep-alive\r\nContent-Length: {body_length}\r\n\n{body}",
                    encoding='utf8'))
                break
            else:
                clientSocket.send(bytes ( f"HTTP/1.1 400 BAD_REQUEST\r\nContent-Type: text/html\r\nConnection: keep-alive\r\nContent-Length: {body_length}\r\n\n{body}", encoding='utf8'))
                break

```
POST을 수현한 부분이다. 
POST를 받고 body에서 나온 정보를 나눴다. body에서 POST를 하려는 데이터가 올바른 형식으로 보내졌는지 그리고 POST를 하는 경로가 
맞는지 확인을 한 후, 경로가 맞으면 csv를 열어서 body에서 입력한 데이터를 저장하였습니다. 
잘 보냈으면 201 CREATED가 전송되지만, 잘못 보낸 경우 400 BAD REQUEST가 전송됩니다. 





client 코드 구현
```
import socket
clientSocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
clientSocket.connect((socket.gethostname(), 8080))

message = clientSocket.recv(2048).decode('utf-8')
print("Message Received", message)
```
client에서 port번호와 ip번호를 입력하여 서버와 연결을 하는 코드이다. 
컴퓨터 한대로 구현하였기 때문에 ip자리에 socket.gethostname()를 입력하였다.  
호스트와 연결을 하고 연결이 되면 호스트에서 Welcome to the server!!라는 메시지가 오는데 
메시지를 받고 출력하였다. 
```
method_check_list = ["GET", "PUT", "POST", "HEAD"]

#IP = '10.30.118.28'
while True:
    method = input("Enter Method:")
    method = method.upper()
    if method not in method_check_list:
        continue
    else:
        break
url = input("ENTER PATH:")
body = input("Enter Body:")
```

method, url, body를 입력하는 부분이다. 
while문을 활용하여 method는 get, put, post, head만 입력하도록 만들었다. 




```
def format_request(method, url, body):
    return f"{method} / HTTP/1.1\r\nHost: {url}\r\nContent-Type: text/html\r\nConnection: keep-alive\r\nContent-Length: {len(body)}\r\n\n{body}"
```
입력한 요청을 http의 프로토콜에 맞춰서 구현하는 함수이다.


```
request = format_request(method, url, body)
clientSocket.send(request.encode('utf-8'))
received_message = clientSocket.recv(2048).decode('utf-8')
print(received_message)
clientSocket.close()
```

입력한 요청을 http의 프로토콜에 맞춰서 구현한다음 
server에 보냈다. 
그리고 요청에 따른 서버의 응답을 print하였고 연결을 해지하였다. 



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

![image](https://user-images.githubusercontent.com/89904421/166412247-7b3b96fa-8121-429d-abaf-7a83b01c101e.png)

서버에서 온 요청

![image](https://user-images.githubusercontent.com/89904421/166412341-119b7f81-0da1-4ce0-9e7e-2cd6b74aaf14.png)

서버의 응답

![image](https://user-images.githubusercontent.com/89904421/166412373-e5f503d3-4004-4dee-9d55-8d43014dd288.png)

csv파일을 확인하면 update라는 칼럼으로 client의 body에서 작성한 데이터가 잘 작성된 것을 볼 수 있다. 

![image](https://user-images.githubusercontent.com/89904421/166412443-90c9c229-ed39-42c4-b8ac-9be2b1a3a27d.png)




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



5. 잘못된 요청을 보내는 경우

client의 잘못된 요청

![image](https://user-images.githubusercontent.com/89904421/166412764-c4ff7f9d-2cbe-4ec3-8157-6b035e99c3b0.png)

이에 따른 응답

![image](https://user-images.githubusercontent.com/89904421/166412789-bd0f6b48-d3f7-43e2-97ad-1a1143e8d511.png)






