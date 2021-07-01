# [동빈나] 12강 - 고객 추가 양식(Form) 구현 및 이벤트 핸들링

- npm install --save axios
  - 서버와 통신하는 axios 설치하기

```react
class CustomerAdd extends React.Component {

    constructor(props) {
        super(props);
        this.state = {
            file: null,
            userName: '',
            birthday: '',
            gender: '',
            job: '',
            fileName: '',
            open: false
        }
    }
}
```

- this.state 으로 초기화하기

```react
    render() {
        return (
            <form onSubmit={this.handleFormSubmit}>
                <h1>고객 추가</h1>
                프로필 이미지 : <input type="file" name="file" file={this.state.file} value={this.state.fileNmae} onChange={this.handleFileChange} /><br/>
                이름 : <input type="text" name="username" value={this.state.userName} onChange={this.handleValueChange} /><br/>
                생년월일 : <input type="text" name="birthday" value={this.state.birthday} onChange={this.handleValueChange}/><br/>
                성별 : <input type="text" name="gender" value={this.state.gender} onChange={this.handleValueChanged}/><br/>
                직업 : <input type="text" name="job" value={this.state.job} onChange={this.handleValueChanged}/><br/>
                <buttom type="submit">추가하기</buttom>
            </form>
        )
    }

}
export default CustomerAdd;
```

- 고객 정보를 form으로 받는다.
- handleFileChange, handleValueChange, handleFormSubmit 3개 함수가 구현되야 함
- 7:20