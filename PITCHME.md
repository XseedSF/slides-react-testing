---

## TESTING REACT APPS

#### AN INTRODUCTION TO <span class="title-jest">JEST</span> AND <span class="title-enzyme">ENZYME</span>

---

### WHY TEST

- Find and prevent errors
- Executable documentation
- Fixing bugs for ever

---

### JEST

- GOOD
	- Fastest option
	- Test only what changes
	- Supports snapshot testing
	- Integral solution ¯\\_(ツ)_/¯
	- Supported by Facebook 

-BAD
	- Lack of documentation

---

### TEST STRUCTURE
```javascript
describe('Component', () => {
	// alias for test(name, fn)
	it('Adds two numbers correctly', () => {
	  expect(/* data to test */).toBe(/* expected value */);
	});
})
```
@[1,6](describe test set)
@[2,3-5](individual test to perform)
@[4](assertion, checks if the test is correct)

---

### LETS TESTS SOMETHING

__/Add/index.js__
```javascript
const Add = (x, y) => x + y;
export default Add;
```
<div class="fragment">
	- Adds two numbers correctly [2+3 = 5] <br/>
	- Commutative property [2+3 = 3+2] <br/>
	- Distributive property [1+(2+3) = (1+2)+3]
</div>

+++

__/Add/test.js__
```javascript
import Add from '.';
describe("Add", () => {
	// 2+3 = 5
	it("Adds two numbers correctly", () => {
		expect(Add(2, 3)).toBe(5);
	});
	// 2+3 = 3+2
	it("Commutative property", () => {
		expect(Add(2, 3)).toBe(Add(3, 2));
	});
	// 1+(2+3) = (1+2)+3
	it("Distributive property", () => {
		expect(Add(1, Add(2, 3))).toBe(Add(Add(1, 2), 3));
	});
});
```
@[3-6](Correctly adds two numbers)
@[7-10](Commutative)
@[11-14](Distributive)

---

### Matchers

The expectation object exposes **Matchers** that let you validate if the test is doing what we expected

+++

```javascript
describe("Testing Matchers", () => {
	it("Exact equality", () => {
		expect(2 + 3).toBe(5);
	});
	it("Object equality", () => {
		const obj = { a: 1 };
		obj['b'] = 2;
		expect(obj).toEqual({ a: 1, b: 2 });
	});
	it("Testing the opposite of a matcher", () => {
		expect(2 + 3).not.toBe(0);
	});
});
```
@[2-4](toBe: uses === to test exact equality)
@[5-9](toEqual: recursively checks every field of an object)
@[10-12](not: opposite of a matcher)

+++

```javascript
describe("Testing More Matchers", () => {
	it("Testing truthfulness", () => {
		const n = null;
	  expect(n).toBeNull();
	  expect(n).toBeDefined();
	  expect(n).not.toBeUndefined();
	  expect(n).not.toBeTruthy();
	  expect(n).toBeFalsy();
	});
	it("Testing number", () => {
		const n = 0.1 + 0.2;
	  expect(n).toBeGreaterThan(0);
		expect(n).toBeGreaterThanOrEqual(0);
		// Consideration for floating point
		expect(n).not.toBe(0.3);    // It isn't! Because rounding error
  	expect(n).toBeCloseTo(0.3); // This works.
	});
	// string: toMatch(regex)
	// array: toContain(element)
	// func: toBeCalled(), toHaveBeenCalledTimes(num)
});
```
@[2-9](Distinguish between udefined, null and false if needed)
@[10-17](Number ranges)
@[14-16](Testing floating point)
@[18-20](Even more matchers! check the API Reference)

---

### SNAPSHOT TESTING

Stores an image of an object used to check for differences in future tests executions

If the two images doesn't match the test fails

+++

- Makes an image of the object
- Compares it to the stored image
- If test fails
  - Show differences between images
  - If unexpected changes: Change code and test again
  - If expected changes: Update the stored image

+++

#### WHEN TO USE SNAPSHOTS

- Don't want to manually write expected value
- Testing UI and UI changes
- Testing reducers

+++

Remember this test

```javascript
describe("Add", () => {
	// 2+3 = 5
	it("Adds two numbers correctly", () => {
		expect(Add(2, 3)).toBe(5);
	});
});
```

We can do something like this

```javascript
describe("Add", () => {
	// 2+3 = snap
	it("Adds two numbers correctly", () => {
		expect(Add(2, 3)).toMatchSnapshot();
	});
});
```

+++

__/Add/test.js__
```javascript
describe("Add", () => {
	// 2+3 = snap
	it("Adds two numbers correctly", () => {
		expect(Add(2, 3)).toMatchSnapshot();
	});
});
```

__/Add/\_\_snapshots\_\_/test.js.snap__
```
exports[`Add Adds two numbers correctly 1`] = `5`;
```

--- 

### SNAPSHOT TESTING

##### TESTING COMPONENTS

+++

__/Button/index.js__
```javascript
import React from 'react';
import PropTypes from 'prop-types';

const Button = ({ onClick,  className, children }) => {
  return (
    <button 
    onClick={onClick}
    className={className}
    type='button' >
    {children}
    </button>
    );
}

Button.propTypes = {
  onClick: PropTypes.func,
  className: PropTypes.string,
  children: PropTypes.node.isRequired
}

Button.defaultProps = {
  className: ''
}

export default Button
```

+++

