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

```react
addCustomer = () => {
        const url = '/api/customers';
        const formData = new FormData();
        formData.append('image', this.state.file);
        formData.append('name', this.state.userName);
        formData.append('birthday', this.state.birthday);
        formData.append('gender', this.state.gender);
        formData.append('job', this.state.job);
        const config = {
            headers: {
                'content-type': 'multipart/form-data'
            }
        }
        return post(url, formData, config);
    }
```

- 특정 api주소로 form데이터를 보낸다.
- 이건 ajax에서도 새로운 form 데이터를 보낼때도 이렇게 사용함
- post라이브러리는 axios에 포함되어 있음

```react
handleFormSubmit = (e) => {
        e.preventDefault()
        this.addCustomer()
            .then((response) => {
                console.log(response.data);
                this.props.stateRefresh();
            })
        this.setState({
            file: null,
            userName: '',
            birthday: '',
            gender: '',
            job: '',
            fileName: '',
            open: false
        })
    }
```

- 이것도 ajax에서 form을 할 때 해당 form의 sumbit이 바로 실행되지 않도록 pre~를 해줌

```react
handleFileChange = (e) => {
        this.setState({
            file: e.target.files[0],
            fileName: e.target.value
        })
    }
```

- 실제로 setState에 있는 값을 변경해줌
- 왜 첫번째 값이냐?
  - 우리는 파일을 1개만 업로드 할거니깐 그 파일중 0번째

```react
handleValueChange = (e) => {
        let nextState = {};
        nextState[e.target.name] = e.target.value;
        this.setState(nextState);
    }
```

- State값을 변경해줌

```react
import CustomerAdd from './components/CustomerAdd';
```

- app.js에 추가해줌