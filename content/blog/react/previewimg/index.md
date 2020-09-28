---
title: '[React] Preview Image - 이미지 미리보기'
date: '2020-09-28'
category: 'react'
---

오늘은 컴퓨터에 있는 이미지를 업로드 하고 싶을 때, 
미리보기를 구현해 보려고 한다.

![](https://images.velog.io/images/jotang/post/b0a87ae0-81d4-467d-a426-c8931b396a94/image.png)

## 준비하기

파일업로드를 위해서는 form태그의 `enctype='multipart/form-data'`를 이용해야한다.

multipart/form-data에 대해서는 찾아보길 바란다.

```jsx
<form encType='multipart/form-data'>
  <label htmlFor='file'>이미지업로드</label>
  <input type="file" id='file' accept='image/jpg, image/jpeg, image/png' />
</form>
```
form태그에 encType을 multipart/form-data로 지정해준다.

그런 후에,

input 태그에 `type='file'`로 지정하고,
업로드를 허용할 파일 확장자명들을 `accept`에 작성한다.
작성하지 않는다면 default로 모든 파일을 업로드 할 수 있게 된다.

`accept='image/*'` 이미지에 해당하는 모든 파일을 허용한다는 의미이다.

이미지를 표시할 구역까지 정해준다면, 모든 준비는 마친것이다.


## 한 장의 이미지 업로드 해보기


먼저 업로드를 했을 때, 해당 파일이 잘 들어오는 지 확인이 필요하다.

input 태그에 이벤트를 걸어주고, 확인해보자
```jsx
 <FileInput 
   type="file" 
   id='file' 
   accept='image/jpg, image/jpeg, image/png' 
   onChange={(e) => insertImg(e)}/>
```

```jsx
const insertImg = (e) => {
  console.log(e.target.files[0])
}
  ```
![](https://images.velog.io/images/jotang/post/9ef21d73-c26c-4a17-90b7-07dac936d17c/image.png)

file이 들어온것을 알 수 있다.

FileReader객체를 통해서 img태그 src에 들어갈 정보를 얻을 것이다.

MDN에서 FileReader를 검색해보면,
> FileReader 객체는 웹 애플리케이션이 비동기적으로 데이터를 읽기 위하여 읽을 파일을 가리키는File 혹은 Blob 객체를 이용해 파일의 내용을(혹은 raw data버퍼로) 읽고 사용자의 컴퓨터에 저장하는 것을 가능하게 해줍니다.

라고 나와있다.

```jsx
const insertImg = (e) => {
  let reader = new FileReader()

  if(e.target.files[0]) {
    reader.readAsDataURL(e.target.files[0])
  }

  reader.onloadend = () => {
    const previewImgUrl = reader.result

  	console.log(previewImgUrl)
  }
}
```
생성자 함수를 통해 FileReader를 불러온다.


`readAsDataURL`메서드는, 컨테츠를 특정 Blob이나 File에서 읽어오는 역할을 하게되고, 읽어오는 read가 종료되는 경우에, readyState의 상태가 DONE이 되고, loadend이벤트가 트리거 되면서, base64 인코딩된 스트링 데이터가 result에 담겨지게 된다.

아래 이미지는 console.log를 찍어본 것으로, bash64로 인코딩된 데이터를 확인할 수 있다.

![](https://images.velog.io/images/jotang/post/4cfb175b-22bf-457f-95bf-bff1033a21f5/image.png)

[FileReader MDN](https://developer.mozilla.org/ko/docs/Web/API/FileReader)

```jsx
const [ previewImg, setPreviewImg ] = useState(null)

const insertImg = (e) => {
  let reader = new FileReader()

  if(e.target.files[0]) {
    reader.readAsDataURL(e.target.files[0])
  }

  reader.onloadend = () => {
    const previewImgUrl = reader.result

    if(previewImgUrl) {
      setPreviewImg([...previewImg, previewImgUrl])
    }
  }
}
```

base64로 변환된 스트링 데이터를 state에 담아주고, img의 src에 넣어주게 되면, 업로드한 이미지를 확인할 수 있게 된다.

![](https://images.velog.io/images/jotang/post/9c338d29-6d47-4bac-91cc-c03f9af22714/image.png)

## 이미지 이름 보여주기 / 삭제하기

우리는 이미지를 추가는 했지만, 업로드한 이미지를 삭제하고 싶을 수 있기에, 
업로드된 이미지의 이름과 삭제 버튼을 추가할 것이다.

`e.target.files[0]`에서 이미지의 파일 이름을 확인 할 수 있다는 것을 아까 알게 되었다.

```jsx
const [ img, setImg ] = useState(null)
const [ previewImg, setPreviewImg ] = useState(null) 

const insertImg = (e) => {
  let reader = new FileReader()

  if(e.target.files[0]) {
    reader.readAsDataURL(e.target.files[0])
    
    setImg(e.target.files[0])
  }

  reader.onloadend = () => {
    const previewImgUrl = reader.result

    if(previewImgUrl) {
      setPreviewImg([...previewImg, previewImgUrl])
    }
  }
}

// render
<ImgArea>
  <Img src={previewImg} alt='img'/>
</ImgArea>
<ImgName>{img}</ImgName>
<DeleteButton>❌</DeleteButton>
```

![](https://images.velog.io/images/jotang/post/d684c6bc-d1cb-4135-bbc0-96a14e83cb83/image.png)

DeleteButton에 삭제하는 함수를 이벤트로 등록해준다. 간단하게 state를 비워주면 된다.


```jsx
const deleteImg = () => {
  setImg(null)
  setPreviewImg(null)
}


<DeleteButton onClick={deleteImg}>❌</DeleteButton>
```

## 여러 장의 이미지 업로드 / 삭제하기
여러 장의 미리보기 이미지 보여주기와 삭제하기 역시 간단하다.

배열에 저장을 해주고, map()과 filter()를 통해서 구현해 보려고 한다.

먼저 state를 배열로 바꿔준다.
```jsx
const [ img, setImg ] = useState([])
const [ previewImg, setPreviewImg ] = useState([])
```

그리고 state에 담아준다.

```jsx
const insertImg = (e) => {
  let reader = new FileReader()

  if(e.target.files[0]) {
    reader.readAsDataURL(e.target.files[0])
    
    setImg([...img, e.target.files[0]])
  }

  reader.onloadend = () => {
    const previewImgUrl = reader.result

    if(previewImgUrl) {
      setPreviewImg([...previewImg, previewImgUrl])
    }
  }
}
```

img 와 previewImg에 순서에 맞게, 해당 이미지 파일과 base64로 인코딩된 스트링 데이터가 들어가게 된다.

이제는 map()을 이용하여 render를 해줄 차례이다.
```jsx
const getPreviewImg = () => {
  if(img === null || img.length === 0) {
    return (
      <ImgAreaContainer>
        <ImgArea>
          <Img 
            src="https://k-startup.go.kr/images/homepage/prototype/noimage.gif" 
            alt="dd"/>
        </ImgArea>
        <ImgName>등록된 이미지가 없습니다.</ImgName>
      </ImgAreaContainer>
    )
  } else {
    return img.map((el, index) => {
      const { name } = el

      return (
        <ImgAreaContainer key={index}>
          <ImgArea>
            <Img src={previewImg[index]} />
          </ImgArea>
          <ImgName>{name}</ImgName>
          <DeleteButton onClick={deleteImg}>❌</DeleteButton>
        </ImgAreaContainer>
      )
    })
  }
}

// render 
 {getPreviewImg()}
```

![](https://images.velog.io/images/jotang/post/8671eb04-9678-4e5b-b865-58ea2e2e8801/image.png)

![](https://images.velog.io/images/jotang/post/110c390b-8e1d-472b-b1d8-59f8c4d490fb/image.png)

마지막으로 삭제를 할 때는, 살짝 주의를 해야한다.
처음에는 이미지의 name으로 배열에서 제외를 했는데, 같은 이미지를 업로드하게 되었을 때, 문제가 되어 index를 활용하기로 하였다.

```jsx
const deleteImg = (index) => {
  const imgArr = img.filter((el, idx) => idx !== index)
  const imgNameArr = previewImg.filter((el, idx) => idx !== index )

  setImg([...imgArr])
  setPreviewImg([...imgNameArr])
}

<DeleteButton onClick={() => deleteImg(index)}>❌</DeleteButton>
```
함수의 인자로 index를 전달해주고, 해당 index와 같지 않은 요소를 새로운 배열로 반환하여 state에 담아준다.

### formData로 변환하고 서버와 주고 받는 것은, 추후에 준비해서 가져오도록 하려고 합니다. 두서없는 글 읽어주셔서 감사드리고, 부족한 부분이나 제가 틀린 부분, 또 더 좋은 방법들이 있다면 알려주세요!! 감사합니다.



제가 작성한 코드는
[Github](https://github.com/taeho-jo/various_functions)에서 확인할 수 있습니다!!

아래 링크에서는 이미지 업로드 및 삭제 직접 확인할 수 있습니다!!

[Various_Functions](https://variousfunctions.netlify.app/)


  