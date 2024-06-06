---
layout: single
title: "개발일지 - 01"
categories: ON_PROJECT
author_profile: false
sidebar:
 nav: "counts"
---

##### ON PROJECT 개발일지

###### 네이버 API 호출 후 테스트 - API 호출 메서드

네이버 로컬 검색 API를 호출하여 특정 위치와 카테고리의 정보를 검색하고, 검색 결과를 출력



###### 1. URL 및 HTTP 연결 설정

API 호출을 위한 URL을 생성하고, HTTP 연결을 설정

```java
String apiUrl = "https://openapi.naver.com/v1/search/local.json?query=" + location + " " + category + "&display=10";
URL url = new URL(apiUrl);
HttpURLConnection connection = (HttpURLConnection) url.openConnection();
connection.setRequestMethod("GET");
connection.setRequestProperty("X-Naver-Client-Id", CLIENT_ID);
connection.setRequestProperty("X-Naver-Client-Secret", CLIENT_SECRET);

```

- `apiUrl`: 검색 쿼리를 포함한 API URL을 생성합니다. 여기서 `location`과 `category` 변수는 검색할 위치와 카테고리를 나타냅니다.
- `URL url = new URL(apiUrl)`: 생성된 URL을 `URL` 객체로 변환합니다.
- `HttpURLConnection connection = (HttpURLConnection) url.openConnection()`: URL에 연결을 열고, 이를 `HttpURLConnection` 객체로 캐스팅합니다.
- `connection.setRequestMethod("GET")`: HTTP 요청 메서드를 `GET`으로 설정합니다.
- `connection.setRequestProperty("X-Naver-Client-Id", CLIENT_ID)`, `connection.setRequestProperty("X-Naver-Client-Secret", CLIENT_SECRET)`: HTTP 요청 헤더에 클라이언트 ID와 클라이언트 시크릿을 설정합니다.



###### 2.  응답 코드 확인 및 입력 스트림 설정

HTTP 응답 코드를 확인하고, 입력 스트림을 설정

```java
int responseCode = connection.getResponseCode();
InputStream is = (responseCode == 200) ? connection.getInputStream() : connection.getErrorStream();

```

- `int responseCode = connection.getResponseCode()`: 서버로부터 응답 코드를 받습니다.
- `InputStream is = (responseCode == 200) ? connection.getInputStream() : connection.getErrorStream()`: 응답 코드가 200(성공)일 경우 입력 스트림을 설정하고, 그렇지 않을 경우 에러 스트림을 설정합니다.



###### 3. 응답 데이터 읽기

서버로부터 받은 응답 데이터를 읽고, 문자열로 변환

```java
Scanner scanner = new Scanner(is);
StringBuilder responseBody = new StringBuilder();
while (scanner.hasNext()) {
    responseBody.append(scanner.nextLine());
}
scanner.close();

```

- `Scanner scanner = new Scanner(is)`: 입력 스트림을 읽기 위해 `Scanner` 객체를 생성합니다.
- `StringBuilder responseBody = new StringBuilder()`: 응답 데이터를 저장할 `StringBuilder` 객체를 생성합니다.
- `while (scanner.hasNext()) { responseBody.append(scanner.nextLine()); }`: `Scanner` 객체를 이용하여 입력 스트림을 한 줄씩 읽어 `responseBody`에 저장합니다.
- `scanner.close()`: `Scanner` 객체를 닫습니다.



###### 4. API 응답 출력

읽어온 응답 데이터를 콘솔에 출력

```java
System.out.println("API Response: " + responseBody.toString());

```

`System.out.println("API Response: " + responseBody.toString())`: `StringBuilder` 객체에 저장된 응답 데이터를 문자열로 변환하여 콘솔에 출력합니다.



###### 5. 예외 처리

예외 발생 시 스택 트레이스를 출력

```java
} catch (Exception e) {
    e.printStackTrace();
}

```

- `catch (Exception e)`: 예외가 발생할 경우 해당 예외를 잡습니다.
- `e.printStackTrace()`: 예외의 스택 트레이스를 콘솔에 출력합니다.



###### 

###### 필요한 변수

- `CLIENT_ID`: 네이버 클라이언트 ID
- `CLIENT_SECRET`: 네이버 클라이언트 시크릿

```java
private static final String CLIENT_ID = "YOUR_NAVER_CLIENT_ID"; // 네이버 클라이언트 ID
private static final String CLIENT_SECRET = "YOUR_NAVER_CLIENT_SECRET"; // 네이버 클라이언트 시크릿

```





###### 전체 API 호출 메서드

```java
public static void main(String[] args) {
    try {
        String location = "서울"; // 예: 서울
        String category = "음식점"; // 예: 음식점

        // API URL 설정
        String apiUrl = "https://openapi.naver.com/v1/search/local.json?query=" + location + " " + category + "&display=10";
        URL url = new URL(apiUrl);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        connection.setRequestMethod("GET");
        connection.setRequestProperty("X-Naver-Client-Id", CLIENT_ID);
        connection.setRequestProperty("X-Naver-Client-Secret", CLIENT_SECRET);

        // 응답 코드 확인 및 입력 스트림 설정
        int responseCode = connection.getResponseCode();
        InputStream is = (responseCode == 200) ? connection.getInputStream() : connection.getErrorStream();
        Scanner scanner = new Scanner(is);
        StringBuilder responseBody = new StringBuilder();
        while (scanner.hasNext()) {
            responseBody.append(scanner.nextLine());
        }
        scanner.close();

        // API 응답 출력
        System.out.println("API Response: " + responseBody.toString());
    } catch (Exception e) {
        e.printStackTrace();
    }
}

```


