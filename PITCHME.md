---
#Testing React Projects
###a simple guide for testing in the react ecosystem

---
##Needs and toolkit
- Test structure (Jest, Mocha, Jasmine)
- Run tests, display results (Jest, Mocha, Jasmine, Karma)
- Make assertions (Chai, Jest, Jasmine)
- Mockups, spies (Sinon.JS, Jest, Jasmine)
- Snapshots (Enzyme, React Test Utils)
- Code coverage reports (Istambul)

---
###Enviroments
<div>
	<div class="half-col" >
		<div class="col-header official-header">Official Approach</div>
		Jest + React Test Utils
	</div>
	<div class="half-col">
		<div class="col-header js-header">JS Approach</div>
		Mocha + Chai + Sinon + Enzyme
	</div>
</div>
<div class="half-col fragment">
	<div class="col-header react-header">React Approach</div>
	Jest + Enzyme
</div>

---
##Jest

---
###Test structure
```javascript
describe('Button', () => {
	// alias for test(name, fn)
	it('Renders component', () => {
	  const div = document.createElement('div');
	  ReactDOM.render(<Button>Button</Button>, div);
	});
})
```
@[1,7](describe)
@[3,6](test)
@[4-5](test code)