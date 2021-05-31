---
title: "[리액트 프로그래밍 정석] 컨텍스트로 데이터 관리하기"
date: 2021-05-31 08:26:28 -0400
categories: front-end react
url: https://heeyeonkoo99.github.io/front-end/
---
3장에서 배운 property와 state는 부모와 자식컴포넌트가 연결된 상태에서 공유하는 데이터였다. 반면 컨텍스트는 부모와 자식 컴포넌트가 연결되어 있지 않아도 데이터를 공유할수 있게 해준다. 그렇기에 현재 로그인한 유저,테마, 선호하는 언어등의 데이터를 사용하는데에 쓰인다. 

# 컨텍스트 API란?
![image](https://user-images.githubusercontent.com/68431716/120163846-e6ba7780-c234-11eb-9154-c3ba3e42b563.png)

'공급자와 소비자를 컴포넌트로 구현하여 컨텍스트를 구성하는 과정'은 리액트 16.3버전의 컨텍스트 API에 편입되었다. 즉 컨텍스트 API를 사용하면 일일이 공급자와 소비자를 구현하지 않아도 된다.     
## 1. createContext()함수로 공급자와 소비자 만들기
- createContext()는 리액트 최상위 함수이므로 React.createContext()와 같이 사용한다. 공급자는 Provider, 소비자는 Consumer로 정의되었다.     
```javascript
const MyContext=React.createContext(defaultValue);
// MyContext.Provider, MyContext.Consumer으로 접근하여 사용
// 또는 const {Provider, Consumer}=React.createContext(defaultValue);와 같이 분할 할당하여 사용
```
     
## 2. 컨텍스트 API로 공급자와 소비자 만들기    
- 컨텍스트 API를 이용하여 로딩 상태를 표시하는 공급자 컴포넌트르 작성해보겠다. 컨텍스트 API를 통해 생성된 공급자와 소비자는 각각 독립된 저장 공간을 가지면서 짝을 이뤄 데이터를 공유한다.     
__1. createContext()함수로 공급자 만들기__    
여기서 setLoading()함수는 key,value,를 인자로 받아 key에 해당하는 state값을 저장한다. 예를 들어 setLoading("loading1",true)을 호출하면 state는 {loading1:true}가 되는것이다. 이후 render()함수가 호출되면서 컨텍스트 데이터는 {loading1:true,setLoading:this.setLoading}이 될것이다.

```javascript
  
import React from 'react';

const { Provider, Consumer } = React.createContext({});

export { Consumer };

export default class LoadingProvider extends React.Component {
  constructor(props) {
    super(props);

    this.state = {};
    this.setLoading = this.setLoading.bind(this);
  }

  setLoading(key, value) {
    const newState = { [key]: value };
    this.setState(newState);
  }

  render() {
    const context = {
      ...this.state,
      setLoading: this.setLoading,
    };

    return <Provider value={context}>{this.props.children}</Provider>;
  }
}
```
    
__2. 한개의 공급자를 구독하는 세개의 소비자 만들기__    
익스포트한 Consumer를 받아 세 개의 소비자를 만든것이다. 
```javascript

import React from 'react';
import PropTypes from 'prop-types';
import Button from '../04/Button';
import { Consumer } from './LoadingProviderWithNewContext';

function ButtonWithNewConsumer({ label }) {
  return (
    <React.Fragment>
      <Consumer>
        {value => (
          <Button onPress={() => value.setLoading('loading', !value.loading)}>
            {value.loading ? '로딩중' : label}
          </Button>
        )}
      </Consumer>
      <Consumer>
        {({ loading2, setLoading }) => (
          <Button onPress={() => setLoading('loading2', !loading2)}>
            {loading2 ? '로딩중' : label}
          </Button>
        )}
      </Consumer>
      <Consumer>
        {({ loading, loading2 }) => <Button>{loading && loading2 ? '로딩중' : label}</Button>}
      </Consumer>
    </React.Fragment>
  );
}

ButtonWithNewConsumer.propTypes = {
  label: PropTypes.string,
};

export default ButtonWithNewConsumer;
```

# 컨텍스트로 모달 만들기     
<createModalProvider.jsx>    
```javascript
  
import React, { PureComponent } from 'react';
import Modal from './Modal';
import { Provider } from './ModalContext';

export default function createModalProvider(ContentMap = {}) {
  return class ModalProvider extends PureComponent {
    constructor(props) {
      super(props);
  
      this.state = { showModal: false };
      this.handleClose = this.handleClose.bind(this);
      this.handleOpen = this.handleOpen.bind(this);
    }

    handleOpen(contentId, modalProps) {
      this.contentId = contentId;
      this.modalProps = modalProps;
      this.setState({ showModal: true });
    }

    handleClose() {
      this.setState({ showModal: false });
    }

    render() {
      const { children } = this.props;
      const { showModal } = this.state;
      const ModalContent = ContentMap[this.contentId];
  
      return (
        <Provider
          value={{
            openModal: this.handleOpen,
            closeModal: this.handleClose,
          }}
        >
          {children}
          {showModal && ModalContent && (
            <Modal>
              <ModalContent {...this.modalProps} />
            </Modal>
          )}
        </Provider>
      );
    }
  };  
}

```
<DeleteModalContent.jsx>

```javascript
import React from 'react';
import { Consumer } from './ModalContext';
import Button from '../04/Button';
import Text from '../04/Text';

export default function DeleteModalContent({ id, name }) {
  return (
    <Consumer>
      {({ closeModal }) => (
        <div>
          <div>
            <Text>
              {name}을 정말로 삭제 하시겠습니까?
            </Text>
          </div>
          <Button primary>예</Button>
          <Button onPress={closeModal}>닫기</Button>
        </div>
      )}
    </Consumer>
  );
}
```
<CreateMemberModalContent.jsx>

```javascript

import React, { PureComponent } from 'react';
import { Consumer } from './ModalContext';
import Button from '../04/Button';
import Text from '../04/Text';
import Input from '../03/Input';

class CreateMemberModalContent extends PureComponent {
  render() {
    return (
      <Consumer>
        {({ closeModal }) => (
          <div>
            <div>
              <Text>회원가입</Text>
              <div>
                <Input label="이메일" name="email" />
              </div>
              <div>
                <Input label="이름" name="name" />
              </div>
              <div>
                <Input label="비밀번호" name="password" />
              </div>
            </div>
            <Button primary>가입하기</Button>
            <Button onPress={closeModal}>닫기</Button>
          </div>
        )}
      </Consumer>
    );
  }
}

export default CreateMemberModalContent;
```
<ModalProviderWithKey.jsx>

```javascript
  
import createModalProvider from './createModalProvider';
import DeleteModalContent from './DeleteModalContent';
import CreateMemberModalContent from './CreateMemberModalContent';

export const CONFIRM_DELETE_MODAL = 'confirm_delete_modal';
export const CREATE_MEMBER_MODAL = 'create_member_modal';

const CONTENT_MAP = {
  [CONFIRM_DELETE_MODAL]: DeleteModalContent,
  [CREATE_MEMBER_MODAL]: CreateMemberModalContent,
};

export default createModalProvider(CONTENT_MAP);
```
<ModalStory.jsx>
```javascript
  
import React from 'react';
import { storiesOf } from '@storybook/react';

import Modal from '../06/Modal';
import ModalProvider, { Consumer } from '../06/ModalProvider';
import ModalProviderWithKey, {
  CONFIRM_DELETE_MODAL,
  CREATE_MEMBER_MODAL,
} from '../06/ModalProviderWithKey';
// import { Consumer as ModalConsumer } from '../06/createModalProvider';
import { Consumer as ModalConsumer } from '../06/ModalContext';
import Button from '../04/Button';
import Text from '../04/Text';
import ButtonWithModal from '../06/ButtonWithModal';

storiesOf('Modal', module)
  .addWithJSX('기본 설정', () => (
    <Modal>
      <div>
        <Text>정말로 삭제 하시겠습니까?</Text>
      </div>
      <Button primary>예</Button>
      <Button>닫기</Button>
    </Modal>
  ))
  .addWithJSX('ButtonWithModal', () => <ButtonWithModal />)
  .addWithJSX('ModalProvider', () => (
    <ModalProvider>
      <div>
        <Text>다음 버튼 눌러 모달을 실행합니다.</Text>
        <Consumer>{({ openModal }) => <Button onPress={() => openModal()}>삭제</Button>}</Consumer>
      </div>
    </ModalProvider>
  ))
  .addWithJSX('ModalProviderWithKey', () => (
    <ModalProviderWithKey>
      <div>
        <Text>다음 버튼 눌러 모달을 실행합니다.</Text>
        <ModalConsumer>
          {({ openModal }) => (
            <Button onPress={() => openModal(CONFIRM_DELETE_MODAL, { id: 1, name: '상품1' })}>
              모달 열기
            </Button>
          )}
        </ModalConsumer>
        <ModalConsumer>
          {({ openModal }) => (
            <Button onPress={() => openModal(CREATE_MEMBER_MODAL)}>회원 가입</Button>
          )}
        </ModalConsumer>
      </div>
    </ModalProviderWithKey>
  ));
```
-------
### 참고하여 알아둘것!
* 참고한 사이트: <https://ko.reactjs.org/docs/context.html>     
* 참고한 블로그: <https://velopert.com/3606>    
**~~갓 벨로퍼트...~~**



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

