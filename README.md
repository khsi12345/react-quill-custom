# Getting started
```
 git clone https://github.com/snrntpa2z/react-quill-custom.git
 cd react-quill-custom
 yarn start
 ```

# React - Quill ( Customizing )

- 글꼴 추가 ( 돋움체 , 굴림체 , 바탕체 , 나눔고딕체 )
- 글자 크기 추가 ( 8pt , 9pt, 10pt, 11pt, 12pt, 14pt, 18pt , 24pt, 36pt )
- 이미지 크기 조절 추가

# 이미지 서버로 업로드 후 Img Path 빼는 방법

1. 이미지 핸들러 Function 추가

```
 imageHandler = (image, callback) => {
    const input = document.createElement("input");
    input.setAttribute("type", "file");
    input.setAttribute("accept", "image/*");
    input.click();
    // 이미지 업로드 부분
    input.onchange = async () => {
      const file = input.files[0];
      formData.append("upload_files", file);
      // 서버 REST API 호출 후 이미지 업로드
      function uploadFile() {
        return API.post(`/api/common/quill-image-upload`, formData).then(
          res => {
            return res.data.results[0].src;
          }
        );
      }
      // id 값은 업로드 후 Return 받은 이미지 주소
      const id = await uploadFile();
      // 이미지 주소를 img 태그 안에 넣어준다
      const range = await this.quillRef
      .getEditor.getSelection();
      await this.quillRef
        .getEditor()
        .insertEmbed(range.index, "image", `${id}`);
    };
  };
```

2. modules에 이미지 핸들러 함수 추가

```
 const modules = {
      toolbar: {
        container: [
          [{ font: Font.whitelist }],
          [
            {
              size: Size.whitelist
            }
          ],
          [{ header: [1, 2, false] }],
          ["bold", "italic", "underline", "strike", "blockquote"],
          [
            { align: "" },
            { align: "center" },
            { align: "right" },
            { align: "justify" }
          ],
          [
            { list: "ordered" },
            { list: "bullet" },
            { indent: "-1" },
            { indent: "+1" }
          ],
          ["link", "image", "video"],
          [{ color: [] }, { background: [] }],
          ["clean"]
        ],
        handlers: {
          image: this.imageHandler
        }
      },
      imageResize: true
    };
```

