---
# TESTING REACT APPS
### AN INTRODUCTION TO <span class="title-jest">JEST</span> AND <span class="title-enzyme">ENZYME</span>

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
		- Fastest Option
		- Backup by Facebook
	</div>
	<div class="half-col">
		<div class="col-header js-header">Cons</div>
		- Lack of documentation
	</div>
</div>

---
## TEST STRUCTURE
```javascript
describe('Component', () => {
	// alias for test(name, fn)
	it('Adds two numbers correctly', () => {
	  expect(/* data to test */).toBe(/* expected value */);
	});
})
```
@[1,7](describe)
@[3,6](test)
@[4-5](expects)

---
## LETS TESTS SOMETHING
```javascript
const Add = (x, y) => x + y;
```
<div class="fragment">
	- Adds two numbers correctly [2+3 = 5]
	- Commutative property [2+3 = 3+2]
	- Distributive property [1+(2+3) = (1+2)+3]
</div>

+++
```javascript
describe("Add", () => {
	// 2+3 = 5
	it("Adds to numbers correctly", () => {
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
### THE EXPECT OBJECT
