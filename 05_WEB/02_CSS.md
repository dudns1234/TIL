## CSS
### color
1. body-h1 태그 속성값으로 직접 색깔지정
    -ex. `<h1 style="color : red">hello</h1>`
2. head-style 태그에 색깔지정
    - ex. 선택자{ 속성명 : 속성값} `h2{color:blue}`
3. css 파일을 따로 만들어서 link 설정하여 태그별로 연결
    - 태그로 전체 색깔지정 `<link rel="stylesheet" href="./mystyle.css">`
    - 태그가 같더라도 #id로 지정해주면 id가 우선
        - id는 한클래스에 한번만
4. P태그 속 속성들(id, class)로 color, font 지정
- class 선택자
    ```html
    <!-- HTML -->
    <p class="hello">하나</p>
    ```
    ```CSS
    /* class 선택자 */
    .hello{
        color: purple;
    }
    ```
- id 선택자
    ```HTML
    <p id='here'>two</p>
    ```
    ```CSS
    /* id 선택자 */
    #here{
        color : gold;
        background-image: url('./assets/image/background.jpg');
    }
    ```
- p의 class 선택자
    ```HTML
    <p class="center">three</p>
    ```
    ```CSS
    /* p의 class 선택자 */
    p.center{
        text-align: center;
    }
    ```
- p의 id 선택자
    ```HTML
    <p class="center" id="size">one</p>
    ```
    ```CSS
    /* p의 id 선택자 */
    p#size{
        font-size: 50px;
    }
    ```
- ul의 class 선택자의 li 모두
    ```HTML
    <ul class="english">
            <li>python</li>
            <li>HTML</li>
            <li>CSS</li>
    ```
    ```CSS
    ul.english > li{
        color: royalblue;
    }
    ```

### Border
- border-width: 2px
- border-style: dashed
- border-color: bisque
- border: 2px dashed bisque (위세개를 한 번 축약가능)
- 사용예시
    ```CSS
    .korean{
    /* border-width: 2px;
    border-style: dashed;
    border-color: bisque; */
    border: 2px dashed bisque;
    border-radius: 5px;
    }
    ``` 
- border-radius:5px

## Margin
- margin-top: 50px
- margin: 25px, 50px, 75px, 100px(Border과 마찬가지로 축약가능)
- 사용예시
```CSS
.english{
    /* margin-top: 50px; */
    margin: 25px, 50px, 75px, 100px;
}
```

## font
[font](https://developers.google.com/fonts/docs/getting_started?hl=ko)