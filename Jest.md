
## Mock

fake implementation of functions or modules/API

```js
const mockfn = jest.fn( x => x+1 ) 
test('', () => {
	myfunc([0,1], fn)  //myfunc(arr, cb)
	// now we can access call metadata via .mock
	
	expect(mockfn.mock.results[0]).toBe(val) //1st myfunc return was 'val'
})
```

Mocking modules
```js
import lodash from 'lodash';

jest.mock('lodash')  
lodash.pipe.mockResolvedValue(1) //way 1

//way 2
jest.mock('lodash' , () => ({
	__esModule: true,
	default: {
		get: jest.fn().mockResolvedValue(1),
		//other methods
	},
}))
```

```js
afterEach(jest.clearAllMocks)  //mock state are retained across tests
```