__/Button/test.js__
```javascript
import renderer from 'react-test-renderer';
describe("Button", () => {
	it("Renders", () => {
		const div = document.createElement('div');
	  ReactDOM.render(<Button>Button</Button>, div);
	});
	it('Snapshot click me button', () => {
		const component = renderer.create(<Button>Click me</Button>);
		const tree = component.toJSON();
		expect(tree).toMatchSnapshot();
	});
	it('Snapshot className button', () => {
		const tree = renderer.create(<Button className="awesome-btn">Click me</Button>).toJSON();
		expect(tree).toMatchSnapshot();
	});
});
```
@[3-6](Render component)
@[7-11](Snapshot testing)
@[12-15](Snapshot testing prop className)

+++

__/Button/\_\_snapshots\_\_/test.js.snap__
```
exports[`Button Snapshot click me button 1`] = `
<button
  className=""
  onClick={undefined}
  type="button">
  Click me
</button>
`;

exports[`Button Snapshot className button 1`] = `
<button
  className="awesome-btn"
  onClick={undefined}
  type="button">
  Click me
</button>
`;
```
@[1-8](No props assigned, just the children)
@[10-17](The snapshot adds the className value)

+++

### LETS PLAY

commit: 4fdebb2

---

### TESTING FUNCTIONS
##### TEST EVENTS AND COMPONENT LOGIC USING MOCKS AND SPYES

+++

#### MOCKS AND SPYES

- Replace an actual implementation of a function 
- Captures calls to a function (and parameters passed)
- Captures instances when <span class="reserved-word">new</span> or <span class="reserved-word">bind</span> is used
- Allow us to change the return of the function

+++

__/Button/test.js__
```javascript
it('Event onClick called only once', ()=>{
	const onClick = jest.fn();
	const component = renderer.create(<Button onClick={onClick}>Click me</Button>);
	const tree = component.toJSON();	
	expect(onClick).not.toBeCalled();
	tree.props.onClick();
	expect(onClick).toHaveBeenCalledTimes(1);
});
```
@[2](Mock function that is going to record every interaction)
@[5](The function hasn't been called yet)
@[6-7](Execute the function and expect it to have been called only 1 time)

+++

```javascript
it('Event onClick called only once', ()=>{
		const onClick = jest.fn();
		const component = renderer.create(<Button onClick={onClick}>Click me</Button>);
		const tree = component.toJSON();
		expect(onClick).not.toBeCalled();
		tree.props.onClick();
		tree.props.onClick('awesome-param');
		expect(onClick).toHaveBeenCalledTimes(2);
		expect(onClick.mock.calls).toEqual([ [], ['awesome-param'] ]);
	});
```
@[6-9](we can get the parameters)

---

### EVERYTHING SEEMS FINE

+++

```javascript
class Search extends Component {
  componentDidMount() {
    this.input.focus();
  }
  render() {
    const { value, onChange, onSubmit, children } = this.props;
    return (
      <form onSubmit={onSubmit}>
        <input  type='text'
                value={value}
                onChange={onChange}
                ref={(node) => { this.input = node; }} />
        <button type='submit'>
          {children}  
        </button>
      </form>
    );
  }
}

Search.propTypes = {
  value: PropTypes.string,
  onSubmit: PropTypes.func,
  onChange: PropTypes.func,
  children: PropTypes.node.isRequired
}
```

+++

```javascript
describe("Search", () => {
	it("Renders", () => {
		const div = document.createElement('div');
	  ReactDOM.render(<Search>Search</Search>, div);
	});
	it('Snapshot click me Search', () => {
		const tree = renderer.create(<Search>Search</Search>).toJSON();
		expect(tree).toMatchSnapshot();
	});
	it('Snapshot value Search', () => {
		const tree = renderer.create(<Search value="keys">Find</Search>).toJSON();
		expect(tree).toMatchSnapshot();
	});
});
```
@[1-14](commit: 70551b9)

+++

```javascript
function createNodeMock(element) {
  if (element.type === 'input') {
    return {
      focus() {},
    };
  }
  return null;
}
it('Snapshot click me Search', () => {
	const options = {createNodeMock};
	const tree = renderer.create(<Search>Search</Search>, options).toJSON();
	expect(tree).toMatchSnapshot();
});
```
@[1-8](mock context for ref elements)
@[10-11](create recives the createNodeMock as an option)

---

### Enzymme
##### A trip into the DOM

+++

```javascript
import { shallow, mount, render } from 'enzyme';
import { shallowToJson, mountToJson, renderToJson } from 'enzyme-to-json';
```

+++

```javascript
	it('Enzyme snapshot', () =>{
		const component = render(<Search>Search</Search>);
		const tree = renderToJson(component);
		expect(tree).toMatchSnapshot();
	});
```

```
exports[`Search Enzyme snapshot 1`] = `
<form>
  <input
    type="text" />
  <button
    type="submit">
    Search
  </button>
</form>
`;
```

+++

##### GOING DEEPER

+++

```javascript
it('Event onChange', () => {
	const onChange = jest.fn();
	const element = mount(<Search onChange={onChange}>Search</Search>);
  expect(onChange).not.toBeCalled();
	element.find('input[type="text"]').simulate('change');
	expect(onChange).toHaveBeenCalledTimes(1);
});
```
@[5](enzyme supports selectors jquery style)

+++

```javascript
it('Event onSubmit', () => {
	const props = { 
		onSubmit: jest.fn(),
		onChange: jest.fn(),
		value: "keys",
	};
	const element = mount(<Search {...props}>Find</Search>);
  expect(props.onSubmit).not.toBeCalled();
	element.find('form').simulate('submit');
	expect(props.onSubmit).toHaveBeenCalledTimes(1);
});
```
@[9](enzyme supports selectors jquery style)

---

### TEST COVERAGE

