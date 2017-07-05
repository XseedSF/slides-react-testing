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
@[1,6](describe)
@[2,5](test)
@[4](expects)

---

### LETS TESTS SOMETHING
```javascript
const Add = (x, y) => x + y;
```
<div class="fragment">
	- Adds two numbers correctly [2+3 = 5] <br/>
	- Commutative property [2+3 = 3+2] <br/>
	- Distributive property [1+(2+3) = (1+2)+3]
</div>

+++

```javascript
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
@[2-5](Add)
@[6-9](Commutative)
@[10-13](Distributive)

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
@[2-4](Uses === to test exact equality)
@[5-9](Recusively checks every field of an object)
@[10-12](Opposite of a matcher)

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

### Snapshot testing

Stores an image of an object to compare against in future tests executions
If the two images doesn't match the test fails

+++

- Makes an image of the object
- Compares it to the stored image
- If test fails
  - Show differences between images
  - If unexpected chages: Change code and retest
  - If expected changes: Update the stored image

+++

If we dont whant to write the expected result

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

```javascript
describe("Add", () => {
	// 2+3 = snap
	it("Adds two numbers correctly", () => {
		expect(Add(2, 3)).toMatchSnapshot();
	});
});
```

\_\_snapshots\_\_/test.js.snap
```
exports[`Add Adds two numbers correctly 1`] = `5`;
```
