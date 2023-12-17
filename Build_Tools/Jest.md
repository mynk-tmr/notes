
## Mock

fake implementation of functions or modules/API

Required stmts
```js
import { jest } from '@jest/globals'
afterEach(jest.clearAllMocks) //to clear mockstate
```

Mock functions
```js
const mockfn = jest.fn( x => x+1 ) 

test('', () => {
	myfunc([0,1], mockfn)  //myfunc(arr, cb)
	// now we can access metadata via .mock
	
	expect(mockfn.mock.calls).toHaveLength(1)  //called once

})
```

Mocking modules
```js
import axios from 'axios';
jest.mock('axios') //module-name

//WAY 1
axios.get = jest.fn().mockReturnValue(1);
```

Methods for mockfn
```js
myMock.mockReturnValueOnce(10).mockReturnValueOnce('x').mockReturnValue(true); //10 x true true ..


```


Mock object
```js
let cb = mockfn.mock ;

expect(cb.calls).toHaveLength(1); //called once only
expect(cb.calls[7][0]).toBe('arg1'); //8th call with 1st arg as 'arg1'
expect(cb.results[0].value).toBe(12); //mockfn returns 12 on first call
expect(cb.lastCall[3]).toBe('test'); //4th arg of last call


expect(cb.contexts[0]).toBe(hot); //this=hot in first mockfn call
expect(cb.instances.length).toBe(2); //exactly 2 instances from mockfn
expect(cb.instances[1].name).toBe('jon'); //2nd obj has name: jon
```