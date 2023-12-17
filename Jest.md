
## Mock

fake implementation of functions or modules/API

```js
const mockfn = jest.fn( x => x+1 ) 
test('', () => {
	myfunc([0,1], mockfn)  //myfunc(arr, cb)
	// now we can access call metadata via .mock
	
	expect(mockfn.mock.results[0]).toBe(val) //1st myfunc return was 'val'
})
```

Mocking modules
```js
import axios from 'axios';
jest.mock('axios') //module-name

//WAY 1
axios.get = jest.fn()
axios.get.mockReturnValue(1) 

//WAY 2
jest.mock('axios' , () => ({
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