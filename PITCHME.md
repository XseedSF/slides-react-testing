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
<div>
	<div class="half-col" >
		<div class="col-header official-header">Pros</div>
		- Fastest option <br/>
		- Integral solution (almost) <br/>
		- Supports snapshot testing <br/>
		- Supported by Facebook 
	</div>
	<div class="half-col">
		<div class="col-header js-header">Cons</div>
		- Lack of documentation
	</div>
</div>

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
@[5-9](toEquals: recursively checks every field of an object)
@[10-12](not: opposite of a matcher)

+++

```javascript
describe("Testing More Matchers", () => {
	it("Testing truthiness", () => {
		const n = null;
	  expect(n).toBeNull();
	  expect(n).toBeDefined();
	  expect(n).not.toBeUndefined();
	  expect(n).not.toBeTruthy();
	  expect(n).toBeFalsy();
	});
	it("Testing number", () => {
		const n = 0.1 + 0.2;
	  expect(value).toBeGreaterThan(0);
		expect(value).toBeGreaterThanOrEqual(0);
		// Consideration for floating point
		expect(value).not.toBe(0.3);    // It isn't! Because rounding error
  	expect(value).toBeCloseTo(0.3); // This works.
	});
	// string: toMatch(regex)
	// array: toContain(element)
});
```
@[2-9](Distinguish between udefined, null, false if needed)
@[10-17](Number ranges)
@[14-16](Testing floating point)
@[18-19](Even more matchers! check the API Reference)

---

### SNAPSHOT TESTING

##### INTRODUCTION

Stores an image of an object to compare against it in future tests executions

If the two images doesn't match the test fails

+++

- Makes an image of the object
- Compares it to the stored image
- If test fails
  - Show differences between images
  - If unexpected chages: Change code and test again
  - If expected changes: Update the stored image

+++

#### WHEN TO USE SNAPSHOTS

- Dont want to manualy update expected value
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
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
import Button from '.';

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
@[7-10](Render component)
@[12-16](Snapshot testing)
@[12-16](Snapshot testing prop className)

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
@[10-16](The snapshot adds the className value)