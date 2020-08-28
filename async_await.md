# async/await

es6에서 Promise가 나왔지만 여전히 사람들은 간결한 코드를 원하였다. 사람들의 욕구를 만족하기 위하여 es8에서는 async/await이 등장하였고 이로 하여금 비동기 처리가 더욱 짧은 코드로 가능해졌다.



## Promise vs async/awiat

async/await이 나오기 이전에는 Promise로 비동기 처리를 하였다. Promise는 콜백 지옥을 탈출하기 위해 만들어진 문법이다. 하지만 이런 Promise도 Promise 지옥을 만들었고 사람들은 더욱 간편한 코드를 원하였다. 이러한 이유로 es8에서 async/await이 등장한 것이다.

<br>



다음은 Promise와 async/await에 차이다.

```javascript
const funcP = (x) =>{
    return new Promise((resolve, reject) =>{
        setTimeout(_ => {
            console.log(x)
            resolve(x)
        }, 1000)
    })
}

//프로미스
const promiseFunc = () => {
    let x = 0
    for(let i = 1, pending = Promise.resolve(); i <= 10; i++) {
        pending = pending.then(_ => funcP(i))
    }
}

promiseFunc()

//async/await
const asyncFunc = async () => {
    for(let i = 1; i <= 10; i++) {
        await funcP(i)
    }
}

asyncFunc()
```

<br>



한눈에 봐도 async/await이 Promise보다 단순하다는 것을 알 수 있다.

<br>

<br>



## async/await와 Promise의 관계

async/await은 항상 Promise와 연관지어서 생각해야 한다. async는 특정 함수를 async/await으로 지정하는 키워드이고 await은 async 함수 내부에서 비동기로 실행될 부분에 지정하는 키워드이다. 항상 주의할 점은 **await으로 지정하는 함수는 return값이 Promise 객체여야 한다는 것**이다.

<br>



다음은 async/await을 간단한 형식으로 나타낸 것이다.

```javascript
const funcP = () => {
    return new Promise(.....)
}

const asyncFunc = async () => { //async로 함수를 설정
    await funcP() //await으로 Promise객체를 반환하는 함수 실행
}
```

<br>



또한 우리는 async/await와 Promise를 섞어서 사용할 수도 있다. await으로 함수를 실행하면 Promise객체를 반환한다는 점을 이용하면 된다.

<br>



다음은 async/await과 Promise를 동시에 이용한 예이다.

```javascript
const funcP = (x) => {
    return new Promise(resolve => {
        console.log(x)
        resolve(x)
    })
}

const asyncFunc = async () => {
    await funcP(1)
    await funcP(2).then(x => {
        console.log(x + 10)
    })
}

asyncFunc()
```

<br>

<br>



## async 함수 내부에 async 다루기

이번에는 async 내부에 async가 어떻게 동작하는지 알아보자

<br>



다음 예제를 보자

```javascript
const foo = (x, y) => {
    return new Promise(resolve => {
        setTimeout(_ =>{
            console.log(x)
            resolve(x)
        }, y * 1000)
    })
}

const test = async () => {
    const go1 = async () => {
        await foo(1, 3)
    }
    go1()
    await foo(2, 1)
    await foo(3, 4)
}

test()
```

<br>



다음의 결과를 보면 비동기 처리가 내가 원하는 순서가 아닌 go1()과 foo(2, 1)이 경쟁을 통하여 빨리 끝나는 것이 먼저 실행 된다는 것을 알 수 있다. 하지만 foo(2, 1)과 foo(3, 4)는 내가 지정한 순서대로 실행 된다는 것을 알 수 있다. 결과적으로 같은 async 블록 안에 묶인 await은 동시에 실행 되지만 다른 async 블록 안에 묶인 await은 서로 경쟁한다는 것이다.

<br>

<br>



## 출처

[victolee](https://victorydntmd.tistory.com/)

[stackoverflow](https://stackoverflow.com/questions/40328932/javascript-es6-promise-for-loop/40329190)